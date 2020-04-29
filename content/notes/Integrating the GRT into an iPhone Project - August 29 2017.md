---
title: "Integrating the GRT into an iPhone Project"
date: 2017-08-29
---

In this blog post, I'll show you to add the [Gesture Recognition Toolkit](https://github.com/nickgillian/grt) to an iPhone app. Created by [Nick GIllian](http://www.nickgillian.com/) while he was a post-doc at MIT, the GRT is a "cross-platform, open-source, C++ machine learning library designed for real-time gesture recognition".  I had known from this [issue on GitHub](https://github.com/nickgillian/grt/issues/24) that the GRT has been used in iOS development. I was only able to find [one example](https://github.com/magnusja/WatchGRT) of this in action, which this guide is partially based upon.

To add the GRT to an iOS project, we're going to create a CocoaTouch framework from the GRT source files. We'll also include an Objective-C++ wrapper that will allow us to easily interface between the framework, and the Swift code in our application. To get started, we need to do a bit of set-up work first.

First, create the folder where you want your project to live. Clone GRT as a submodule by using the commands:

- git submodule init
- git submodule add https://github.com/nickgillian/grt.git

Create a new Xcode project. Set the target be a CocoaTouch Framework. Note that if you want to use the GRT in a watchOS project, the steps below will be the same, just with a watchOS Framework target, rather than a CocoaTouch Framework target:

![iOS+CocoaTouch+Framework](/blog_assets/2017/iOS+CocoaTouch+Framework.jpg)

Note that if you want to use the GRT in a watchOS project, the steps below will be the same, just with a watchOS Framework target, rather than a CocoaTouch Framework target:

![watchOS+CocoaTouch+Framework](/blog_assets/2017/watchOS+CocoaTouch+Framework.jpg)



Copy the GRT folder into the project, making sure that the files are added to the framework target.



![GRT+folder](/blog_assets/2017/GRT+folder.jpg)

"Copy items if needed" should be selected.

![Copying+to+framework](/blog_assets/2017/Copying+to+framework.jpg)

In order to access the GRT from an iOS app, we'll need to create an Objective-C wrapper. This will allow us to call down into the GRT C++ code from Objective-C or Swift. [Magnus Jahnen](https://github.com/magnusja), the author of the previously mentioned, watchGRT repository, created [some wrapper files](https://github.com/magnusja/grt/tree/501160c5883aa030235edaaeab43525866134c6d/iOS/ObjCWrapper) which I've modified for this particular project. You can grab these modified files here and add them to your project,

Essentially, what we're doing is allowing a GRT pipeline to be created by the framework from our Swift-based app. The [GestureRecognitionPipeline](https://github.com/nickgillian/grt/blob/master/GRT/CoreModules/GestureRecognitionPipeline.h) is the core object of the GRT. It allows you to set-up a gesture recognition system with modules that will perform [pre-processing](https://github.com/nickgillian/grt/tree/master/GRT/PreProcessingModules) on incoming data (say, from an iPhone's accelerometer or gyroscope), configure a [classification module](https://github.com/nickgillian/grt/tree/master/GRT/ClassificationModules) for identifying the gesture performed, and then applying any [post-processing](https://github.com/nickgillian/grt/tree/master/GRT/PostProcessingModules) needed to make sure that

The init method of our GestureRecognitionPipeline.mm file call's down to the GRT code, and creates an instance of a pipeline:

```
- (instancetype)init
{
    self = [super init];
    if (self) {
        self.instance = new GRT::GestureRecognitionPipeline;

        // Redirect cout to NSLog
        self.nsLogStream = new NSLogStream(std::cout);
    }
    return self;
}
```



The last step here is to compile the framework. Note that if you make any changes to the Objective-C wrapper, you will need to compile the framework again - whatever functionality from the GRT that you want to add to your app will need to be exposed accordingly in the wrapper.

Now that we've finished setting up the framework, we can go ahead and add it to an iOS project.

Create a new target for an iOS app. I've named mine "GRT-iOS-HelloWorld":

![HelloWorld+App](/blog_assets/2017/HelloWorld+App.jpg)

We'll go ahead and add the framework to our new app:

![Adding+framework](/blog_assets/2017/Adding+framework.jpg)

Finally, let's import the GRT-iOS framework, to verify that we can create objects from the GRT. Create a Bridging-Header file, and add this line to it:



```
#import <GRTTiOS/GRTiOS.h>
```



Now, we can create an instance of a GRT Pipeline in our view controller's viewDidLoad method:



```
let pipeline = GestureRecognitionPipeline()
```



Build and run the app to verify that there are no compile errors. If not, then you've successfully added the GRT to your iPhone project!

The app doesn't do anything yet; in a future post, I'll show how I implemented a real-time gesture recognition system using the GRT on iOS. You can download the project [here](https://github.com/narner/GRT-iOS-HelloWorld).
