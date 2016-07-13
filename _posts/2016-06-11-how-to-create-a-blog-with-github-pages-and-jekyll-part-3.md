---
layout: post
title: How to create a blog with GitHub Pages and Jekyll Part 3
---

How to create you first Jekyll post
-----------------------------------

1.	Create a ```_posts``` folder at the root of your repository
2.	Create a file matching the following pattern ```YEAR-MONTH-DAY-title.MARKUP```, eg ```2016-06-11-this-is-my-first-post.md```. ```md``` means [Markdown](https://daringfireball.net/projects/markdown/), which is what's used on [GitHub](https://github.com)
3.	Starts that file with a [Front Matter](https://jekyllrb.com/docs/frontmatter/) block, like this-is-my-first-post

	```Markdown
	---
	the front matter content
	---
	```

	The front matter content is following the [YAML](http://yaml.org/) format, which basically means something like

	```YAML
	key1: value1
	key2: value2
	```
	In our case, we'll use the following

	```
	---
	layout: post
	title: The title of my awesome post!
	---
	```

4.	Below, simply write your content!
	Markdown is a pretty simple format to use. You can check <https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet> for samples

5.	Save it. If you haven't already done so, run ```jekyll serve```. This will generate the corresponding html file under  ```_site```. Head over to <http://127.0.0.1:4000/year/month/day/name-of-your-awesome-post.html> (or whatever address and port number is displayed by the ```jekyll serve``` command). Note that the extension will be ```html```.

Now you'll probably noticed that your post is not really sexy...so head over to the [next part](/2016/06/13/how-to-create-a-blog-with-github-pages-and-jekyll-part-4.html) to fix that.