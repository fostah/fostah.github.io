---
layout: post
title: iOS 6 UINavigationBar Appearance Customization
by: Mike Foster
type: post
category: ios
date: 2012-09-28 16:00
---

In iOS 6, you can still universally customize your app's navigation bar through the `[UINavigationBar appearance]` object, as you would expect. However, in iOS 6 the impact is even more universal, impacting views that were previously immune to such customization. For instance, `MPMoviePlayerViewController` now inherits from the `[UINavigationBar appearance]` object. In iOS 5 the navigation bar of `MPMoviePlayerViewController` remains the same whether or not the app has a custom navigation bar appearance. It appears as if it is floating over the playback, which happens to be the preferred visual for my application. 

This isn't a huge deal to address, of course. My app can easily single out that specific view and apply whatever customization of the navigation bar that is required, even nullifying customization so that the default is used, as demonstrated in the following example.

    [[UINavigationBar
      appearanceWhenContainedIn:[MPMoviePlayerViewController class], nil]
     setBackgroundImage:nil
     forBarMetrics:UIBarMetricsDefault];

This basically creates a black list in which you can add any classes you don't want impacted by the custom navigation bar appearance.

There is a problem, however, with the black list approach. If you happen to have YouTube videos available in your application, you may assume that the example above will take care of its navigation bar's appearance as well. That's what I assumed, at least. As I found out from [this answer on Stackoverflow](http://stackoverflow.com/a/12433837/16524), the YouTube player is actually an instance of a private class called `MPInlineVideoViewController`. And since it's private, that throws the black listing idea out the window. 

To be honest, black listing wasn't a great approach anyways. There could certainly be other views that I'm not thinking about that also shouldn't be customized. Specifying exactly which views should be customized would be more manageable. A white listing approach would be better in this situation.

In my app, all views that are specifically implemented for the app extend from one super view controller. This allows for custom configurations to be easily applied across the app. This same class can be used as the app's white list filter, as this example shows.

    UIImage* navBg = ...;

    [[UINavigationBar
      appearanceWhenContainedIn:[SuperViewController class], nil]
     setBackgroundImage:navBg
     forBarMetrics:UIBarMetricsDefault];

It's a great solution, mainly because it's simple, but also because it will be obvious when a view that is suppose to have a custom navigation bar appearance doesn't have it. If it's missing the navigation bar customization, it'll be missing a lot of other configurations as well, which will boldly stand out to developers and tester alike.