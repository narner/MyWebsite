



```
title: "Distributing Frameworks via Private CocoaPod"
date: "2022-18-06"
```



If you're developing an iOS SDK that you want to license and distribute to customers, and don't want the framework to be publically available for download, you can use a private CocoaPod to distribute it. 

Once you've finished writing your framework* and it's ready for distribution, you'll first want to create a private repository that has your [.podspec file](https://guides.cocoapods.org/syntax/podspec.html). 

Here's an example `.podspec` file that 









// distributing the framework in the repo 

// pod spec 







// pod file for the project you wish to use the private pod in

















**You'll probably want to bundle it as an [XCFramework](https://help.apple.com/xcode/mac/11.4/#/dev544efab96) (a framework that is compiled to run both on iOS simulators and physical iOS devices), you can distribute it via a private CocoaPod.*







