---
layout: post
title: Credentials in git, or how to manage several GitHub accounts
---

On Windows, it seems by default git uses [Git Credential Manager for Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows). The first time you try to push something to GitHub, when accessing the repository through HTTPS, you'll be asked for your username and password. The next time you push anything to GitHub (whether that same repository or a different one), the previously entered credentials will be used automatically.

This is great if you always use the same GitHub account, but can be confusing and impractical when you need to push to different accounts.

Those credentials are stored in the `Credential Manager`. You can do a search or open the Control Panel. Make sure you're looking at the `Windows Credentials`, not `Web Credentials`. Under `Generic Credentials` you should see `git:https://github.com`. Remove the entry and the next time you push something to GitHub, you'll be asked again for your username and password.