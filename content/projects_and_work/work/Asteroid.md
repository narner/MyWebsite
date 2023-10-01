---
title: "Asteroid"
date: "2023-04-13"

---

I was the founding employee of a pre-seed startup called [Asteroid](https://www.producthunt.com/products/asteroid). Our product was a native macOS app, built in Swift, that let users create ARKit apps without having to write any code. 

It had a node-based interface, inspisred by Max, Quartz Composer, and Origami. Users would create "reactions";  building blocks of functionality that notices something happening via an input object, and triggers something happening in 3D.

![Reactions](/post_assets/asteroid/Reactions.png)

I built the much of the app including the node-based interface, project export, and various nodes; including the camera, audio, and CoreML nodes. 

![AsteroidNodeInterface](/post_assets/asteroid/AsteroidNodes.gif)

When you had created your prototype on the graph, it could be exported out of the app as an Xcode project. It would contain a framework we wrote that would bundle the code for your prototype in an iOS app. You could then run the app on your phone, and modify the project to create a fully-featured app. 

You can check out the ProductHunt launches [here](https://www.producthunt.com/products/asteroid) and [here](https://www.producthunt.com/products/asteroid#asteroid-2) 

![ProductVideo](/post_assets/asteroid/AsteroidProductVideo.mp4)
