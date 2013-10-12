---
layout: post
title: iOS 6 Orientation Handling
by: Mike Foster
type: post
category: ios
---

Updating an app from iOS 5 to iOS 6 is equivalent to death by 1000 papercuts, though you do get to live afterwards. So, it's not quite as bad.

The main issue is that there's several items within the SDK that are deprecated. I don't even think it's fair to call them deprecated. Abandoned would be a better description. Usually when something is deprecated, it still functions as expected. This is not turning out to be the case with iOS 6, at least in certain situations.

In order to help others possibly avoid some of these papercuts, I'll post about any issues I run into, and provide the solutions I come up with. This is the first of those post. Hopefully there won't be too many to follow.

#### The Problem

The app has a modal view, a subclass of MPMoviePlayerViewController, that must be locked to landscape orientation. The rest of the app must be locked to portrait orientation.

#### iOS 5 Solution

In the app's *-Info.plist* file, *Supported interface orientations* should only contain one item, *Portrait (bottom home button)*.

Next, in the modal view controller class that needs orientation locked to landscape, you need to override the `- (BOOL)shouldAutorotateToInterfaceOrientation:
(UIInterfaceOrientation)interfaceOrientation` method and return `YES` or `NO` relative to a boolean check against the `interfaceOrientation` argument.

Here's what the function looks like.

    - (BOOL)shouldAutorotateToInterfaceOrientation:
        (UIInterfaceOrientation)interfaceOrientation
    {
        return (interfaceOrientation == UIInterfaceOrientationLandscapeLeft ||
                interfaceOrientation == UIInterfaceOrientationLandscapeRight);
    }

Pretty simple stuff, eh?

#### iOS 6 Solution

In iOS 6, the `-(BOOL)shouldAutorotateToInterfaceOrientation:
(UIInterfaceOrientation)interfaceOrientation` method is deprecated, and appears to not be called at all anymore. It's replaced by a set of methods; `-(BOOL)shouldAutorotate` and `-(NSUInteger)supportedInterfaceOrientations`. Both need to be utilized in the solution I came up with. Here are the details.

First, as with the iOS 5 solution, the app's *-Info.plist* file still only contains one item, *Portrait (bottom home button)*. Doing this, however, locks the app to portrait orientation, no matter what you specify in your view controllers. The App's `UIApplicationDelegate` now needs to get involved.

To make other orientations available throughout the app, you must override `-(NSUInteger)application:(UIApplication*)application supportedInterfaceOrientationsForWindow:(UIWindow*)window` within the app's `UIApplicationDelegate`, and return `UIInterfaceOrientationMaskAll`:

    - (NSUInteger)application:(UIApplication*)application
            supportedInterfaceOrientationsForWindow:(UIWindow*)window
    {
        return UIInterfaceOrientationMaskAll;
    }

With this current setup, the app is no longer locked at all, free to rotate to all supported orientation. To accomodate this issue, changes need to be made to the `rootViewController` of the app. 

In the app I'm working on, the rootViewController is a `UITabBarController`, which I happen to already override for other reasons. You'll need to override your app's rootViewController class and override `-(BOOL)shouldAutorotate`, and have it return `NO`:

    // iOS6 support
    // ---
    - (BOOL)shouldAutorotate
    {
        return NO;
    }

**You don't want to override `-(NSUInteger)supportedInterfaceOrientations` in this case**. I initially assumed you should override this method. Nope. Doing so caused a bug where returning from a modal view resulted in the navigation bar sliding up under the status bar. Oops. It turns out just specifying that you don't want the `rootViewController` to autorotate is enough to lock it to portrait orientation.
	
Finally, in the modal UIViewController you want presented in landscape orientation, you need to override both `-(BOOL)shouldAutorotate` and `-(NSUInteger)supportedInterfaceOrientations`:

    // Deprecated in iOS6, still needed for iOS5 support.
    // ---
    - (BOOL)shouldAutorotateToInterfaceOrientation:
            (UIInterfaceOrientation)interfaceOrientation
    {
        return (interfaceOrientation == UIInterfaceOrientationLandscapeLeft ||
                interfaceOrientation == UIInterfaceOrientationLandscapeRight);
    }

    // iOS6 support
    // ---
    - (BOOL)shouldAutorotate
    {
        return NO;
    }

    - (NSUInteger)supportedInterfaceOrientations
    {
        return UIInterfaceOrientationMaskLandscape;
    }
    // ---

And, that's pretty much it. You should be good to go. Now on to the next papercut.