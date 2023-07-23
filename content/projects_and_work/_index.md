---
title: "Projects"
date: "2022-01-01"
layout: "has-contents"
---


## Work

## Personal

## Open source



**Detecting ASL Signs on iOS** | 2021

I built a demo app that can detect American Sign Language signs using computer vision. The CoreML model was created using David Lee's American Sign Language Letters Dataset, which is hosted on Roboflow [here](https://public.roboflow.com/object-detection/american-sign-language-letters). The GitHub repo for the app is [here](https://github.com/narner/ASL-Classifier-Demo).

**Dyscalculia Tester App** | 2020

[Dyscalculia](https://www.dyscalculia.org) is a learning disorder that results in difficulty learning or comprehending mathematics (sometimes called "math dyslexia"). I built a prototype of an iOS app consisting of [two types of tests](https://www.youtube.com/watch?v=p_Hqdqe84Uc&t=231s) for dyscalculia developed by cognitive neuroscientist [Dr. Brian Butterworth](https://www.dyscalculia.org/experts/brian-butterworth). GitHub repo [here](https://github.com/narner/DyscalculiaTester).

---

![Judah](https://picsum.photos/200/300 "l-float") **Peer-to-Peer Audio Chat iOS App** | 2019

I built a local peer-to-peer, push-to-talk audio chat app. You could create audio channels with other people who were in the same relative geographic location as you - useful for situations like production sets, construction sites, etc. Read more [here](/projects_and_work/push_to_talk_audio_chat_app/). 

--- 

**Whistlr - Audio QR-Code Contact Sharing iOS App** | 2017

Whistlr was an app that would let people share their contact cards with each other via audio signals - sort of like [Bump](https://en.wikipedia.org/wiki/Bump_(application)), but with audio. It used the [ChirpSDK](https://github.com/chirp), which allows for data to be transmitted via tones as an audio QR code. Read more [here](/projects_and_work/whistlr/). 

**Project Soli Alpha Developer Program** |  2016-2017

Google ATAP had a program where third-party developers could get access to the development kit of a new  mm-wave radar sensor called [Project Soli](https://atap.google.com/soli/), and I was accepted to participate. I built some gesture-controlled musical instruments, which were [shown at Google I/O](https://www.youtube.com/watch?v=H41A_IWZwZI); and experimented with gesture controlled interactive graphics. Read more [here](/projects_and_work/o_soli_mio/).

**Swept Frequency Capacitive Sensing (Touché) Experiments** | 2017 

I build a homemade version of Disney Research's [Touché swept-frequency capacitive sensor](https://la.disneyresearch.com/publication/touche-enhancing-touch-interaction-on-humans-screens-liquids-and-everyday-objects/) and made some demos that replicated the results in Disney's academic publications showing how this type of sensor could be used to make interactions between human gesture and plants / water. 

GitHub repo [here](https://github.com/narner/Touche-Experiments) + blog post [here](/projects_and_work/emulating_touch%C3%A9/)

**Detecting Flame Strength with a Bluetooth Sensor and iPhone** | 2017 

I wanted to learn about Bluetooth, so I built a system that would transmit data from a flame sensor to an iOS app using Bluetooth LE. The hardware setup used a flame sensor and an [Arduino Flora board](https://www.adafruit.com/product/659). The iOS app was written in Swift and used CoreBluetooth for the data communication. 

GitHub repo [here](https://github.com/narner/iOS-FlameSensor-Bluetooth-Study) + blog post [here](/notes/integrating-arduino-bluetooth-sensors-with-ios-september-5-2017/).

**Analog and NeoPixel LED Strip Controls with Arduino**| 2017 

At the time, I was prototyping some physical product ideas that needed a lighting system. I built a multiplexing system for controlling a strip of analog LED's, as well as a digital Adafruit NeoPixel strip. GitHub repo [here](https://github.com/narner/Analog-and-NeoPixel-LED-Strip-Control). 

**ML-based Gesture Recognition on iOS** | 2017 

In the pre-CoreML era, I built an app that could detect the type of gestures you were making with your iPhone via the CoreMotion framework's access to the phone's accelerometer. It utilized a C++ library called the [Gesture Recognition Toolkit](https://github.com/nickgillian/grt). I wrote an Objective-C++ wrapper around the library for use with Swift, and built an app with both on-device training + real-time prediction modes.  

GitHub repo [here](https://github.com/narner/GRT-iOS-HelloWorld) + blog post [here](/notes/machine-learning-powered-gesture-recognition-on-ios-october-7-2017/).

**Arduino Controlled macOS Synthesizer** | 2015 

I built an AudioKit-powered macOS synthesizer that was controlled by an Arduino circuit. Data from the potentiometers and switch connected to the Arduino was read by the macOS app over the USB serial port, and used to control the synthesizer parameters. GitHub repo [here](https://github.com/narner/Arduino-AudioKitOSX). 

**AudioKit** | 2014-2016

[AudioKit](https://audiokit.io) is an open-source framework for audio synthesis, analysis, and production for Apple platforms. I joined the AudioKit team prior to its public launch, and was part of the project for three years. I created example projects, tutorials, and website content; as well as features like framework testing and Sensible Defaults - presets for a variety of audio synthesis objects that when used by a developer, would allow for a good template to start with.

You can check out the first version of the project website [here](https://web.archive.org/web/20141108033113/http://audiokit.io/).

**Delay Tracker: iOS App for Reporting Amtrak Delays** | 2015

Delay Tracker was the first iOS app I built. It's purpose was to let Amtrak travelers who were experiencing train delays to report them to the Surface Transportation Board, the federal agency charged with regulation of the nation's railroads. You can read more about it [here](/projects_and_work/delay_tracker/).  
