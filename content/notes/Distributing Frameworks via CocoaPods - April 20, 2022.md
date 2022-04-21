---
title: "Distributing Frameworks via CocoaPods"
date: "2022-04-20"

---



If you're developing an iOS SDK that you want to be able to license and distribute to customers via [CocoaPods](http://cocoapods.org), but don't want to be able to have your source code publically available; you can distribute it as a framework that CocoaPods will install in your project. 

Once you've finished writing your framework* and it's ready for distribution, you'll first want to create a public repository that has your [.podspec file](https://guides.cocoapods.org/syntax/podspec.html), and the framework you want to distribute. Name it something like "MyFramework-Podspec". The one below can serve as a template:

```ruby
Pod::Spec.new do |spec|
  spec.name               = "MyFramework"
  spec.version            = "0.0.1"
  spec.platform = :ios, '15.2'
  spec.ios.deployment_target = '15.2'
  spec.summary            = "My Framework"
  spec.description        = "My Framework for doing Blah"
  spec.homepage           = "https://www.nickarner.com"
  spec.documentation_url  = "https://www.nickarner.com"
  spec.swift_versions = '5.0'
  spec.license = { :type => 'Commercial', :text => 'See www.nickarner.com' }
  spec.author             = { "Nick Arner" => "..." }
  spec.swift_version      = "5.3"
  spec.source            = { :http => 'https://github.com/my-org/my-framework-podspecs/releases/download/0.0.1/MyFramework.xcframework.zip' }
  spec.dependency 'Alamofire' , '~> 5.4.3' 
  spec.ios.vendored_frameworks = 'MyFramework.xcframework'
end
```

The key lines here are `spec.source` and `spec.ios.vendored_frameworks`. The first one is the field for specifiying that the location of the framework to be downloaded and used for compiling the CocoaPod. Rather than specifying the source as Swift files that would be downloaded and compiled, as you would if you were making your entire pod open-source; here we're specifying the path to our `xcramework` that will be used to build our CocoaPod. 

`spec.ios.vendored_frameworks` specifies that we are going to use the framework we've included in the compilation step for building the CocoaPod.  

*For more information on configuring your `.podspec` file, see the [Podspec Syntax Reference](https://guides.cocoapods.org/syntax/podspec.html).* 

Once you've created your `.podspec` file, go ahead and commit it to your repository. 

Zip your `.xcframework` file, and upload it to the Releases section of your repository:

![GitHubRelease](/blog_assets/2022/GitHubReleases.png)

The link to this file is what we'll use for the `podspec`'s `spec.source` parameter. 

Now, the part that tripped me up was that it appears you also have to include the a copy of the `.xcframework` file in your repository *in addition to uploading it as a release*: 

![GitHubRelease](/blog_assets/2022/Framework.png)



*You'll probably want to bundle it as an [XCFramework](https://help.apple.com/xcode/mac/11.4/#/dev544efab96) (a framework that is compiled to run both on iOS simulators and physical iOS devices), you can distribute it via a private CocoaPod. Here's a sample script you can modify and run in the root of your project to build an `.xcframework`*:

```bash
xcodebuild archive \
  -scheme MyFramework \
  -archivePath "archives/MyFramework-iOS.xcarchive" \
  -destination "generic/platform=iOS" \
  -sdk iphoneos \
  SKIP_INSTALL=NO \
  BUILD_LIBRARY_FOR_DISTRIBUTION=YES

xcodebuild archive \
  -scheme MyFramework \
  -archivePath "archives/MyFramework-iOS-simulator.xcarchive" \
  -destination "generic/platform=iOS Simulator" \
  -sdk iphonesimulator \
  SKIP_INSTALL=NO \
  BUILD_LIBRARY_FOR_DISTRIBUTION=YES

xcodebuild -create-xcframework \
  -framework "archivesMyFramework-iOS.xcarchive/Products/Library/Frameworks/MyFramework.framework" \
  -framework "archives/MyFramework-iOS-simulator.xcarchive/Products/Library/Frameworks/MyFramework.framework" \
  -output "MyFramework.xcframework"

```
