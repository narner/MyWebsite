---
title: "Machine Learning Development on Apple Platforms"
date: 2019-10-14
---

Apple has been investing heavily in machine learning (ML) capabilities for its development platforms. Acquisitions of applied ML startups allows Apple to fold their technology into the hardware, operating system, and software development tooling for their products. Creating developer friendly frameworks makes it easy to leverage the ML capabilities of Apple’s computing devices, allowing for a greater number of apps that can apply ML in a wider array of use cases.

It's up to app developers to creatively make use of these powerful capabilities. A wide range of available input data (video, audio, text, and sensors) allows developers to create compelling apps in sectors as varied as health care, augmented reality, and creativity tools.

This post discusses how ML development fits into the Apple development ecosystem. We’ll look at frameworks and tools Apple has introduced in the last few years and how they fit into the broader world of ML.

## ON-DEVICE INFERENCE

"On-device inference" is basically a techno-speak way of saying "the ML model performs a prediction on incoming data without sending the data to a server running in the cloud somewhere” or “the data used for prediction never leaves the user's device."

In apps that use ML without on-device inference, data is sent to a cloud server that's hosting a ML model. The model performs predictions based on the received data, and then sends the results back to the application running on a user's device.

One major benefit to on-device inference is that it can be performed with poor or non-existent network connectivity.

## TOOLS FOR WORKING WITH MACHINE LEARNING ON APPLE PLATFORMS

### COREML

