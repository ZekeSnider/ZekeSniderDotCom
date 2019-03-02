---
layout: post
title:  "The Making of Jared"
date:   2018-11-20 22:15:40 -0700
categories: side-projects
tags: [instagram]
image: jared.png
imageAlt: "iMessage icon with robot emoji"
---

I've realized that I've never written about my side project Jared here, and its background, so I figured now is a good time.

# Background
For the uninitiated, iMessage is Apple's messaging platform that is only available on Apple devices. If you text another iPhone user from an iPhone, the bubble will be blue (sent via iMessage).

Apple has tried to lock down iMessage as much as possible, and there is no public API for common chat bot hooks. As far as I know, I was the first to attempt to write any sort of chat bot for iMessage. Because of how obtuse it is, I assume most would rather write for a platform that actually encourages development of bots, but I was in it for the challenge! iMessage is my favorite messaging service, and I wanted to build on it.

There are only a few hooks available to iMessage:

+ ❌ iMessage Apps (introduced in iOS 10). These provide very limited hooks that would not provide a chat bot like is implemented in Jared. All input would need to be in the context of the message extension, as these apps cannot read messages, names, or any personal information. They also can only send messages with the user's explicit consent. 

+ ❌ Siri Shortcuts (introduced in iOS 12). Shortcuts have interesting potential, but they are also limited. They can only be triggered manually, and user consent is needed to send messages with images. They cannot read conversation history.

+ ✅ AppleScript support via Messages.app for the Mac.

The Messages.app on the Mac has existed before the introduction of iMessage. It originally only supported AIM, Google Talk, and Jabber accounts. It was just Apple's instant messenger client. Of course it has evolved and now only supports iMessage, but the base code base has remained the same. For that reason (I'm assuming), it still (had) full AppleScript support.  

AppleScript is basically a local RPC framework for automating actions on the Mac. Its syntax is very... strange (English like)? It's not so much an elegant programming language, but it is good for its use as an automation language. There is also a technology called JXA which allows you to use AppleScript actions from JavaScript instead, but it is not very well documented and I haven't spent much time with it. But I think all the AppleScript in this project could be replaced with JXA.

For my purposes, two pieces of functionality were key in the Message App's AppleScript dictionary:

{% include fullWidthImage.html file="incominghandler.png" description="Message Handler" %}

This is triggered when a message is received. It provides the content of the message as well information on the sender. From then we can do routing on it, and if needed...

{% include fullWidthImage.html file="sendhandler.png" description="Send Action" %}

