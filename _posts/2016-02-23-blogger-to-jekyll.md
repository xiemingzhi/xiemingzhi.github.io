---
layout: post
title: Blogger to Jekyll
---


I wanted to see if it was possible to import blogger posts into github blog.<br>
Found this tool that makes it simple to do so:<br>
[jekyll-import](https://github.com/jekyll/jekyll-import)

Instructions:<br>
1. install the gem
  gem install jekyll-import
<br>
2. export your posts from blogger
![Blogger export screenshot]({{ site.url }}/assets/blogger-export.png)
download the resulting xml to another folder
<br>
3. execute the importer
{% highlight ruby %}
ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::Blogger.run({
      "source"                => "blog-02-21-2016.xml",
      "no-blogger-info"       => false, # not to leave blogger-URL info (id and old URL) in the front matter
      "replace-internal-link" => false, # replace internal links using the post_url liquid tag.
    })'
{% endhighlight %}
This will create a _posts directory with all your posts.
<br>
4. Copy the posts to your github blog _posts directory and submit it to github
<br>

