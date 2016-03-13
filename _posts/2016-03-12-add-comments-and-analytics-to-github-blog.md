---
layout: post
title: "Add comments and analytics to github blog"
description: ""
category: 
tags: []
---
{% include JB/setup %}

After investigating different comment system [Comment System Comparison](http://www.htpcbeginner.com/disqus-vs-livefyre-vs-facebook-comments/) decided to use [Disqus](https://disqus.com/home/settings/account/).

To use apply for account then setup website, use the website name as id in "_config.yml_" also set the provider to disqus.

For [google analytics](https://analytics.google.com) use your google account to apply for domain and hostname this will provide you with a tracking id, put this in "config.yml" and specify google as your analytics provider.