Send a message to a specified recipient. This even supports attachments, and [sending to group chats](https://stackoverflow.com/questions/44852939/send-imessage-to-group-chat/44998688#44998688) making it extremely useful to me. These two pieces were all I need to implement my chat bot.

# The first implementation
{% include fullWidthImage.html file="JaredArchitecture.001.jpeg" description="v1" %}

The first take (which I have not open sourced because of how sloppy it is), was functional but not elegant. Everything was implemented in one AppleScript handler file. It did not call into any other languages or frameworks.

{% include fullWidthImage.html file="MessagesPreferencesBefore.png" description="Installation" %}

Because everything is in one file, it became quite bloated quickly. In addition, AppleScript syntax is very verbose and not well catered towards writing scalable software. It was really intended for small macro automations.

+ Single threaded - Whenever the handler is processing a message, it is unable to handle new incoming messages. This caused messages to not get handled if they were routed at the same time, and errors to assert causing the whole system to stop functioning until it was manually restarted.
+ Errors - If any error was thrown and not caught, it could cause an error dialog to pop, which shut the whole thing down as mentioned. These errors could be timeouts (spending more than 60 seconds processing), or just inconsistent system errors. As such, I devised a method that would automatically dismiss any error alerts, but this was not 100% effective either.
+ Limited Scope - AppleScript as a language is limiting as it lacks many standard language functionality and frameworks. I was able to cut some corners by using [some](http://www.mousedown.net/mouseware/JSONHelper.html) [AppleScript helper apps](http://www.mousedown.net/mouseware/TwitterScripter.html) that exposed things like REST calls, but this was obtuse and didn't work for all the features that I wanted to implement.

And thus brought v2, a full rewrite...

# The better implementation 
{% include fullWidthImage.html file="JaredArchitecture.002.jpeg" description="v2" %}

This (current) version uses a native app written in Swift that exposes an AppleScript interface. The Applescript handler in messages is very simple and just passes data to the Swift app. This solves several issues of the initial implementation.

+ Multi-threading - All incoming requests on the Swift side are put in a background thread via a GCD Dispatch queue. This allows processing to take as long as needed for each message, and allows for other things such as sending delays.
+ Frameworks - Swift is a well supported, native language. You'll be able to find libraries (built in or not) for most things, and it allows for interop with C and Obj C, among other languages.
+ Plugins - The original version had all routing in one AppleScript file, which became difficult to maintain after adding many commands. The Swift version added a plugin framework with a rules engine for routing. Additional commands could be added just by building them into a plugin module and placing them in a plugin directory. The plugins are loaded in via NSBundle modularization. 
+ UI - Because it is running as an app, Jared can now provide a cocoa UI for enabling/disabling the service, as well as configuration options. 

{% include fullWidthImage.html file="JaredUI.png" description="The new UI" %}

These improvements allowed for much more flexibility. Many bottlenecks were removed, and it also allowed for adding things like database persistence for various state parameters, background processing jobs for scheduling, etc. 

A simple send script is called for sending outgoing messages. It is called by making an [osascript](https://ss64.com/osx/osascript.html) shell call from Swift.

This solution was great, until Apple made a change in macOS High Sierra 10.13.4.

{% include fullWidthImage.html file="missingsetting.png" description="Wait where'd it go.. 🤔🤔🤔" %}

They removed the option to specify an AppleScript handler in Messages.app. I would like to think I caused this, but more realistically an engineer at Apple probably discovered this menu option one day and asked why the functionality exists at all. 

The message send action still exists, but now there is no way to receive the hooks for incoming messages, which brings us back a few steps.

# Working around Apple
{% include fullWidthImage.html file="JaredArchitecture.003.jpeg" description="Third time's the charm" %}

In `~/Library/Messages` there is a messages.db SQLite database that contains the contents of all message history. It gets updated when new messages are received/sent. Suprisingly, it is a standard SQLite database, and it is not encrypted. So, as a workaround to the AppleScript handler, we can instead query the database on a set interval (5s)? For all new records since the last query. This will allow us to batch process all new messages received.

In addition, there are lots of fields in the database that are available in the database, that were not available via AppleScript, such as read receipts, and more. Although it will be difficult/impossible to query on changes to fields that do not trigger update of a date field.

This solution works around the removal of the AppleScript handler preference, while still keeping most of the codebase the same. 

This is not fully implemented yet, as I've been working on it on a branch which hasn't been merged yet. Progress can be tracked on [this issue](https://github.com/ZekeSnider/Jared/issues/20). Credit to Github user mezeipetister for the idea of repeatedly querying the database. Unfortunately I've neglected this project too much, but I hope to complete this fix in the near future, so that we can get Jared working on Mojave 😀. I will likely write another update post once that is done.

# PS: Private Frameworks
You may ask, isn't there some private API call you can use to bypass AppleScript for sending messages. Well, there is in theory, but I haven't been able to get any of them to work properly (read: at all). I've used [class-dump](https://github.com/nygard/class-dump) to retrieve header files, and tried to use methods of the Messages private framework, but haven't had any luck. 

This is something I should probably spend some more time on, but it's pretty tedious and unsatisfying. A hundred different things could be preventing the method calls from working, but because it's a private API, it's very difficult to diagnose it. If anyone has anyone to share on this, please let me know!



So there you have it, the complete story of Jared thus far. If you want to try it out, [check it out on GitHub](https://github.com/zekesnider/jared) and give it a star. Hit me up by Twitter or email if you have any questions!
