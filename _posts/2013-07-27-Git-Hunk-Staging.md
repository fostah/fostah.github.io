---
layout: post
title: Git's Hunk / Patch Staging
by: Mike Foster
type: post
category: vcs
date: 2013-07-27 7:00
--- 

"Yeah, I think they're called Chunks," one of my fellow developers said. I was in the process of moving our iOS frameworks from a Subversion repository over to individual Git repositories, the hierarchy of which I plan to share in a future post. 

"Or maybe it's Hunks," he continued. We went on to have a lengthy conversation about this horribly named featured of Git. And, if you don't think "hunks" is all that bad, try having a conversation about it in an office with an open layout. Let me know how many odd looks and raised eyebrows end up pointed your way.

Bad naming and colleague judgment aside, I'm glad this developer turned me on to hunks (see how easy it is for the conversation to get weird). Instead of staging a whole file with multiple changes for different purposes, hunk staging allows developers to choose which changes within the file to stage, and which to leave for later stagings.

To admit, after learning of hunks, I wasn't convinced of its usefulness. Sure, I could see how it would allow for more granular, descriptive committing practices, which we should certainly strive for. Even with that -- maybe due to the name itself, I'm not sure -- but I didn't expect hunks to become a part of my development process. I especially didn't expect it to happen almost immediately. I certainly didn't imagine that `git add -p` would soon become the default way that I stage anything and everything for committing. But, it did. And, it is.

By the way, that command is just how simple the feature is to use from the command line. It basically turns your terminal into a very simple code review application that allows you to make choices about what to stage. 

The screen capture below gives you an idea of the presentation (click to expand). As you can see, there's quite a few options, the main ones in my opinion being:

	y - Oh yeah, stage that hunk.
	n - Eew, gross. Get that hunk away from me.
	s - Split that hunk. Yeah, just like that.
	e - If you want a hunking done right, do it yourself.

<a href="/images/posts/githunk_screencap.jpg" onclick="return hs.expand(this)">
	<img class="postImg" src="/images/posts/githunk_screencap-thumb.jpg" align="right" title="Git Hunk Staging"/>
</a>

As I mentioned already, I'm hooked on hunks. Adding a simple code review to the commit process, even if its only a review of your own code, is invaluable. Give it a try. I'm sure you'll also appreciate the power of hunk staging.

[Check out this documentation](http://git-scm.com/book/ch6-2.html#Staging-Patches) for more details on staging with patches and hunks.