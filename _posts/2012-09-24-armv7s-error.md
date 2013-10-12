---
layout: post
title: file is universal (3 slices) but does not contain a(n) armv7s slice
by: Mike Foster
type: post
category: ios
---

That's an error message I received today when first attempting to build an app after updating to Xcode 4.5. It's pretty cryptic, but basically it has to do with one of the third party libraries the app relies on not being compiled for a specific architecture, armv7s. 

This is easy enough to temporarily address, until the third party releases a version that does target the specific architecture. There's actually two solutions. The first is to remove armv7s from the list of 'Valid Architectures' in the project's Build Settings. This doesn't seem ideal, as explained by [this answer on Stackoverflow][stackoverflow_link]. But, it's only suppose to be a temporary solution, and your app will still be supported by the armv7s architecture. You just may not get all the architecture has to offer, which is probably not a huge deal.

The other solution is to set the 'Build Active Architecture Only' setting to YES. This is not a complete solution, since if armv7s is your active architecture, you'll still run into this issue, but it should suffice for most cases. 

[stackoverflow_link]: http://stackoverflow.com/a/12402966/16524