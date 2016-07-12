---
layout: post
title: Generate splashscreens and icons with Ionic2
---

It's currently not written in the [CLI docs for Ionic2](http://ionicframework.com/docs/v2/cli/), but just like with Ionic1, it's possible to easily [generate splashscreens and icons](http://ionicframework.com/docs/cli/icon-splashscreen.html) for your app.

Simply use `ionic resources` to generate both splashscreens and icons. You can add the `--icon` or `--splash` options to only generate one or the other.

For it to work, you first need to add two initial files under `project/resources`:

- icon.png of at least 192x192
- splash.png of at least 2732x2732

Note that the size of the needed icon and splashscreen will depend on the platform and device:

- Currently the biggest splashscreen for Android has a side of 1920, while on iOS it's 2732. But if you only target smartphones it'll be smaller
- Currently the biggest icon for Android has a side of 192, while on iOS it's 180.