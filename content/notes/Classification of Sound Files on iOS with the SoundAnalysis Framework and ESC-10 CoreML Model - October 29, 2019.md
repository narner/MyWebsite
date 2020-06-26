---
title: "Classification of Sound Files on iOS with the SoundAnalysis Framework and ESC-10 CoreML Model"
date: 2019-10-29
---

Earlier this year, Apple introduced the [SoundAnalysis framework](https://developer.apple.com/documentation/soundanalysis), enabling apps to “analyze streamed and file-based audio to classify it as a particular type”.

It’s available in the following SDK’s:

- iOS13+
- macOS 10.15+
- Mac Catalyst 13.0+
- tvOS 13.0+
- watchOS 6.0+

### **INPUT TYPES**

The SoundAnalysis framework is able to work with live audio via the device microphone, or from pre-recorded audio files. In a real-world application scenario, these could be either audio files downloaded from a server onto the app, or recorded by the app for later analysis.

However, at the time of writing, I was not able to get live audio input working with Sound Analysis after following Apple’s guide. Here’s a [StackOverflow post](https://stackoverflow.com/questions/58496448/error-updating-tree-format-when-using-ios-soundanalysis-framework?noredirect=1#comment103345155_58496448) with what I was working through for future reference.

Should I get live audio input working, I’ll update the blog post / project with an additional example. For now, this project demonstrates how to use SoundAnalysis off of audio files only.

###  **DATASET**

The dataset I used is the ESC (environmental sound classification)-10 dataset; the same one that Apple uses in their [TuriCrate tutorial on Sound Classification](https://apple.github.io/turicreate/docs/userguide/sound_classifier/). The ESC-10 is a subset of the [larger ESC-50 dataset that’s available on GitHub](https://github.com/karoldvl/ESC-50).

The dataset consists of ten classes, with ten files for each class. The classes are:

1. Dog Barking
2. Rain
3. Sea Waves
4. Crying baby
5. Clock Tick
6. Person Sneezing
7. Helicopter
8. Chainsaw
9. Rooster
10. Fire Crackling

This is not a very large collection of audio files for an extremely robust dataset, and the classes aren’t particularly interesting in and of themselves without a larger context — the point of the project is to demonstrate the concept of how a sound classification dataset would be used with CreateML to create a CoreML model in conjunction with the SoundAnalysis framework. If you were building a custom sound classification recognition model for use in a production app, you would most likely need to create your own custom dataset that uses audio data that is close to the real world scenario in which your app would be used for.

There are some other audio-focused datasets available; a compilation of some can be found [here](http://www.cs.tut.fi/~heittolt/datasets), [here](https://towardsdatascience.com/a-data-lakes-worth-of-audio-datasets-b45b88cd4ad), and [here](https://cassebook.github.io/ch06/index/). However, you’ll need to make sure that you check what the licenses are for each dataset before you use them to create a model that can be used in a commercial application.

###  **CREATEML PROJECT**

The [CreateML app](https://developer.apple.com/documentation/createml) that’s included in Xcode allows developers to create CoreML models that recognize sounds without writing any code.

With this [new template from CreateML](https://developer.apple.com/videos/play/wwdc2019/425/), you can make a machine learning model that can recognize types of sound events without having to write any code, and use the interface to do so in CreateML that you would use to make ML models that are trained on visual, tabular, or sensor data.

In addition to the ECS-10 CoreML model, I’ve included the CreateML project used to make the model in this repository.

![CreateMLInterface](/blog_assets/2019/CreateMLInterface.jpg)

​		CreateML Training Interface



The ECS-10-CoreML-Demo is an Xcode project that demonstrates how to use the SoundAnalysis framework in an app.

The app will randomly select from ten pre-loaded .wav files and play them through the speakers (using [AudioKit](http://audiokit.io/)), while running an analysis with the SoundAnalyzer framework. Setting up this pipeline takes remarkably few lines of code:



```
    var model: MLModel!
    
    //Access the bundled CoreML model
    let soundClassifier = ESC_10_Sound_Classifier()
    model = soundClassifier.model

    // Create a new audio file analyzer.
    do {
    audioFileAnalyzer = **try** SNAudioFileAnalyzer(url: audioFileURL)
    } catch

    // Create a new observer that will be notified of analysis results.
    let resultsObserver = ResultsObserver()
    // Prepare a new request for the trained model.

    do {
    	let request = try SNClassifySoundRequest(mlModel: model)
    	try audioFileAnalyzer.add(request, withObserver: resultsObserver)
    } catch

		// Analyze the audio data.
		audioFileAnalyzer.analyze()
```



### **HERE’S A VIDEO OF THE TEST APP IN ACTION:**

The project can be found on GitHub [here](https://github.com/narner/ESC10-CoreML).

[![](http://img.youtube.com/vi/dAtzSo51T_4/0.jpg)](http://www.youtube.com/watch?v=dAtzSo51T_4 "")
