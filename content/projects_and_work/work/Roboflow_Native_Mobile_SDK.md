---
title: "Roboflow Native iOS SDK"
date: "2023-04-13"

---

In 2022, I led the building of the [Roboflow Native iOS SDK](https://blog.roboflow.com/roboflow-ios-sdk/). Roboflow is a startup that builds tools for the creation and deployment of Computer Vision ML models by developers, without the need for specialized computer vision knowledge. Developers can create and host their models on the Roboflow platform, as well as deploy them on edge devices.

![Roboflow-iOS-SDK](/post_assets/roboflow/roboflow_ios_sdk.png)

The Native iOS SDK allows iOS developers to download the [CoreML](https://blog.roboflow.com/what-is-coreml/) version of their model, created on Roboflow, directly to their device for use in their app.

Additionally, I built [CashCounter](https://apps.apple.com/app/roboflow-cash-counter/id1633812788), an app available on the App Store, that identifies US currency and displays the total monetary value. If the model predicts an incorrect value, users can press the "Incorrect Count" button, which will upload the image frame to the Cash Counter [dataset](https://universe.roboflow.com/alex-hyams-cosqx/cash-counter) to gradually improve the model over time.

![CashCounter](/post_assets/roboflow/CashCounter.png)

