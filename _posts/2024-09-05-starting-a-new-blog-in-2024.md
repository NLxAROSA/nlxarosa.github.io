---
date:   2024-09-05 08:00:00 +0200
image: /img/newblog.png
---

## Starting a new blog in 2024

<img align="left" src="/img/newblog.png" width="64"/>Starting a new blog in 2024 (or reviving an old one) can be an intimidating challenge. There are so many options to choose from, with varying levels of costs and complexity. So how did I go about it?

### GitHub Pages

GitHub offers a free service calles [GitHub Pages](https://pages.github.com/). You create a repository named `yourusername.github.io`, put an index.html in there, and you have a working website at https://yourusername.github.io. This is pretty neat and very simple. You can add your own [custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) there as well, and GitHub will handle TLS certificates for you via [Let's Encrypt](https://letsencrypt.org/). If you already own a domain, it's a matter of adding an A-record and and specifying the domain name to GitHub. I started with zero knowledge of the above and had this setup in 10 minutes.

### Jekyll

So you have a working (though very small) website. That's not a blog though. Luckily, GitHub Pages supports [Jekyll](https://jekyllrb.com/) for building your website, and Jekyll does support blogging. You can follow their [Quickstart](https://jekyllrb.com/docs/) guide, which is great to begin with, but if you don't want to deal with Ruby at all you can also go with this [template repo](https://github.com/chadbaldwin/simple-blog-bootstrap) from [Chad Baldwin](https://chadbaldwin.net/), who also wrote a [nice blog post](https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html) on how to set up your own blog in minutes. Works great and it's easy to backport the Ruby stuff in case you ever want to.

### Writing posts

Writing posts is a matter of writing a [GitHub Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) file and committing/pushing it to the repo. The basic result is what you see here. Thing for me to remain to do is add my [Bluesky profile](https://bsky.app/profile/larsrosenquist.bsky.social) link to the bottom of the page and better styling/alignment for images and then I'm done. Easier and faster than installing Wordpress and a bunch of plugins!

### My philosophy

Keep it simple. Personally I'm tired of all the 'social' networks and the walled gardens they create. I'm using [Mastodon](https://mastodon.social/@larsrosenquist) more and more these days as well as dedicated forum websites for my hobbies and other interests. So no social sharing links on my blog (though I made it easy to copy the link of each article, did you notice?), no ads, no analytics, no data farming, no needlessly verbose content to boost SEO, just plain content, like a psycho from the 90s/00s. ;)
