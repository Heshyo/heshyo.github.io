---
layout: post
title: Fast setup with npm, Ionic2 & OSX
---

I really like `npm` and the way more and more frameworks come with easy scaffolding features. With Ionic2 (still beta currently) for example, you just need to

```
npm i -g ionic@beta
ionic start myNewApp --v2
```

then `npm install` whenever you need to install or update dependencies. The main problem is that web technologies evolve very fast, and all your plugins don't necessary follow (nor you)...

For example, I work on Windows but I had an issue on an Ionic2 app that required debugging some native code.

I boot the Mac, `git clone` the repo and then `npm install` the dependencies.

Oops, I get tons of errors. Rerunning `npm install` gives me different errors each time...A colleague advises me to update npm. Great idea as the Mac had a prehistorical version of node:

```
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```

Now I'm on the latest node v6! Great, let's `npm install` again. Looks like everything installs correctly.

__Note__ that I could have used `npm i -g n lts` to install the LTS version of node. That would have saved me some time...More below.

So now that I updated node, I just need to `ionic run ios`. Nope, I'm getting a

> fs: re-evaluating native module sources is not supported. If you are using the graceful-fs module, please update it to a more recent version.

Searching on the net, I find lots of projects having that error. Basically, lots of things were changed in node v6, and so lots of npm modules must be updated. People advise to downgrade to node v5 or v4. I decide to go with v4 as it's a [LTS](https://github.com/nodejs/LTS/) version while v5 is a beta version (check the [node 5 release notes](https://nodejs.org/en/blog/community/node-v5/) if you want more info about the release numbers, beta and LTS).

I find a gist to [remove node from Mac OS](ttps://gist.github.com/TonyMtz/d75101d9bdf764c890ef). It looks like node, npm and all my node related files were removed.

I decide to install [homebrew](http://brew.sh/) as I found lots of ways to install and uninstall node through homebrew.

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

I liked that it tells me what it's about to do. Once done, I installed node v4 following the instructions from this [StackOverflow answer](http://apple.stackexchange.com/a/207883)

```
brew install homebrew/versions/node4-lts
```

Now it's time to remove my current modules, reinstall them and finally run the app:

```
rm -r nodes_modules
npm install
ionic run ios
```

but it doesn't know `ionic` anymore! Of course, I removed all npm modules, so I need to `npm i -g ionic@beta` then `ionic run ios` again.

I'm advised to upgrade my cordova version as well as add ios-deploy and ios-sim:

```
npm i cordova
npm i -g ios-deploy
npm i -g ios-sim
```

Now I can `ionic run ios` again...but I get a

> Error: spawn EACCES

I easily find a [bug report on Ionic](https://forum.ionicframework.com/t/how-to-fix-this-error-spawn-eacces/20490). I just need to make `hooks/after_prepare/010_add_platform_class.js` executable, or better yet, do an `ionic hooks add`. It takes care of the permissions as well as add other hooks.

So I `ionic run ios` once more, only to realize that the version of XCode doesn't know iOS 9 yet! In the app store I see that Yosemite can be updated: we're still on Yosemite 10.10.4 (nope, we don't use the Mac very often). The 10.10.5 update was already downloaded, so I just needed to restart. It still took 10 minutes to install the update though. Then it restarted. Then I waited another 10 minutes.

I now try to update XCode but I actually need El Capitan. So I let it download through the night. Next morning I see that it's downloaded but I'm not asked if I want to install it. Eventually I end up downloading it again, and again I'm not asked to install it. Others have had the same issue, and I find out that I can just find the installer inside `Applications`. So I start the installation. A few hours later I see that the installation is finished.

I can now install the latest XCode, which is pretty big meaning it still takes quite some time to download and then install...

I can finally build and test on my iPhone. Oh wait, I still need to take care of those provisioning profiles. I of course select the wrong account for the app (clients usually want to have their own account). The app ID is on another account, but I can't seem to tell XCode to switch account, I always get `no valid signing identities matching the team id`. So I remove all other accounts then retry. This time it works, but of course, I don't have the required certificate on this Mac, as it was generated with PuttyGen on a PC.

So I get the `.p12` file from the PC, look around for a way to add it to the key chain. I find that I need to execute the following

```
security import myPrivateKey.p12 -k ~/Library/Keychains/login.keychain
```

I try again from XCode to debug on my device and it works! Now, what was it that I wanted to debug again?