---
layout: post
title: Why do you want to always use a versioning system (probably Git)?
---

First, let's see the kind of issues we can have as a software developper
------------------------------------------------------------------------

1. I write a program alone. I'm adding a new feature or fixing some bug, and then nothing is working anymore! I would like to go back to how things were just an hour ago, or even better, I'd like to easily see what I modified to know where I made a mistake.
1. I write a program with other people and it's always a mess to keep our code synchronized. We modify the same files and we end up overwritting each other's modifications.

Note that those issues arise all the time in software development, but they may also arise in other domain.

Meet Git, or any versioning system
----------------------------------

Versioning systems were created to help you with those issues:

1. They contain the history of all the modifications of all the files ([see the part below on commit](#commit))
	- You can easily go back in time to a working version of your code
	- You can easily compare files between versions to track down regression (things that used to work but are not working anymore)
1. They handle merging files from different persons, so you don't need to worry anymore to keep your files synchronized between team members or even between your computers

Yeah but I don't really need that
---------------------------------

> I'm just an IT/CS student and I work alone, I really don't need Git.

If you're just starting progamming classes, then maybe you havent yet encounter the dreaded "nothing is working anymore" mentioned above, but don't worry, you will, sooner than later, probably aournd 2am, a few hours before your project's deadline.

> I write great code and I never break things.

You're flawless, but hard drives are not. You can still benefit of Git's synchronization capabilities.

> It's going to be a hassle to install/learn/use.

- __It's easy to install:__ Versioning system used to be a bit complex to setup, because they used to be devided in 2 parts: the client and the server. [CVS](https://en.wikipedia.org/wiki/Concurrent_Versions_System) and [SVN](https://en.wikipedia.org/wiki/Apache_Subversion) are 2 famous old versioning system that used that model. Nowadays all versioning system are called decentralized, meaning that everyone can be a client and/or server.
	- On Mac or Linux you'll probably use your package manager to install Git, on Windows just head over to <https://git-scm.com/download/win>. You'll then be able to use Git through the command line.
	- You can then install a GUI like [TortoiseGit](https://tortoisegit.org/download/) if you don't want to type commands to interact with Git.
- __It's easy to learn:__ all versioning system work pretty much the same way (at least from a high level perspective) and mostly use the same terms. I'll be using Git nomenclature here, as Git became the de facto versioning system:
	- _Repository_: that's basically the folder where all your files are
	- <a name="commit"></a>Commit: that's when you save a version of your files, locally. Eg you just fixed a bug, then you commit all the related files, so that Git will remember that version
	- _Push_: that's when you want to send all your modifications to someone else
	- _Pull_: that's when you want to get the modifications from someone else. This is actually 2 actions
		- _Fetch_: that gets the modifications from someone else
		- _Merge_: that merges the modifications from someone else with your local files
	- _Clone_: that's when a repository already exists remotely and you want to get it. It's a bit like the very 1st Pull (or Fetch)
	- Note that older, centralized versioning would only have push and pull 
- __It's easy to use:__ especially if you're using [TortoiseGit](https://tortoisegit.org/) as you don't need to learn by heart all the commands. But note that even with TortoiseGit you can still run Git commands if ever the GUI doesn't allow you to do what you want. Let's say you're using TortoiseGit.
	- Right click on a file or folder and you'll have access to Git commands
	- If the Git repository doesn't exist yet, choose "Git create repository here..."
	- If the Git repository already exists, choose "Git clone..."
	- Once the repository 





First, what is a versioning system exactly?

It's a software that allows you to save versions of the same files, meaning that you'll be able to see the history of all the modifications.


Templating in Jekyll works with the [Liquid](https://github.com/Shopify/liquid/wiki)  templating engine.

Now you'll probably want to check <http://jekyllthemes.org>, look for something that you like and then study it a bit to modify it to your liking. Note that some themes don't require you to do anything appart from copying files to your repository, while others may require you to install other libraries. After setting up my blog I chose the [Freshman21](http://yulijia.net/freshman21/) theme as it looks like a usual blog site, without a huge image taking the entire screen requiring you to scroll down to start seeing the content. The good thing also was that it only required [SASS](http://sass-lang.com/), so it was pretty simple to use.
