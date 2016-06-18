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

You're flawless, but hard drives are not. You can still benefit of Git's synchronization capabilities to save your changes on the cloud. It can also help you if you don't always use the same computer, eg sometimes you use your desktop, sometimes you use your laptop.

> It's going to be a hassle to install.

__It's easy to install:__ Versioning system used to be a bit complex to setup, because they used to be devided in 2 parts: the client and the server. [CVS](https://en.wikipedia.org/wiki/Concurrent_Versions_System) and [SVN](https://en.wikipedia.org/wiki/Apache_Subversion) are 2 famous old versioning system that used that model. Nowadays all versioning system are called decentralized, meaning that everyone can be a client and/or server.

- On Mac or Linux you'll probably use your package manager to install Git, on Windows just head over to <https://git-scm.com/download/win>. You'll then be able to use Git through the command line.
- You can then install a GUI like [TortoiseGit](https://tortoisegit.org/download/) if you don't want to type commands to interact with Git.

> It's going to be a hassle to learn.

__It's easy to learn:__ all versioning system work pretty much the same way (at least from a high level perspective) and mostly use the same terms. I'll be using Git nomenclature here, as Git became the de facto versioning system:

- _Repository_: that's basically the folder where all your files are
- <a name="commit"></a>Commit: that's when you save a version of your files, locally. Eg you just fixed a bug, then you commit all the related files, so that Git will remember that version
- _Push_: that's when you want to send all your modifications to someone else
- _Pull_: that's when you want to get the modifications from someone else. This is actually 2 actions
	- _Fetch_: that gets the modifications from someone else
	- _Merge_: that merges the modifications from someone else with your local files
- _Conflict_: that's when Git cannot automatically merge changes for you. You'll need to help it decide what to do, we'll see that below
- _Clone_: that's when a repository already exists remotely and you want to get it. It's a bit like the very 1st Pull (or Fetch)
- _Working Directory_: that's where your own files are. All of gits files will by default be in a .git directory

Note that older, centralized versioning would only have _push_ and _pull_, _commit_ and _fetch_ wouldn't exist. Actually, the way Git works is a bit like this:

- When you _fetch_, you retrieve the remote changes and store them locally in something called "remote" (so you now locally have what's on the remote server)
- When you _commit_, you save your changes in something called "local"
- When you _merge_, you merge the changes from your local "remote" to your "local"
- When you _push_, you send your changes to the remote server as well as your local "remote"

If that last part seems a bit complex, don't worry about it too much for now.

> It's going to be a hassle to use.

__It's easy to use:__ especially if you're using [TortoiseGit](https://tortoisegit.org/) as you don't need to learn by heart all the commands. But note that even with TortoiseGit you can still run Git commands if ever the GUI doesn't allow you to do what you want.

With TortoiseGit, just right click on a file or folder and you'll have access to Git commands.

1. First, you'll want to either create a brand new repository, or get an existing one.

	- If the Git repository doesn't exist yet, choose "Git create repository here...". Note that you can already have files insite the directory. You'll be able to commit them right after.
	- If the Git repository already exists, choose "Git clone..." and specify the address of the remote repository

1. Now that you have the repository on your computer, just start working as usual.

	- Try to only work on one small functionality, then as soon as you're happy with it, choose "Git commit -> master...". You select the files you want to commit and add a commit message, explaining what you worked on. 
		- It's important to try and add useful commit messages, so that you can easily navigate in the history or do a search for a specific modification.
		- Note that you can click the "View Patch>>" at the bottom right, to show the changes on a selected file.
	- If for some reasons you're not happy with your modifications, instead of choosing "Commit...", just choose "Revert..." and select the files you want to revert to the previously commited version.
	- If you previously cloned the repository, you can now choose "Push..." to send your changes.
	- If you previously created the repository and want to share it, you can choose "Daemon" to run  a small server so that everyone can _clone_ your repository, using the address shown on the "Daemon" page.

> I've got an error when pushing!

You may get an error message telling you the remote repository has changes you don't have yet. It just means someone else already did one or more _commits_ and then _pushed_ them, but you don't have them yet in your local repository. So you simply need to _pull_, which will _fetch_ the remote changes and then _merge_ them to your local repository, then you _push_ again.

> There are conflicts!

When merging remote changes to your own, it's possible that Git won't be able to decide what changes to keep. This can happen if you and someone else worked on the same file __and__ modified the same portion of the file, but differently. In that case, Git has no way in knowing what change is the correct one. You'll have to manually compare the remote file with your own, and decide what to do. But don't worry, TortoiseGit comes with a tool to help you with that: you'll be able to see what was modified by the other person on the left, what was modified by you on the right, and below the resulting merged file. You can right click on a conflict and choose to keep the remote changes, your local changes or even both, with either yours first or second. Anytime you can also directly modify the merged file.

Use GitHub
----------

Git is the de facto versioning system, and [GitHub](https://www.github.com/) is the de facto site to host repositories.

If you're a student, I would really advise you to publish all your projects from school to GitHub.

- It allows you to have a backup in case your hard drive dies, and it can also help you if sometimes you work on your own computer, and sometimes you work on your school's computers.
- It's also a good thing to add to your resume: it means you know how to use Git, and it can be a good way to show off your work.