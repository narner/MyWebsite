---
title: "Things a First Time PM Should Know About iOS Development"
date: "2019-09-27"
---

This post is a modification of some notes I wrote for a friend, who was recently hired for their first Project Management position at a startup that is developing an iOS app. They don't have a technical background, and wanted to know what some fundamental things to know about iOS development, the iOS ecosystem, and workflows of development.

I modified those notes for this blogpost. Feel free to reach out at [nicholasarner@gmail.com](mailto:nicholasarner@gmail.com) if you have any questions or want to learn more!

&nbsp;

## App Development

**Languages**

There are two programming languages that can be used for developing Apple apps; Objective-C, and Swift. Objective-C was the first language that was used by Apple for macOS/iOS development. You shouldn't really ever encounter much about Objective-C though, unless you're dealing with an older app project.

Swift is Apple's big investment into the future of software development on their platforms - any new apps written from scratch should be implemented in Swift.

With every new OS version update, there's a corresponding Xcode update, (and also update to the Swift language), so often some time is needed to be spent whenever this happens making sure that apps are converted to the new Swift version.

**Interfacing between Swift and Other Languages**

Swift and Objective-C code can be used together in projects, so if there's a project that contains some legacy Objective-C code, you can integrate newer Swift code with it easily. More info [here](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift).

Additionally, it's quite easy to [interface with C code directly from Swift](https://theswiftdev.com/2018/01/15/how-to-call-c-code-from-swift/). If a lot of the core functionality of your product is implemented in C code,  you can use that code both in your iOS app via Swift, and an Android app via Java or Kotlin.

**Xcode/iOS SDK/Documentation**

