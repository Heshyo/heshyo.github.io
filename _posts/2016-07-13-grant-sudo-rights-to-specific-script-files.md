---
layout: post
title: Grand sudo rights to specific script files
---

When using [Symfony](https://symfony.com/), we often need to clear the cache. The problem is that depending on how you configure the user rights, you may need sudo privileges to clear the cache: eg the owner of the Symfony files is a normal user, the group is the web server. Whenever Symfony creates the cache directories and files, those are now owned by it, and the normal user cannot delete them.

To allow a normal user to use sudo in some cases without requiring a password, you'll need to modify the `/etc/sudoers` file. It's possible to include other files or directories by using the `#include` directive. Eg your `/etc/sudoers` file may already contain `#includedir /etc/sudoers.d`, which means that any file under `/etc/sudoers.d/` will also be parsed. 
Adding files under `/etc/sudoers.d/` can be useful to keep things clean if you have too many rules. But it looks like it can be safer also during system upgrades, as the main `/etc/sudoers/` file may be modified, while the content of `/etc/sudoers.d/` will be kept as is.

Here's how to edit the main file or a different file:

```
sudo visudo
sudo visudo -f /etc/sudoers.d/myOtherFile
```

In one of those file you can then add something like

```
userName ALL=(ALL) NOPASSWD: /path/to/the/script.sh
%groupName ALL=(ALL) NOPASSWD: /path/to/the/otherScript.sh
```

The user `userName` will be able to `sudo script.sh` without needing a password, same with the group `groupName` with `sudo otherScript.sh`.