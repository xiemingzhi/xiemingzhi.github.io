---
Title: This is my technology ravings.
---

# Ming's technology blog

The title element overrides the one in index.html.

---

# New install instructions

## Chocolatey The package manager for Windows.

```cmd
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"  
open administrator powershell 
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco install ruby -y  
Ruby is going to be installed in 'C:\tools\ruby25'
Installing 64-bit ruby...
ruby has been installed.
  ruby can be automatically uninstalled.
Environment Vars (like PATH) have changed. Close/reopen your shell to
 see the changes (or in powershell/cmd.exe just type `refreshenv`).
 ShimGen has successfully created a shim for rubyinstaller-2.5.3-1-x86_x32.exe
 The install of ruby was successful.
  Software installed to 'C:\tools\ruby25\'

Chocolatey installed 1/1 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
continue to jekyll install
```
restart cmd check version 
ruby -v 

to uninstall

```cmd 
choco uninstall ruby
```

to uninstall chocolatey
Uninstalling Chocolatey
Should you decide you don't like Chocolatey, you can uninstall it simply by removing the folder (and the environment variable(s) that it creates). Since it is not actually installed in Programs and Features, you don't have to worry that it cluttered up your registry (however that's a different story for the applications that you installed with Chocolatey or manually).

Folder
Most of Chocolatey is contained in C:\ProgramData\chocolatey or whatever $env:ChocolateyInstall evaluates to. You can simply delete that folder.

NOTE You might first back up the sub-folders lib and bin just in case you find undesirable results in removing Chocolatey. Bear in mind not every Chocolatey package is an installer package, there may be some non-installed applications contained in these subfolders that could potentially go missing. Having a backup will allow you to test that aspect out.

---

## Install jekyll:

```ruby
open new cmd.exe 
gem install jekyll
gem install bundler
bundle install
```
   
### Create new post

```cmd
bundle exec octopress new post "AngularJS project to call RESTful service" 
octopress new post "AngularJS project to call RESTful service"
```

Help file for running jekyll [jekyll quickstart](http://jekyllrb.com/docs/quickstart/)

```ruby
bundle exec jekyll build
bundle exec jekyll serve
```

### Syntax

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

---

# Troubleshooting

## problem  
bundle install  
Gem::RuntimeRequirementNotMetError: ffi requires Ruby version >= 2.0, < 2.6  
## solution  
bundle update  

## problem  
MSYS2 could not be found. Please run 'ridk install'  
or download and install MSYS2 manually from https://msys2.github.io/  
## solution
use windows cmd dont use git bash  
ridk install  
1. - MSYS2 base installation
2. - MSYS2 system update (optional)
3. - MSYS2 and MINGW development toolchain
choose 3 installs to c:\msys64

## problem
```ruby  
or just  
jekyll serve  
C:/tools/ruby26/lib/ruby/2.6.0/bundler/runtime.rb:319:in `check_for_activated_spec!': You have already activated public_suffix 4.0.1, but your Gemfile requires public_suffix 3.1.1. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
```  
## solution
add `bundle exec`

## problem
If you meet the problem command not found python  
## solution 
open new cmd

## problem
bundle exec jekyll serve 
```
Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
                    Invalid CP950 character "\xE2" on line 54
jekyll 3.8.5 | Error:  Invalid CP950 character "\xE2" on line 54
```
## solution 
https://jekyllrb.com/docs/installation/windows/#encoding  
EncodingPermalink  
If you use UTF-8 encoding, make sure that no BOM header characters exist in your files or very, very bad things will happen to Jekyll. This is especially relevant when you’re running Jekyll on Windows.  
Additionally, you might need to change the code page of the console window to UTF-8 in case you get a Liquid Exception: Incompatible character encoding error during the site generation process. It can be done with the following command:
```
chcp 65001
```

---

# Old install instructions

Originally forked from [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)  
Code at [jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap/)

Install ruby, jekyll requires Ruby version >= 2.0.0. If you are using windows install [uru](https://bitbucket.org/jonforums/uru/wiki/Downloads) for multiple versions of ruby.

```cmd
C:\tools>uru_rt admin install
uru admin add C:\ruby193\bin
uru admin add C:\ruby22\bin
uru ls
uru 224
```

### Create new post

```cmd
# Rakefile
# Usage: rake post title="A Title" [date="2012-02-09"] [tags=[tag1,tag2]] [category="category"]
rake post title="AngularJS project to call RESTful service" date="2016-03-13"
```

### Create new page 

```cmd
# Rakefile
# Usage: rake page name="about.html"
```

Help file for running jekyll [jekyll quickstart](http://jekyllrb.com/docs/quickstart/)

```ruby
bundle exec jekyll build
bundle exec jekyll serve
```

