---
layout: post
title: Background audio and lockscreen player on iOS Ionic2/Cordova
---

If your Cordova/Phonegap app needs to play audio on iOS when the app is in the background, you simply need to use [cordova-plugin-media](https://www.npmjs.com/package/cordova-plugin-media) and tell iOS you want to use the background audio mode (see [Background Execution](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html) for more info on those modes in iOS).

On Phonegap, you just need to add the following in your config.xml (see [Modifying Manifests](http://docs.phonegap.com/phonegap-build/configuring/config-file-element/) for more info):

```xml
<config-file platform="ios" parent="UIBackgroundModes" overwrite="true">
	<array>
		<string>audio</string>
	</array>
</config-file>
```

Now that you can listen to your audio while your app is in the background, and even when the screen is locked, it'd be nice to be able to see what's playing and control it on the lock screen. For this you'll need to use some plugin(s), eg:
- [cordova-plugin-remotecommand](https://github.com/leon/cordova-plugin-remotecommand) to play, pause, go to the next or previous song
- [cordova-plugin-nowplaying](https://github.com/leon/cordova-plugin-nowplaying) to specify the title, artist and artwork

Now that everything is working and you're about to publish, don't forget to explain in the notes before publishing that you need the background audio (same with any other background mode), even if it may seem obvious, as it's probably not that obvious for Apple bots.