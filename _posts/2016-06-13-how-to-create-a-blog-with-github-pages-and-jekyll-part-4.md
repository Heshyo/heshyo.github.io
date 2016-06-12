---
layout: post
title: How to create a blog with GitHub Pages and Jekyll Part 4
---

Make your post more sexy by using a nice template
-------------------------------------------------

You may have noticed in the command window where ```jekyll serve``` is running something like

```
Build Warning: Layout 'post' requested in _posts/year-month-day-your-awesome-post.md does not exist.
```

The ```post``` layout we specified in our ```Front Matter``` part simply doesn't exist yet. We'll have to create it and add it to the ```_templates``` folder.

Templating in Jekyll works with the [Liquid](https://github.com/Shopify/liquid/wiki)  templating engine.

Now you'll probably want to check <http://jekyllthemes.org>, look for something that you like and then study it a bit to modify it to your liking. Note that some themes don't require you to do anything appart from copying files to your repository, while others may require you to install other libraries. After setting up my blog I chose the [Freshman21](http://yulijia.net/freshman21/) theme as it looks like a usual blog site, without a huge image taking the entire screen requiring you to scroll down to start seeing the content. The good thing also was that it only required [SASS](http://sass-lang.com/), so it was pretty simple to use.
