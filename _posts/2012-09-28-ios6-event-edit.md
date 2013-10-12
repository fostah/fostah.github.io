---
layout: post
title: iOS 6 Event Edit and Calendar Access
by: Mike Foster
type: post
category: ios
date: 2012-09-28 18:00
---

[Github Repository: iOS-Event-Edit-Example](https://github.com/fostah/iOS-Event-Edit-Example/downloads)

An instance of the iOS [`EKEventStore`](https://developer.apple.com/library/IOS/#documentation/EventKit/Reference/EKEventStoreClassRef/Reference/Reference.html) class provides a reference to the Calendar database. It's needed if your app plans to access the calendar in anyway.

In iOS 6, Apple introduced a new requirement for an application to gain access to the Calendar through `EKEventStore`. Basically, you have to ask permission first:

> On iOS 6 and later, you must request access to an entity type after the event store is initialized with requestAccessToEntityType:completion: for data to return.

Why the OS can't ask for permission on behalf of the application is beyond me. All I know is that any code that utilized `EKEventStore` in an application on iOS 5, won't work in iOS 6, without some changes.

The purpose of this post is to point you to an example of how to utilize the `EKEventEditViewController` class to allow your application's users to add an event to their calendar, relative to an entity within your app. For instance, maybe your app lists exciting activities and events happening in the user's area. You would probably like to allow them to easily add to their calendar reminders of events they're interested in.

Beyond showing how easy it is to add this very powerful piece of functionality to your app, the main purpose is to show how to properly support iOS 5 and iOS 6 devices. I've added that snippet below, in case that's all you're interested in figuring out.

    EKEventStore* eventStore = [[EKEventStore alloc] init];

    // iOS 6 introduced a requirement where the app must
    // explicitly request access to the user's calendar. This
    // function is built to support the new iOS 6 requirement,
    // as well as earlier versions of the OS.
    if([eventStore respondsToSelector:
        @selector(requestAccessToEntityType:completion:)]) {
        // iOS 6 and later
        [eventStore
         requestAccessToEntityType:EKEntityTypeEvent
         completion:^(BOOL granted, NSError *error) {
             // If you don't perform your presentation logic on the
             // main thread, the app hangs for 10 - 15 seconds.
             [self performSelectorOnMainThread:
              @selector(presentEventEditViewControllerWithEventStore:)
                                    withObject:eventStore
                                 waitUntilDone:NO];
         }];
    } else {
        // iOS 5
        [self presentEventEditViewControllerWithEventStore:eventStore];
    }

First, you need to check if the `EKEventStore` instance responds to the method used to request access. If it does, then go ahead an make the request. If it doesn't, then you can just move on to presenting the instance of `EKEventEditViewController`.

Once you've been granted access, the completion method is called. At that point, you'll want to call the function that presents `EKEventEditViewController`. 

There's one important thing to note within the completion method. If you don't use the `performSelectorOnMainThread:` method to call your present method, the app will hang for 10 to 15 seconds before displaying the `EKEventEditViewController's` view. Just something to be aware of.

If you're interested in the complete example, you can clone or download the code from [its repository on github](https://github.com/fostah/iOS-Event-Edit-Example/downloads). I hope it's useful.