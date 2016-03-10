---
layout: post
title: "Change github blog to jekyll bootstrap"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Trying out [Jekyll Bootstrap](https://github.com/plusjade/jekyll-bootstrap/) because it contains comments, categories and other utilities such as analytics.

Came across some problems with this plugin, it could pick up the bootstrap-3 theme. i.e Some links to the bootstrap js and css files were wrong "/assets/themes//bootstrap/css/bootstrap.min.css"  instead of "/assets/themes/bootstrap-3/bootstrap/css/bootstrap.min.css".

To fix change _includes/JB/setup  
<pre>
from
page.theme.name
to
layout.theme.name 
</pre>

Also change _config.xml and set ASSET_PATH to "ASSET_PATH : /assets/themes/bootstrap-3"

This was due to [jekyll upgrade](http://jekyllrb.com/docs/upgrading/2-to-3/). 
For more information on how Jekyll Bootstrap works [Jekyll QuickStart Guide](http://jekyllbootstrap.com/usage/jekyll-quick-start.html).