[CoreML](https://developer.apple.com/documentation/coreml) is the framework for running ML models on an Apple device. No data is sent off the device to a cloud server that's processing user data — all of the inference is done with some combination of the CPU, GPU, and the Neural Engine (silicon that's part of the A11 Bionic and newer processors that's designed to accelerate certain ML tasks).

### EXISTING COREML MODELS

Apple has created a variety of [CoreML models for developers](https://developer.apple.com/machine-learning/models/) to easily get started working with on-device ML straight away. These models include object detection, image segmentation, drawing prediction, and others.

![ImageClassification](/blog_assets/2019/ImageClassification.jpg)



​						Image Classification - SqueezeNet Model

![Segmentation](/blog_assets/2019/Segmentation.jpg)



​						Image Segmmentation - Deeplab V3 Model

![ObjectDetectionRealTime](/blog_assets/2019/ObjectDetectionRealTime.jpg)

​							Object Detection - YOLOv3 Model



### TURICREATE

[TuriCreate](https://github.com/apple/turicreate) is a Python library that allows developers to build custom ML models of several different types:



![TuriCreateModelTypes](/blog_assets/2019/TuriCreateModelTypes.jpg)	



Once the ML model has been created with TuriCreate, it can then be converted to a CoreML model that's ready to be deployed in an app.

ML developers that are familiar with existing Python-based ML tools like TensorFlow or PyTorch should find TuriCreate to be familiar.

For a detailed walkthrough of how to use TuriCreate, take a look at this Apple [presentation](https://developer.apple.com/videos/play/wwdc2018/712) from last year's WWDC event.

TuriCreate is the result of [an acquisition Apple made in 2016 of Turi](https://techcrunch.com/2016/08/05/apple-acquires-turi-a-machine-learning-company/), a Seattle-based ML startup that had described itself as "a ML platform for developers and data scientists."



## **CREATEML DEVELOPER TOOL**

Apple recently released a developer tool bundled with Xcode 11 called CreateML:

![CrateMLLocation](/blog_assets/2019/CrateMLLocation.jpg)

It allows developers create CoreML models through a drag and drop interface; models are built from image, sound, sound, motion, text, and tabular data sources.

For more on what you can do with this tool, check out this [presentation](https://www.notion.so/narner/Machine-Learning-Development-on-Apple-Platforms-a690f86d419242c5a3964a405a19fac4) from the most recent WWDC.

Apple is putting a lot of money and hours into making frameworks and tools that help non-ML experts create models that can run on their platforms. I don't think it's a stretch to say that CreateML could end up allowing users to create all of the ML model types that TuriCreate supports without the need for writing any code.

### CONVERTING EXISTING MODELS TO COREML

There are a variety of open-source tools for converting pre-trained ML models into CoreML models that can then be used in an app. Apple's open-source CoreML Community Tools project, [coremltools](https://github.com/apple/coremltools), provides scripts for converting all of the below into a CoreML model format:

- scikit-learn
- LIBSVM
- Caffe
- Keras
- XGBoost

Gaël Foppolo has a [blog post](https://blog.gaelfoppolo.com/introduction-to-core-ml-conversion-tool-d1466bf10018) with a walkthrough on how to use coremltools to perform model conversion.

One important thing to note is that not all model architecture types are able to be automatically converted to CoreML! Be sure to check [this document from Apple](https://developer.apple.com/documentation/coreml/converting_trained_models_to_core_ml) to make sure that the model you want to convert is able to be converted. If it is not covered, you may have to use open source third party tools or write your own custom conversion code.

In addition to core-ml tools, there's an open source project for converting TensorFlow models to CoreML called [tf-coreml](https://github.com/tf-coreml/tf-coreml).

If you have an existing, pre-trained model that you want to convert to take advantage of Apple's neural engine hardware, you can convert it to CoreML using these tools. Additionally, if you're writing a model from scratch and have an existing machine-learning developing pipeline that makes use of frameworks like Caffe, Keras, TensorFlow; etc, you can continue to develop the model using them, and then convert the model to CoreML once it's completed.



## ON DEVICE TRAINING

[As of CoreML3](https://heartbeat.fritz.ai/whats-new-in-core-ml-3-d108d352e50a), CoreML now offers the ability to personalize ML models directly on a user's device — enabling the app to fine-tune the embedded ML model to adapt to the individual data of the user.

As a user is interacting with an app having a personalizable ML model, the app will analyzes user interaction data to tune the model to conform to the individual user.

These new capabilities enable a whole new frontier for how people can interact with their computing devices - their software will be able to adapt itself to their habits, behaviors, and tastes.

Apps making use of ML will increasingly be able to anticipate people's actions and desires, in a way that will be respectful of their privacy and the integrity of their personal data.

Matthijs Hollemans has [a multi-part, in-depth overview](https://machinethink.net/blog/coreml-training-part1/) of how CoreML's on-device training capabilities work.

## WHAT ABOUT TFLITE AND OTHER MOBILE FRAMEWORKS?

While cross-platform frameworks like TFLite or PyTorch Mobile are able to be deployed on Apple devices, they're not able to take advantage of the work that Apple has done at the framework level of enabling the operating system to dynamically determine whether a model gets run on the CPU, GPU, or Apple’s new Neural Engine.

There is no SDK or API for interacting with these chips directly - the operating system manages their use for the developer via CoreML. The developer is only concerned with implementing the ML model, while CoreML handles how that model gets run at the silicon level.

That said, it would be remiss not to mention some of the other frameworks that can be used to deploy ML models for devices in the Apple ecosystem:

### TENSORFLOW LITE

![TensorFlowLite](/blog_assets/2019/TensorFlowLite.jpg)

[TensorFlow Lite](https://www.tensorflow.org/lite) is a "lite" version of TensorFlow that's designed to run on IoT and mobile devices. Developers choose an existing pre-trained model, or create a new one with TensorFlow, and then use the TensorFlow Lite Converter to convert the model to run on TensorFlow Lite. The converter optimizes these pre-trained TensorFlow models to run on devices with a limited amount of compute and memory resources.

### PYTORCH MOBILE

The recently announced [PyTorch Mobile](https://pytorch.org/mobile/home/#targetText=PyTorch Mobile,deployment on iOS and Android) framework is designed to be a cross-platform library for deploying PyTorch models on both iOS and Android devices. A model would be created once in PyTorch, and then deployed via the respective platform-specific frameworks.

![PyTorchMobile](/blog_assets/2019/PyTorchMobile.jpg)

## FURTHER LEARNING AND GOOD WORK TO CHECK OUT

- The folks at [fritz.ai](http://fritz.ai/) have done an excellent job of publishing their findings, as well as curating [a fantastic newsletter](https://www.fritz.ai/newsletter.html) that showcases the very latest developments in mobile machine learning technology
- Matthijs Hollemans' [blog](https://machinethink.net/blog/) is an excellent resource for in-depth technical posts and examples on using CoreML in iOS projects.
- The [Awesome CoreML Models GitHub repository](https://github.com/likedan/Awesome-CoreML-Models) has, as it's name suggest, a list of really awesome open source CoreML models, as well as example projects that use them.
