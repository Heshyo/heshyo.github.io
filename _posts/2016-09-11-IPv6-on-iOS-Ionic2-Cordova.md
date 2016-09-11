---
layout: post
title: IPv6 on iOS Ionic2/Cordova
---

Since June 1st, 2016, all apps submited to the App Store must [support IPv6-only networking](https://developer.apple.com/news/?id=05042016a). Obviously we updated our backends to handle IPv6 (while doing so I realized that neither Amazon nor Azure had support for IPv6 yet) and tested our apps by doing a DNS64/NAT64 on our Mac (see [To set up a local IPv6 Wi-Fi network using your Mac](https://developer.apple.com/library/ios/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html)).

I thought Ionic2/Cordova apps wouldn't have issues, but looks like they do, at least on Apple's test environment. I only found a few posts about apps being rejected for lack of IPv6 compatibility, some were with native apps, others with Cordova. On the few posts about Cordova, I found one that [suggested using a specific plugin](http://stackoverflow.com/a/37896631/276648): [cordova-HTTP](https://github.com/wymsee/cordova-HTTP). And sure thing, after using it, there was no IPv6 issue on Apple's side anymore. Maybe using the new [WKWebFrom from Ionic](http://blog.ionic.io/cordova-ios-performance-improvements-drop-in-speed-with-wkwebview/) will help with this problem. It's quite weird that UIWebView doesn't work with IPv6.
