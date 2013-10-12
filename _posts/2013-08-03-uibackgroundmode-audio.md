---
layout: post
title: UIBackgroundMode audio for AirPlay Streaming
by: Mike Foster
type: post
category: ios
date: 2013-08-03 11:00
--- 

If this post comes up in your search result, it's likely you're facing one of two problems. And, if you're facing the first problem, then you're sure to be facing the second one soon. So, which is it for you? Is it:

>When streaming a video in my iOS app through AirPlay, video playback stops when the phone sleeps...

Or, is it:

>Apple rejected my build for utilizing the audio UIBackgroundMode property...

Either way, then this post can help. And, I'll waste no more of your time as I'm sure your stressing.

By default Apple stops video playback through AirPlay when the device sleeps. I don't agree with this, but it's not a bug. It's expected. With that said, try streaming a video using Apple's own [Trailers app](https://itunes.apple.com/us/app/itunes-movie-trailers/id471966214?mt=8) over AirPlay. Playback continues when the device sleeps, doesn't it? So, obviously it's not only possible, but some factions within Apple don't seem to agree with the default behavior either.

To allow your app to stream videos over AirPlay when the device is asleep you first need to declare within your app's Info.plist file that your app requires the "audio" UIBackgroundMode. The image below shows how this key/value pair appears when shown with Xcode's plist viewer ("Required Background Modes"). 

<img class="postImg" src="/images/posts/uibackgroundmode_audio.png" title="UIBackgroundMode audio"/>

If you're looking at the plist source code directly, then it's declared like so:

	<key>UIBackgroundModes</key>
	<array>
		<string>audio</string>
	</array>

The next thing you have to do is utilize the audio background mode within your app, which just requires setting some AVAudioSession properties when your app launches. Do the following in your app's AppDelegate's `application:didFinishLaunchingWithOptions` method:

    #import <AVFoundation/AVFoundation.h>
    //...
    
    @implementation YourAppDelegate
    
    - (BOOL)application:(UIApplication *)application
        didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
	{
        // …
        AVAudioSession *audioSession = [AVAudioSession sharedInstance];
        [audioSession setCategory:AVAudioSessionCategoryPlayback error:nil];
        [audioSession setActive: YES error: nil];
        return YES;
    }
	
    // ..
    @end

And, that should be it. You should be good to go. Your app should keep streaming over AirPlay when the device sleeps. Now get that app submitted to Apple, so they can rejected it for utilizing the audio background mode.

Yes, I am serious. __The first time I released an app that utilized the audio background mode, Apple rejected it.__ However,  I was able to successfully dispute the rejection, because this is a valid use of the audio background property. 

What I recommend to do is be proactive with the review process. Apple allows you to provide notes for the reviewer. Be up front.  Let the reviewer know you are using the audio background mode, and let them know why. And, I recommend providing [this link to an Apple Technical Q&A article](http://developer.apple.com/library/ios/#qa/qa1668/_index.html) that advices developers to utilize this technique. It even has an "Important" note that states:

>The UIBackgroundModes audio key will also allow apps to stream audio or video content in the background using AirPlay.

So, there you go. Be up front, point to that article and you'll be good to go. 
