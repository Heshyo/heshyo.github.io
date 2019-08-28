---
layout: post
title: How to create a blog with GitHub Pages and Jekyll Part 2
---

How to use Jekyll for your site/blog on GitHub Pages
----------------------------------------------------

[GitHub Pages](https://pages.github.com/) uses [Jekyll](https://jekyllrb.com/), "a simple, blog-aware, static site generator", so let's install it.

### New way to install it

Since the creation of the post, it seems the recommended way to install Jekyll on Windows changed.

1.	Install the [RubyInstaller for Windows](https://rubyinstaller.org/) (get the recommended Devkit version if you're unsure). Install MSYS2 at the end of the wizard.
2.	Install Jekyll and bundler gems

	```gem install jekyll bundler```

3.	Create your new site (below named _mysite_) with 

	```jekyll new mysite```

	This creates a director name _mysite_

4.	Go to the directory and build the site

	```
	cd mysite
	bundle exec jekyll serve
	```

5.	You should see that your site is available at <http://localhost:4000>

### If you're on Linux or Mac
1.	Install [Ruby](https://www.ruby-lang.org/), see more [here](https://www.ruby-lang.org/en/downloads/)
2.	Install [Gem](https://rubygems.org/), the Ruby package manager, see more [here](https://rubygems.org/pages/download)

### If you're on Windows

1.	Install chocolatey (a package manager for Windows, similar to apt-get on Linux)

    Run in command line as admin:

    ```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

    To upgrade chocolatey in the future, run

    ```choco upgrade chocolatey```

2.	Install ruby through chocolatey

    Open a new command line (not as admin) - you need to open a new command line, else chocolatey will not be available

    ```choco install ruby -y```

    To upgrade ruby later on, run (it seems it needs admin rights)

    ```choco upgrade ruby```


### All systems
Now you just need to install Jekyll through Gem, the Ruby package manager
Open a new command line (not as admin) - you need to open a new command line, else gem will not be available

```
gem install jekyll
```

After installing jekyll you may see:

> MSYS2 could not be found. Please run 'ridk install'
or download and install MSYS2 manually from https://msys2.github.io/

This is needed for installing gems with native extensions.

To upgrade jekyll later on, run

```gem update jekyll```

Now that Jekyll is installed, simply run the following at the root of your GitHub Pages repository

```
jekyll serve
```

It will create your static website and open a local webserver for you to test. Just head over to <http://127.0.0.1:4000/> (or whatever address and port number is displayed by the ```jekyll serve``` command), and you should see the index of your website.

Check [part 3](/2016/06/11/how-to-create-a-blog-with-github-pages-and-jekyll-part-3.html) to write your first post.