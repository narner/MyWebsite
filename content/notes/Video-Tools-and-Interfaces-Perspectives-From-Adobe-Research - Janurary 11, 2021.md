---
title: "Video Tools and Interfaces: Perspectives From Adobe Research"
date: 2021-01-11

---



I recently attended an online seminar hosted by [Mira Dontcheva](https://research.adobe.com/person/mira-dontcheva/) of Adobe Research. Mira is a principal scientist and research manager working in Human-Computer Interaction. Her seminar, titled “Video is the Core Communication Tool of the Future”, highlighted some of the work that Adobe Research is doing in that area. 

Mira was quick to point out that the title of the talk may already be inaccurate, given how the Covid-19 epidemic has so accelerated this technological trend. We’re on more video calls than ever before, Zoom is now a household name (and it’s stock shooting through the roof), doctors are having more telemedicine appointments, and students are raising their virtual hands on Zoom calls, rather than in-person. 

One participant on the call made a very good, and early point; that while the usage of video in communication is accelerating at a very rapid pace, that does not mean it is the end all and be all of communication going forward. Video has an immediacy that is useful for some types of communication contexts, but that immediacy may not be useful or even wanted for *all* types of communication. 

Two big questions around how we interact with video content surfaced:

1) How do we save info from video? How do we get information from video? 

2) How do we get information from video? 



For the first question, it seems that there are a variety of ad-hoc solutions that people use to do this, including; 

1) Taking notes while watching a video

2) Taking screenshots of the video 

3) Manually scrolling through a recording to find the part of the video you needed 



These are obviously not ideal for the people using them, because it means that while they’re watching a video, they have to be doing something else. 

As far as getting information from video content; machine learning and computer vision techniques for extracting meaning from video and audio are becoming more advanced, and able to detect a variety of semantic meaning types from video content. Some companies working on this include[ Otter.ai](https://otter.ai/) for transcription from video, and [Viovr](https://www.vidrovr.com/); a platform for making video searchable.

Being able to get that information, and exposing it to a user via basic User-Interface primitives, will allow for users to get more value from interacting with video content.

These primitives include: 

* A Table Of Contents for video, so that you can easily skip to the relevant section in the video right away. 
  * This is [something that YouTube now supports](https://www.youtube.com/watch?app=desktop&v=G-gFSVqlBvs); and will probably become more common with other video tools.

* Making video translatable to documentsCTRL-F for video, so you can find specific things in a video file down to the frame level 

* Bookmarks for video 



These primitives can be created by leveraging computer vision and machine learning techniques such as: 



**Object Recognition (including People Detection)**

![Object Recognition](/blog_assets/2020/object_recognition.jpeg)

https://en.wikipedia.org/wiki/Object_detection



**Motion Understanding**

![Motion Understanding](/blog_assets/2020/motion_understanding.png)

https://www.hao-jiang.net/images/cvpr2011.png



**Scene Understanding**

![Scene Understanding](/blog_assets/2020/scene_understanding.jpeg)

https://cse.buffalo.edu/~jcorso/r/snippets.semantic_segmentation.html



**Audio Transcription**

![Scene Understanding](/blog_assets/2020/audio_transcription.png)

https://medium.com/@ageitgey/machine-learning-is-fun-part-6-how-to-do-speech-recognition-with-deep-learning-28293c162f7a



## ADOBE RESEARCH PROJECTS

Mira presented some of the work that was being done at Adobe Research in the area of intelligent video interaction. 

* [B-Script](https://berndhuber.github.io/bscript/), which searches for contextually relevant B-Roll (background footage placed under voiceover dialog) based on the position in a video transcript. When a word or group of words is highlighted, a search interface will search for possible video options from social media and professional stock footage libraries that the user can use as B-Roll for their video. 
* [MixT](http://dontcheva.org/pubs/2012/uist12_MixT_chi.pdf), a system for automatically generated step-by-step tutorials from video of users demonstrating concepts 
* [Temporal Segmentation of Creative Live Streams](https://ailiefraser.ca/LiveStreamVideoNavigation_CHI2020.pdf), a system for segmenting live-stream video into usable segments based on audio transcripts
* [Interactive Body-Driven Graphics for Augmented Video Performance](https://rubaiathabib.me/2019/03/04/body-driven-graphics/), a tool that maps gesture and body language to interact with graphical elements 



All of these research projects leverage computer vision and ML techniques to detect meaning from video content, and present it to users in a way that can add more meaning and context than just a statically played-back video. 



## VIDEO TO AUGMENTED-REALITY TUTORIALS

During the webinar, we also discussed Augmented-Reality as a way to interact with video content. Augmented-Reality provides a way to bring video content out of a flat computer screen and into the physical, real world. It has the potential to take video content about and set in the real world, and actually *put that content in the real world* by displaying the videos in context around the objects that they are about. Instead of watching a cooking tutorial on your laptop and then cooking the meal in your kitchen, you would be able 

Some of the work I did at a previous startup, Asteroid, involved prototyping video content interactions for AR. While experimenting with various headsets and mobile AR usage, we came to the conclusion that that AR was great for displaying multimedia content. 

Part of our thesis was that video manipulation and tutorials are going to be “the killer app” for consumer AR. Time will tell if that is indeed the case. I do expect that as Apple and Facebook roll out their consumer AR offerings, there will be more work done in this space; either from them or the people building on top of their platforms. 



## FUTURE DIRECTIONS

Video is becoming more and more of a ubiquitous feature across all kinds of applications and industry sectors. One only needs to look at the success of Zoom, and the current circumstances of the pandemic, to see the opportunity of video startups. We will see more startups focused on not just providing video capabilities across application stacks, but also working on making it easier to interact with and manipulate video. These will be built around being able to extract information using cutting-edge computer vision and machine learning techniques...making it so that software can understand the content that is in video; to search for people, objects, spoken dialog, or text in a scene, and using that information to let users make decisions about how they’re making the video. 

This will allow people working with video to spend more time on the creative aspects of working with video, and less on manually searching for specific content in the video they’re working with. The teams of startups working in this area will have a mix of machine learning, computer vision, and video production / editing experience. 

The interfaces for working with video that make use of these new ML and CV techniques in such a way that they make it easy for the user to see and search for the semantic information that is locked in the videos that people are working with. These interfaces don’t have to be radically different from the ones we have today...in fact, many will be able to make use of the familiar paradigms I mentioned at the beginning:

* A Table Of Contents for video

* Making video translatable to interactive documents

* CTRL-F for video, so you can find specific things in a video file down to the frame level 

* Bookmarks for video 

Working with video should be as easy as working with text in a word processor. We’re on the path towards this, but we still have a ways to go. 