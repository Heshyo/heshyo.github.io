---
layout: post
title: TortoiseGit useful tips
---

Here are some not necessarily obvious things that are quite useful in TortoiseGit.

See what you are about to push
------------------------------

Right click on your root folder then select "Git sync..." instead of "Push...".

This will show you all the commits and files that will be pushed.

Commit only files under a folder
--------------------------------

Simply right click on a sub folder instead of the root one then select "Commit...".

You can always filter things further by selecting/unselecting the files to commit.

Modify the last unpushed commit
-------------------------------

Right click on a folder then select "Commit..." and check "Amend previous commit".

This allows you to modify your previous commit. You can add or remove files to be committed, as well as modify the content of the files to commit.

__Note__ that Git does not directly allow modifying the history (there are plugins that exist, but more often than not it's best not to mess with the history). What happens internally is that the previous commit will be removed and a new one will be created. That's why modifying the previous commit is only useful if you haven't already pushed it, else as soon as you pull you'll get it back.

Discard one or more previous commits
------------------------------------

Right click on your root folder (or any sub folder or file to filter the commits) then select "Show log". Right click on the commit you want to go back to (ie all further commits will be discarded) and select "Reset branchName to this...". You will then have 3 choices:

- Soft reset: This is not really useful with TortoiseGit. The commits are discarded but the working directory (your code) is not modified (so you don't lose any code you modified). Also all the files you've modified and added are still in Git's index, ready to be committed. With TortoiseGit it's thus mostly useful if you want to still have the untracked files still selected when you do a "Commit...", as modified tracked files are selected by default
- Mixed reset: The commits are discarded but the working directory (your code) is not modified. Afterwards you can do a "Commit..." as usual and select what you want to commit. This is useful if you want to modify how many commits you have, and what's in them, without losing your code.
- Hard reset: The commits as well as the working tree (your code) are discarded. __You'll lose everything__ you've done after the commit you reset to. This is useful if you realize what you've done is not working and you want to go back to a previous state. __Note__: think twice or more before doing a hard reset, as any uncommitted change will be __lost forever__, and you will need to do things manually to hopefully recover the indexed files and later commits (and as you're using TortoiseGit, you probably have no current indexed files, so you'll probably lose all your uncommitted changes). See the following stackoverflow answers if you've made a mistake: [recover uncommitted (but indexed) files](http://stackoverflow.com/a/6780036) and [recover from (not too old) hard reset](http://stackoverflow.com/a/6636). To recover uncommitted, non indexed files, check what your IDE offers (eg PHPStorm has its own history, for Visual Studio I found, but haven't tested yet, the [AutoHistory](https://visualstudiogallery.msdn.microsoft.com/dfcb2438-180c-4f8a-983b-62d89e141fe3) extension).

Stash current working directory for later use
---------------------------------------------

Right click on the root folder (unfortunately clicking on any other folder will have the same effect, there's no filtering) and select "Stash save".

This allows you to save the current modifications on the working directory and go back to the previous commit. It's useful if you're at a point where you can't really commit anything (eg you're in the middle of fixing a bug or adding a new feature) and you notice a small bug you want to fix first. You save your current changes with a "Stash save" (you can add a description if you want), then fix the bug, commit the fix for the bug, then "Stash pop" to get back what you were working on.

If you do several "Stash save" without doing a matching "Stash pop", you can select which stash to get by doing a "Stash list". You'll see the list of all the things you stashed away, and you can get back to any of them by right clicking and choosing "Stash Apply". __Note__ that unlike with "Stash pop", when doing "Stash Apply" the corresponding stashed code will not be removed from the list. You'll want to manually remove it by doing a "Delete Ref...".

Commit files partially
---------------------

Right clik on a folder, select "Commit...", then right click on a file and chose "Restore after commit". Eventually double click on the file (still in the commit window) to open the merge tool.

This allows you to temporarily remove some of your changes through the merge tool. Once you commit your file(s), all the changes you removed are automatically put back, allowing you to do another commit. See [how to partially commit a file](http://stackoverflow.com/a/32527098) for a more detailed explanation on how to use TortoiseGitMerge for the task.

The same feature exists in TortoiseHg, but I find the UX of TortoiseHg a lot less confusing, but indeed less powerful (the last time I used TortoiseHg was around 2013, so things might have changed). In TortoiseHg, whenever you commit a file, you can see what was modified as usual, but you can also very easily select what you want to commit by checking/unchecking things in the diff tool. Also I find the term "Restore after commit" not that intuitive.
