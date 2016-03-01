---
Title: This is my technology ravings.
---

# Ming's technology blog

The title element overrides the one in index.html.<br>

Install ruby, jekyll requires Ruby version >= 2.0.0. If you are using windows install [uru](https://bitbucket.org/jonforums/uru/wiki/Downloads) for multiple versions of ruby.

<pre>
C:\tools>uru_rt admin install
uru admin add C:\ruby193\bin
uru admin add C:\ruby22\bin
uru ls
uru 224
</pre>

Install jekyll:

```ruby
gem install jekyll
```

Help file for running jekyll [jekyll quickstart](http://jekyllrb.com/docs/quickstart/)<br>
```ruby
bundle exec jekyll build
bundle exec jekyll serve
```
or just 
```ruby
jekyll serve
```
If you meet the problem command not found python<br>
solution open new cmd<br>

This is an example of syntax highlighting.<br>
Github.io blogging supports rouge (_config.xml).<br>
Github repository supports Pygments (README.md).<br>

```java
public class Hello {
	public static void main(String args[]) {
		System.out.println("Hello World");
	}
}
```

