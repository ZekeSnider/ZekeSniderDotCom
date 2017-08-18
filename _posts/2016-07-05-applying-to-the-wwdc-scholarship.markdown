---
layout: post
title:  "Applying to the WWDC scholarship"
date:   2016-05-02 19:13:40 -0700
categories: apple
image: wwdc-submission.png
---

I submitted my application to Apple’s WWDC 2016 student scholarship 2 days ago. It seems like every year I really want to do this, but this year I finally did. It feels like a great accomplishment, even if I don’t win. I’ve been learning Swift ever since The [Swift Programming Language book](https://itunes.apple.com/us/book/swift-programming-language/id881256329?mt=11) dropped at WWDC 2014, but I’ve always struggled with Cocoa frameworks because there’s just so much there to learn.
Recently, I’ve become somewhat bored with working on web projects in my free time. Possibly because I’m a web developer by occupation now, or maybe I’m just getting tired with the platform. But either way, I figured the best way to hit the ground running on learning iOS development would be to just build an app and fill in the gaps as I went. This brings us to my app, titled Z Split.

# Z Split

I started on this project in January, before even considering that I could submit this for the scholarship. It’s a split timer, in the vein of [WSplit](http://www.speedrunslive.com/tools/), [Llainfair](http://jenmaarai.com/llanfair/en/), and [LiveSplit](http://livesplit.org/)d. There are plenty of split timers out there, but no really great ones on iOS or OS X. This was a gap I wanted to fill.
I wanted to make a split timer that is elegant, and makes use of native APIs and specifics to the platform. I used Autolayout, Core Data, 3D Touch, Watch Kit, UIKeyCommand, among other technologies.

{% include sideByImage.html file="ZSplitCurrentView.jpeg" description="Current Run View" %}
I worked with my [dad](https://twitter.com/the_big_cor) on the design for the app. We decided to go with a dark theme for the app to start out with. I usually prefer dark theme in apps (Tweetbot for one). A light version is definitely coming at some point in the future. Hoping system wide dark mode is coming in iOS 10!
In March I attended the [try! Swift conference in Tokyo](http://www.tryswiftconf.com/en). There were lots of great speakers there, and I learned a lot of great tips there. One of which: protocols/extensions in Swift are really great!
I used protocols in my code to simplify two things: gradients on my custom UITableViewCells, and simple CoreData tables. They allowed me to save on repetitive code and slim down my View Controllers so they don’t become gigantic all encompassing monsters.
One of the hardest parts of developing the app was probably getting everything with Core Data working properly. It’s a pretty great framework, but the learning curve is quite high for a newbie like myself. And there’s a lot of tricky “gotchas” to manage with a persistence system. For example, at one point my app was taking seconds to load in each view because loading the split/route images from the core data store was blocking the UI thread. And I haven’t even gotten into the more advanced features of background thread management yet.

{% include fullWidthImage.html file="ZSplitAppleWatch.jpeg" description="Z Split for Apple Watch" %}

Z Split for Apple Watch
Developing the Apple Watch extension app was also an interesting experience. I wanted to make it as simple as possible, because otherwise you’ll honestly just reach for your phone because of the loading times on third party apps on the watch. Currently it just acts as a quick status indicator for the status of a run and you can preform actions such as Split, Pause, etc by the force touch menu. In the future I may consider adding more independent functionality to the watch app. But for now, I think it works quite well (as long as your phone is nearby).

{% include fullWidthImage.html file="ZSplitCommitHistory.png" description="Git commit summary" %}

This might not be the best idicator of activity since it includes library commits (I used 1 cocoapod, RSKImageCropper) and Storyboards which rack up line addition/deletions. But it gives a general idea of my activity on the project over time. Even though this project wasn’t conceived after the scholarship was announced, the past 2 weeks were still definitely a giant cram session to get things done on time.


{% include notfullWidthImage.html file="ZSplitIcon.png" description="Z Split’s amazing app icon" %}

# Future Development
I plan to continue development of Z Split, and submit it to the App Store in the coming weeks. There’s tons of features that I want to add, cramming over the scholarship period basically got me to the minimum required featured set. Additional functionality I want to add includes:
+ iCloud Sync
+ Handoff support
+ More functionality on the Apple Watch app
+ Sharing functionality for runs
+ Ability to edit routes after they are created (duplication functionality if you want to change the ordering)
+ Maps integration for automatic splitting for location based activities
+ Polish the UX, add some sounds and fancy animations to make things more personable
+ Add more run control buttons such as undo, skip.
+ More staticstics on the run page. Gold splits, comparisons to best segments, possible time save, delta time save over last segment, etc. Lots to do here.
+ More advanced staticstics on saved run histories. There’s lots of interesting information that could be generated based on previous runs because it’s stored in Core Data. Just gotta write some queries.
+ Code refactoring
+ And eventually… An OS X version! (Fingers crossed for UXKit coming to the mac)

Also I have a quite a few other ideas for apps to work on, and I will be open sourcing a OS X app I’ve been working on quite soon. Stay tuned. 😉

Even if I don’t win the scholarship, this was still an amazing experience, it was a great way to get me motivated!