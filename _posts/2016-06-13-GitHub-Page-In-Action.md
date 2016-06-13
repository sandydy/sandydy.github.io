---
published: false
layout: post
date: {}
tags:
  - jekyll
  - blog
  - disqus
  - social share buttom
comments: true
signature: true
---
The problem statement is still blog related. I managed to set up ghost as my blog with a VPS on cloud. I was, however, feeling uncomfortable with backup, restore, resilence and so on. Obviously those are all **PAID** service. I was later looking at cheap alternatives. I ended up found [GitHub Page](https://pages.github.com/) and [GitLab Page](https://gitlab.com/groups/pages). Both of which supports jekyll and free hosting. Why not? In the meantime, the blog post covers the mirgration paths, custom domain setting, social share buttons setup...

I am a good fan to open source technologies and so as to markdown language. I found [wordpress](https://wordpress.org/) too bulky and [ghost](https://ghost.org/) simple to use. The free blogging platform such as blogger do not fit for developer... I was, then, looking for a platform good fit for developer. I can customize the blog as I'd like. I have control to the styling and development. In the meantime, the platform is blogger friendly. 

In terms of blog reader, same as I do, what I expect is [google](http://www.google.com) to help me locate the target. And the blog is easy to read... As a blogger, what I need are ...

1. Text editor easy to edit, insert image, highlight codes, simple list items, tables, etc. (Markdown simply speaking)
2. Easy maintainance to the content, I want to write and publish, no more complicated steps
3. One platform, I don't need to move between tools to create one blog
4. Theming, support flexibility to allow me to modify the look and feel
5. Always ON with **NO CHARGES**, this is particular important

What I imagine actually *does not exist*. Instead, a close to reality solution is GitHub/GitLab pages which many people are using to host their web site. The tools I've been using are ...

1. Jekyll, static web page generation in ruby...
2. [prose.io](http://prose.io/) as a editor to your github page. `Note: Prose.io support is the main reason I ended up with GitHub page`

The combination works prefectly fine. It is just close to [ghost](http://ghost.org). I spent good amount of time to explore the possiblity and turns out work fine. `Either you pay for service or you spent time to save pennies` 

The theme I use is [Hyde](https://github.com/sandydy/hyde-on-docker). I made small tweaks on top of [Mark Otto](https://github.com/mdo)'s [original](https://github.com/poole/hyde) 
![Mark Otto](/public/images/2016/06/13/BlogCreation/poolehyde.jpg)

and [Rhadow](https://rhadow.github.io)
![Rhadow](/public/images/2016/06/13/BlogCreation/rhadowblog.jpg)
**Thanks both of them.**

There are several challenges. First of all, I'd need a starting point. Hyde theme is good, but too simple. Rhadow's theme is good, but too much tailor made. I turn out clone from Rhadow's blog and start from there. In which, my blog already have...

`Fully functional jekyll repository, from styling, post, tagging, paginations, commenting, signature, google analytics, facebook like and share...`

If you are new to GitHub page, you could refer to [link](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)

1. extracted all configuration items to `_config.yml`. For instance, twitter account number, web page domain names, etc...
2. setup docker-compose.yml file to allow local development. Please read my another [blog post](/2016/06/08/new-experience-docker-on-windows/) for details
3. added social share button supports
4. CNAME support to point http://blog.sandydy.com to http://sandydy.github.io

It turns out a couple of features surprised me 
# Disqus offers commenting feature to remote web page
[Disqus](https://disqus.com/) offers [embeded scripts](https://disqus.com/features/) to your web page, after I injected to my blog. Users can read blog post in my page and comment to disqus underneath. At the end, my web page doesn't need to have a backend server to handle interactions between.

![disqus.jpg](/public/images/2016/06/13/BlogCreation/disqus.jpg)

My comments blog post comment are like ...
![Sandy's Blog - Disqus Comment](/public/images/2016/06/13/BlogCreation/disquscomment.jpg)

The tool also offers comment count as ...
![Disqus comment count](/public/images/2016/06/13/BlogCreation/disquscommentcount.jpg)

# Social apps share buttons
Most of which provides code snippets for you to easily embeded their share button to your web site. However, the styling is killing. the buttons goes up and down and doesn't align with each other.

- [Facebook](https://developers.facebook.com/docs/plugins/share-button)
- Twitter [Tweet](https://dev.twitter.com/web/tweet-button)
- Twitter [Follow](https://dev.twitter.com/web/follow-button)
- [LinkedIn](https://developer.linkedin.com/plugins/share)
- [GPlus +1](https://developers.google.com/+/web/+1button/)

you may find this [article](http://stackoverflow.com/questions/4770372/misalignment-of-facebook-twitter-buttons) useful when you are dealing with alignment issue between the share buttons

After I've implemented all the above, I realised that there are package solution to offer even more share options...
For instance, [Sharrre](http://sharrre.com/#demos)

# Custom domain name support
I had registered my domain name (sandydy.com) early the year with [GoDaddy](http://godaddy.com) at USD$1 first year. I surely want the domain points to my blog at github automatically...

as a result
[blog.sandydy.com](http://blog.sandydy.com) --> [sandydy.github.io](http://sandydy.github.io)
![GitHub Page CNAME SubDomain](/public/images/2016/06/13/BlogCreation/blogsandydycom.jpg)

This could be done just a few simple steps, please refer to this [article](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) for details. Tips: you may find this [article](https://blog.longwin.com.tw/2010/12/windows-clear-dns-cache-2010/) useful to clear your cache to speed up the domain name registration updates.

# Migration
GitHub Page uses Jekyll static web page generation tool, Jekyll provides blog migration from variety existing blog platform. Please refer to this [article](https://jekyllrb.com/docs/migrations/) for details.

I've tried both [*Ghost -> Jekyll*](http://import.jekyllrb.com/docs/ghost/) and [*Blogger -> Jekyll*](http://import.jekyllrb.com/docs/blogger/), both works even though each of which requires small tweaks. For instance, 

1. Ghost post migration does not migrate tags nor images. 
2. Blogger migration result are htmls, 
3. ... 

Feel free to reach me for issues you might encounter in migration.

# Conclusion
The easiest path to create your own blog definitely is to use packaged blogging platform like ghost / wordpress. And you can find lots of themes and resources against. However, as a developer, we tend to go for flexibility and turns out GitHub Page a reasonable choice to me. Even though I'd spent almost 4 days to setup everything up...

For beginers to GitHub Page, you may refer to this [article](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) to learn more.
