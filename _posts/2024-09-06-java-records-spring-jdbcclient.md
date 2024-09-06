---
date:   2024-09-06 08:00:00 +0200
image:  /img/jdbc.png
tags:   [Java, Spring, Database]
---

## Java Records and Spring JdbcClient

Much has been written about [Java Records](https://docs.oracle.com/en/java/javase/14/language/records.html) not working well, or at all, in combination with [JPA](https://www.oracle.com/java/technologies/persistence-jsp.html) due to their immutability. But how about using Java Records in combination with [Spring](https://spring.io/projects/spring-framework)'s [JdbcClient](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/core/simple/JdbcClient.html)? Let's dive in and take a look!

## Why did I do this?

I was working on a sample application for a different purpose and needed a quick way to store and retrieve some simple data. To keep things as lean and with as little boilerplate code or overhead as possible, I wanted to use Java Records over POJO's + [Lombok](https://projectlombok.org/) (or actually writing all the boilerplate code, yuck!). I decided to give JdbcClient a go and see where I would end up.

I had very low expectations of success due to all the publications and horror stories around Java Records and persistence, so I was pleasantly surprised to learn that Java records work with JdbcClient and [RowMapper](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/core/RowMapper.html) just fine, and it's fairly simple too! Note that I did not implement common functionality such as transaction management, input validation, etc. to keep the example clean, but it should not be a problem to add later.

For those who just want to check out the entire app and code, it's on [GitHub](https://github.com/NLxAROSA/jdbcrecordsdemo).

## Configuration

#### Using an embedded H2 database

To configure H2 as an embedded database I'll add the H2 dependency to `pom.xml` and provide the following configuration in `application.properties`. Spring Boot will create a `DataSource` for H2 and autowire it in any `JdbcClient` that I inject into the constructor of one of my Spring beans. In order to make that possible I also added the [Spring Boot starter](https://github.com/spring-projects/spring-boot/tree/main/spring-boot-project/spring-boot-starters) for JDBC.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
  <groupId>com.h2database</groupId>
  <artifactId>h2</artifactId>
  <scope>runtime</scope>
</dependency>
```

#### Database configuration properties

The following configuration tells spring what the connection URL, driver class and credentials are for setting up the `DataSource`.

BTW, did you know that H2 comes with a full-blown database explorer called [H2 Console](https://www.h2database.com/html/tutorial.html#tutorial_starting_h2_console) that lives inside your web-app? Not using it for this example, but am using it in [this one](https://github.com/NLxAROSA/zoom-calendar-vsdk-example), and it's a very nice and powerful way to quickly check 'under the hood' of your database, schemas, settings and data while developing and debugging. Give it a try!

```properties 
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=${DATABASE_USER:databaseusernotset}
spring.datasource.password=${DATABASE_PASSWORD:databasepasswordnotset}
spring.h2.console.enabled=false
```

Note that I am setting database user and password values from the values of environment variables. By using the `${ENV_VAR_NAME:somedefaultvalue}` format you tell Spring to populate the value of the property from the value of the environment variable, or use the hardcoded 'defaultvalue' if the environment variable is not set. This prevents you from committing credentials to Git (or other SCM) and will also result into immediate failure when deploying into an environment where the variables are not set (as opposed to a potentially working application if you put your development environment database server properties in there). Note that with an embedded h2, the above actually works as it will simply create the admin user with that name and password.

In my sample apps I usually provide a `.env.example` file with exports for the environment variables and placeholders for the values and a `.gitignore` with `.env` line in it. I then instruct users to create a `.env` file, populate it with their local values, then run the app using 
```bash 
source .env && ./mvnw spring-boot run
```
This will help consumers of your repo prevent accidentally committing credentials. For the example app I did not provide any and let it use the defaults, assuming that you will not use my app with its embedded H2 database in production.

#### Bootstrapping the database
H2 will automatically execute statements from `schema.sql` to create your database schema on application start. Afterwards, it will also automatically execute any (insert) statements from `data.sql` to provide any bootstrap data your app may require. Put these files under `src/main/resources` For this example, it's a rather simple schema with one table as defined below.
```sql
CREATE TABLE scheduled_session (
  session_id BIGINT AUTO_INCREMENT PRIMARY KEY,
  start_date TIMESTAMP NOT NULL,
  session_name VARCHAR(200) NOT NULL,
  passcode VARCHAR(50) NOT NULL
);
```
In `data.sql` I provide one row of test data that I use during verification of the tests (yes, ideally that should go under `src/test/resources` and not along with the bootstrap data).

## Code
I want to store a 'scheduled session' along with its name, passcode, and start date/time and retrieve it at a later time using its name. Let's take a look at what we will need to accomplish that.

#### A Java Record to act as a value object
I start off with a fairly simple Java Record to act as my value object. A single record with two [String](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/String.html) properties and a [LocalDateTime](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/time/LocalDateTime.html) property. You can of course make this more complex and with more layers if you want/need, e.g. when working with multiple entities and relationships between them. Records certainly do not restrict you to flat structures.
```java
public record ScheduledSession(String sessionName, String passCode, LocalDateTime startDate) {}
```
Pretty bare bones, isn't it? No code, no annotations, just a record definition and that's it. (Yes, no JavaDoc either, I know. ;))

#### Some SQL statements
For the persistence part I'll need to define some (parameterized) SQL statements for the insert and select functionality. For this example I'll just store these in a String constant, but feel free to maintain these in separate SQL files in your own apps as this may make testing your SQL in isolation easier.
```java
private static final String INSERT_SQL = "insert into scheduled_session (start_date, session_name, passcode) values (:startDate, :sessionName, :passCode)";
private static final String SELECT_SQL = "select session_name, start_date, passcode from scheduled_session where session_name = :sessionName";
```

#### Inserting using JdbClient
To fill the parameters with actual values and execute the SQL I'll use Spring's JdbcClient. The method returns the number of rows that are affected, which in the case of our insert, should always be 1.
```java
public int insertScheduledSession(ScheduledSession scheduledSession) {
    return jdbcClient
            .sql(INSERT_SQL)
            .param("startDate", scheduledSession.startDate())
            .param("sessionName", scheduledSession.sessionName())
            .param("passCode", scheduledSession.passCode())
            .update();
}
```

#### Selecting using JdbClient and Optional
When retrieving a scheduled session by its name, I'm taking into account the possibility of the session not existing. So instead of returning a ScheduledSession outright, I'll return an [Optional](https://docs.oracle.com/en%2Fjava%2Fjavase%2F22%2Fdocs%2Fapi%2F%2F/java.base/java/util/Optional.html) that may or may not contain a ScheduledSession instead.
```java
public Optional<ScheduledSession> getScheduledSession(String sessionName) {
    return jdbcClient
            .sql(SELECT_SQL)
            .param("sessionName", sessionName)
            .query(rowMapper).optional();
}
```
#### Mapping ResultSet to your Java Record
Since I'm dealing with plain JDBC, I don't have a framework like Hibernate to handle row-to-object mapping for me 'automagically'. In order for me to map a row to an object I have to provide a RowMapper that maps the values of the ResultSet onto a new ScheduledSession record.
```java
private RowMapper<ScheduledSession> rowMapper = (rs, rowNum) -> new ScheduledSession(
            rs.getString("session_name"),
            rs.getString("passcode"), 
            rs.getTimestamp("start_date").toLocalDateTime());
```

You could eliminate this code altogether by using a [DataClassRowMapper](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/core/DataClassRowMapper.html) instead of a custom one, at the expense of performance and being forced to map all your columns (which I, in this particular case, don't want to).

And that's all there is to it. Checkout the full and working source code [on GitHub](https://github.com/NLxAROSA/jdbcrecordsdemo). It contains all this code and a few tests to verify that it actually all works.

## Conclusion

While Java Records and JPA are not a match made in heaven, consider whether you really need 'big' frameworks such as [Spring Data](https://spring.io/projects/spring-data), JPA or others. For small and simple apps such as micro-services or micro-liths, good 'ole [JDBC](https://docs.oracle.com/en/java/javase/22/docs/api/java.sql/module-summary.html) + Java Records may be all you need. It works well and results in clean, readable code with little magic under the hood (though you will always swipe some complexity and boilerplate under the hood).