Xcode is an IDE ([Integrated Development Environment](https://en.wikipedia.org/wiki/Integrated_development_environment)) that Apple provides for developing apps on their platforms. When you download Xcode, you're also downloading the iOS, macOS, watchOS, and iPad) SDK's ([software development kits](https://www.notion.so/narner/Things-a-First-Time-PM-Should-Know-About-iOS-Development-7ddcc5c1155f45fbaa1baa1277a19ea5)), which contain all the software libraries that let you develop for the respective platforms.

There's generally a lot of overlap between the last three of these; with variances specific to the respective device types. One of the things that Apple is currently working on, [Project Catalyst](https://arstechnica.com/gadgets/2019/07/catalyst-deep-dive-the-future-of-mac-software-according-to-apple-and-devs/), will allow for iOS apps to be able to be easily ported to run as desktop-based macOS apps.

All the documentation for the software tools for building app functionality can be found on Apple's dev site [here.](https://developer.apple.com/documentation/)

Apple also provides a detailed [Human-Interface Guide](https://developer.apple.com/design/human-interface-guidelines/) for making sure apps conform to the best practices of user interaction that their platforms afford — required reading for anyone who's designing apps for Apple devices.

**View Controllers / Storyboards**

Inside of Xcode, there's a file called the Storyboard - this is where all the individual views that make up the app's user interface are laid out in a sequential order, with connection between them that signify transitioning between one view and the other. Each view has a corresponding file, usually named something like "MyViewController.swift" — the code that controls the interaction between the user interface of a view, and the logic / feature code of that view.

Not all of the UI is implemented from working in the storyboard file, however - oftentimes, when a user interface starts to grow more visually interesting; it gets somewhat difficult to implement it in the purely visual modality of storyboards, so the developer may end up implementing the interface partially (or at times fully) in code.

![XcodeStoryboard](/blog_assets/2019/XcodeStoryboard.jpg)

**SwiftUI**

One exciting direction that Apple is moving in is a new framework called [SwiftUI](https://developer.apple.com/xcode/swiftui/). It borrows a lot of concepts from reactive programming — user interfaces are able to be built in a way that's more *reactive* to external events (user input, network traffic, API calls, etc) in a way that is both more friendly to programmers writing the code (they don't have to write as much of it or try to take into account unknown information about the type of data that they're going to receive).

Right now, it's only available for apps that target iOS 13 or later. If your app needs to support a version of iOS older than iOS 13, you won't be able to take advantage of SwiftUI.

Eventually, these earlier versions of iOS will start to decline in percentage in the total number of iOS devices in worldwide use, and the oldest version of iOS that will be running on users' phones will be iOS 13, so it will be safe to use SwiftIUI for all apps that need to be backwards compatible.

Whenever Apple starts saying "you should start writing all of your new apps in Swift UI"...you should listen, because eventually the "should" will be replaced by "have to"...meaning, also, that any *existing* apps you have will need to be updated to use SwiftUI, too.

Apple's provided some great [resources for learning SwiftUI](https://developer.apple.com/tutorials/swiftui), and there's a great community sourced set of them [here](https://github.com/vlondon/awesome-swiftui), too.

&nbsp;

## Managing Source Code

**Shared Code**

You may hear developers reference things like "core code", "shared libraries", "core libraries" or the like - they're most likely referencing code that is shared between iOS and Android, or in some cases, between an MacOS app and an iOS app. This often, but not always, encompasses code that contains core functionality or perhaps core IP that the apps are centered around.

**Package Managers / Dependencies**

No one likes to reinvent the wheel, especially developers - good developers are often extremely lazy, in the sense that if there's something that they don't have to write code to do, then they won't do it.

One unfortunate downside to having too many dependencies is that if there's no documented process for how to properly add a new one or how a new team member can clone the repository that contains dependencies with various methods of managing them, then there becomes a sort of time trap where new people have to figure out and guess how to setup the project on their machine. Anyone who does know how has a sort of "tribal knowledge" that they then have to spend time passing this info on to new team members if it isn't written down anywhere.

There are a few different ways to manage dependencies in the iOS ecosystem:

- [CocoaPods](https://cocoapods.org/)
- [Swift Package Manager](https://swift.org/package-manager/) (specific to Swift code only)

One that's not specific to the iOS ecosystem though is the use of [git](https://git-scm.com/book/en/v1/Getting-Started-Git-Basics) submodules; which is a way to use git to add in code from other, existing source-code repositories to their project.

- [Git Submodules](https://chrisjean.com/git-submodules-adding-using-removing-and-updating/)

**Git/SourceControl**

Git is the system that's most commonly used for source control / checking code into a project.

You hopefully won't ever really have to do with git much other than if you want to pull the latest changes to the code that the engineering team is working on. To do this, you'll need to have git installed on your computer. Follow the instructions [here](https://gist.github.com/derhuerst/1b15ff4652a867391f03#file-mac-md) to get it set up on your computer.

Generally companies have preferred programs/ways for interfacing with git, so best to just use whatever your company uses. There are also generally project specific instructions for how to clone the project (i.e. download all the source code, dependencies, etc etc onto your computer so the project can be compiled and run).

**Running the "in-development" version of the app on your computer**

You'll almost certainly want to be able to run the in-development version of your app for yourself so that you're able to see / test new changes, bug fixes, and features to app in a day-to-day setting.

You'll first need to [Download Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12).

Someone on your team will have to invite you to the company's Apple development team account, which, essentially, will give you "permission" to run the app on your device/the iOS simulator on your computer. The simulator is included when you download Xcode, and allows apps to be run on a simulated iPhone or iPad on your desktop, without having to run them on a physical iPhone or iPad.

Once you've been invited to the Apple development account, you'll have the ability to upload app updates to the Apple App Store, should that end up being part of your job.

Click on Preferences...

&nbsp;

![XcodePreferences](/blog_assets/2019/XcodePreferences.jpg)

&nbsp;

Click on Accounts, then on the "+" button the bottom left corner

&nbsp;

![XcodeAccounts](/blog_assets/2019/XcodeAccounts.jpg)

&nbsp;

Click on Accounts, then on the "+" button the bottom left corner

You'll be prompted to sign in with your Apple ID - once that's down, click on "Download Manual Profiles".

Assuming everything here worked, you should know be able to run the app from the simulator or on your own iPhone.

&nbsp;

## App Store Distribution

**Getting your app to Apple**

When you update an app, you can select the update to go live immediately after Apple has approved it, to go live at a specific date/time, or to be manually set live by someone on the team

Basically, when an update has been certified by the as "feature complete" by a manager, it enters a code freeze (no more features/bug fixes get added to that version, which is being submitted to the app store)

A binary gets created (essentially, all the source code, frameworks, assets get bundled up into what's called an IPA (iOS App Store Package) file, which is then uploaded to the app store. Various different workflows for this; sometimes a lead engineer or eng manager will handle this process; sometimes a PM; all depends on the team.

Various automated checks from Apple servers are performed during this process — if they detect any errors, those have to get corrected and then the app gets re-uploaded. Once the file is uploaded, it enters review by Apple (there's a team at Apple that manually reviews all of these).

New screenshots may need to be added if there are big UI changes; also having a text summary so that the Apple testers know what's new / what may need to be tested.

Video recordings of the app are helpful too; especially for "bigger" changes since the previously released version. [Reflector](https://www.airsquirrels.com/reflector) is a good app for this; it let's you mirror your phone running the iOS app  to your laptop, and recored a video of the phone.

If there are major changes to the app since the last time it was updated, my experience has been that having these things + making sure they are very clear helps speed the review process up quite a bit than if they had not been there

**Apple Beta Updates**

Apple beta versions of their pre-release software to developers, so that they're able to have a head-start for updating their apps to be able to work with the new changes that Apple will be releasing, and to make sure they are able to conform to any new requirements that Apple may specify that they need to meet. Developers are able to download iOS beta versions onto iOS devices, and able to download the corresponding beta versions of Xcode and the iOS SDK (Software Development Kit; the suite of software libraries used to develop iOS apps) to develop for  the new version.

&nbsp;

## Further Education

Staying up to date on everything that Apple announces during their WWDC (worldwide developers conference) event is a good idea - things to be aware of as far as new features for iOS / relevant devices. All the videos from WWDC events are [here](https://developer.apple.com/videos/wwdc2019/](https://developer.apple.com/videos/wwdc2019/)); grouped by date and topic.

You can also [download this app](https://apps.apple.com/us/app/wwdc/id640199958) from Apple and watch all the WWDC videos from there on your phone/iPad.

[Ray Wenderlich](https://www.raywenderlich.com/) has probably one of the best tutorial sites on the internet for iOS development, as well as other topics like Android development, game development in Unity, etc.
