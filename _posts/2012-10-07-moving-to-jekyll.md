---
layout: post
title: Moving to Jekyll, and Failed Migration to Azure
description: Some Description
date: 2012-10-07 16:01:22 +11:00
tags: "jekyll, blogging, meta"
---

tldr; 
--

This is just an FYI that I am currently migrating from [Wordpress](http://wordpress.org) to [Jekyll](https://github.com/mojombo/jekyll). Getting to a point where I am happy may take a week or two.

Back story
--

Recently, I have been getting really slow speeds on [Dreamhost](http://dreamhost.com/), and have quite honestly had enough of it. Also, the hosting has been provided to me free of charge for years, and I thought it was about time to stop mooching. 

The initial plan was to move over to [Windows Azure Web Sites](https://www.windowsazure.com/en-us/home/scenarios/web-sites/) on a [Pay-As-You-Go](https://www.windowsazure.com/en-us/pricing/calculator/) plan. I had some success migrating it to Azure, but I was having issues getting my domain working the way that I had it. Azure absolutely refused to recognise my www subdomain (it would 404 while the TLD worked fine).

I then discovered that my Wordpress installation had been infected my malware, and given this is the second time in many months that this has occurred (while running the absolute latest version), my frustration lead me to looking at alternative options. 

Jekyll
--

I had been toying with the idea of running a Jekyll blog for a while, and honestly I don't think I need any more complexity than that. I could use Azure for it and pay whatever miniscule bandwidth costs that come with it, but then I discovered that you can run a Jekyll blog straight from [Github Pages](http://pages.github.com/) for free. Sign me up!

I'm also using [Jekyll-Bootstrap](http://jekyllbootstrap.com/), which makes the process of theming my site easier, considering I have the visual prowess of a frog.

In Conclusion
--

Wordpress is completely gone, but I haven't quite yet mastered my Jekyll installation, so please bare with me while I work out the kinks and actually decide what I want the blog to look like post-wp. 