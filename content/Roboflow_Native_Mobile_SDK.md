---
title: "Roboflow Native iOS SDk"
date: "2023-04-13"

---

In 2022 I lead the building of [Roboflow Native iOS SDK](https://blog.roboflow.com/roboflow-ios-sdk/). Roboflow's a startup building tools for the creation and deployment of Computer Vision ML models by developers without the need for specialized computer vision knowledge. Developers can create and host their models on the Roboflow platform, as well as deploy them on edge devices. 

![Roboflow-iOS-SDK](/post_assets/roboflow_ios_sdk.png)

The Native iOS SDK let iOS developers download the [CoreML](https://blog.roboflow.com/what-is-coreml/) version of their model created on Roboflow direclty to their device for use in their app. 

Additionally, I built [CashCounter](https://apps.apple.com/app/roboflow-cash-counter/id1633812788), an app availble on the App Store, that would identify US currency and display the total monetary value. If the model predicted an incorrect value, users could press the "Incorrect Count" button, which would upload the image frame to the Cash Counter [dataset](https://universe.roboflow.com/alex-hyams-cosqx/cash-counter) in order to gradually improve the model over time. 

![Roboflow-iOS-SDK](/post_assets/CashCounter-3.PNG)

