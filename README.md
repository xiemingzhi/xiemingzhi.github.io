---
Title: This is my technology ravings.
---

# Ming's technology blog

The title element overrides the one in index.html.

# New install instructions

```cmd
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"  
choco install ruby -y  
continue to jekyll install
```

---

Originally forked from [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)  
Code at [jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap/)

# Old install instructions

Install ruby, jekyll requires Ruby version >= 2.0.0. If you are using windows install [uru](https://bitbucket.org/jonforums/uru/wiki/Downloads) for multiple versions of ruby.

```cmd
C:\tools>uru_rt admin install
uru admin add C:\ruby193\bin
uru admin add C:\ruby22\bin
uru ls
uru 224
```

---

Install jekyll:

```ruby
gem install jekyll
gem install bundler
bundle install
```

Create new post
```cmd
# Rakefile
# Usage: rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]] [category="category"]
rake post title="AngularJS project to call RESTful service" date="2016-03-13"
```

Help file for running jekyll [jekyll quickstart](http://jekyllrb.com/docs/quickstart/)
```ruby
bundle exec jekyll build
bundle exec jekyll serve
```
or just 
```ruby
jekyll serve
```
If you meet the problem command not found python  
solution open new cmd

This is an example of syntax highlighting.  
Github.io blogging supports rouge (_config.xml).  
Github repository supports Pygments (README.md).

```java
public class Hello {
	public static void main(String args[]) {
		System.out.println("Hello World");
	}
}
```

