---
title: "Project Soli & the Coming Use of Radar in Human-Machine Interfaces"
date: 2020-04-07
---

Radar is a 85 — year old technology that up until recently has not been actively deployed in human-machine interfaces. Radar — based gesture sensing allows user intent to be inferred across more contexts than optical-only based tracking currently allows.

Google’s use of Project Soli, a radar-based gesture recognition system, in the Pixel 4 series of phones is most likely the first step in further adoption of radar as an input for interacting with our devices.

**Background on Project Soli**

At Google I/O 2015, the ATAP (Advanced Technology and Projects) group [announced several new initiatives](https://www.youtube.com/watch?v=mpbWQbkl8_g). These included:

- [Project Abacus](https://9to5google.com/2015/05/29/smart-lock-passwords-is-cool-but-google-project-abacus-wants-to-eliminate-password-authentication/) — multi-factor user authentication based on user location, typing patterns, and voice patterns
- [Project Vault](https://techcrunch.com/2015/05/29/googles-project-vault-is-a-secure-computing-environment-on-a-micro-sd-card-for-any-platform/?ncid=rss&utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+Techcrunch+(TechCrunch)) — a secure computing environment on a MicroSD Card, for any platform
- [Project Jacquard](https://atap.google.com/jacquard/) — conductive thread embedded in mass-producible textiles to create wearable-based interactions
- [Project Soli](https://www.youtube.com/watch?v=0QNiZfSsPc0) — a tiny radar sensor that’s capable of micro-gesture detection

Of these, Jacquard and Soli are still active — Jacquard having been integrated into a variety of consumer products with fashion labels such as [Levi’s](https://atap.google.com/jacquard/collaborations/levi-trucker/), [Saint Laurant](https://atap.google.com/jacquard/collaborations/ysl/), and [Adidas](https://atap.google.com/jacquard/collaborations/gmr/).

After going through several prototype iterations, Google integrated Soli into the [Pixel 4 as part of the Motion Sense feature](https://www.blog.google/products/pixel/new-features-pixel4/), which allows the phone to start the process of facial authentication before the phone’s owner even has to touch their phone.

​	

![Soli_Prototypes](/blog_assets/2020/Soli_Prototypes.jpg)



​							Soli Prototypes (from: https://venturebeat.com/2019/01/02/google-gets-regulatory-approval-to-deploy-radar-based-motion-sensing-device-project-soli/



Shortly after the announcement at I/O, ATAP put out a call for third party developers to apply to the Alpha Developer program for Project Soli, as a way to gain feedback on their early stage development kit. I filled out an application to develop music — based interactions with Soli, and was accepted into the program.

I wrote more about my experience as a member of the alpha developer program [here](https://www.nickarner.com/blog/projectsoli); what I wanted to do with this blog post was provide more of an overview of the capabilities of millimeter-wave radar, and how they enable certain new experiences and experiments in the field of human-computer interaction.

There’s been several academic papers written exploring this area since Project Soli was announced exploring different application areas, so we’ll look at those; as well as a quick overview of what millimeter-wave radar is, and the types of properties afforded by it.

First though, let’s take a look at the first commercial product to use Project Soli, the Pixel 4.

### PIXEL 4 AND MOTION SENSE: CAPABILITIES

The first commercial product to integrate Project Soli is the Pixel4, released by Google in October, 2019. The teaser-ad hinted that the new phone would be the first product to integrate with Soli; given the touchless air gestures shown in it:



[![](http://img.youtube.com/vi/KnRbXWojW7c/0.jpg)](http://www.youtube.com/watch?v=KnRbXWojW7c "")





The Soli chip affords **three new types of capabilities** for the Pixel 4:

**Presence** — thanks to radar’s ability to detect motion in the nearby area from where it’s placed, the Pixel 4 will turn off the always-on display if the user of the phone isn’t nearby while it’s placed on a table; therefore being able to both save battery power and not intrude upon the user’s attention

**Reach** — the Soli sensor will detect if a hand is moving towards it; waking up the screen and activating the front facing cameras for face-unlock

**Gestures**

-  Flick
- Presence
- Reach
- Swipe

9 to 5 Google did an [analysis](https://9to5google.com/2019/10/09/pixel-4-motion-sense-app-game-developers/) of the Pokemon Wave Hello game that comes bundled with the Pixel 4 phones, and discovered a Unity plug-in in the game that connected to a “Motion Sense Bridge” application running on the phone that gave the game developers access to various gesture parameters:

Flick

- flickConfidence
- flickDirection
- flickPrediction
- flickRange
- flickVelocity

Presence

- presenceConfidence
- presencePrediction
- presenceRange
- presenceVelocity

Reach

- reachAzimuth
- reachConfidence
- reachElevation
- reachPrediction
- reachRange
- reachVelocity

Swipe

- swipeAmplitude
- swipeConfidence
- swipeDirection
- swipeIntensity
- swipePrediction
- swipeTheta



Right now, third-party developers don’t have access to the MotionSense gestures unless they’ve been given access to the Android internal MotionSense Bridge app by Google. Hopefully, Google will open up full-access to the Soli sensor so developers are able to explore how they can use the gesture recognition capabilities it affords in new and innovative ways.

![Pixel4_Soli_Chip](/blog_assets/2020/Pixel4_Soli_Chip.jpg)

(The Pixel 4’s Soli sensor; from iFixit’s Pixel 4 XL Teardown https://www.ifixit.com/Teardown/Google+Pixel+4+XL+Teardown/127320)



![Pixel4_Sensors](/blog_assets/2020/Pixel4_Sensors.jpg)

The location of the Soli sensor on the Pixel 4 (from [https://ai.googleblog.com/2020/03/Soli-radar-based-perception-and.html](https://ai.googleblog.com/2020/03/soli-radar-based-perception-and.html))



**Challenges of Creating a Training a Dataset for Radar-based Gesture Recognition**

In a [post on the Google AI Blog](https://ai.googleblog.com/2020/03/soli-radar-based-perception-and.html), Google ATAP engineers describe some of the challenges and considerations for embedding radar into a smartphone, such as making the radar chip small and modular enough that it can fit at the top of a phone, adding filters to account for vibrational noise that occurs when music is playing from the phone, and machine learning algorithms that are able to run at a low power level.

One of the challenges with creating any robust machine learning model, especially one that will be in a device in the hands of millions of consumers, is making sure the model is able to accurately predict a gesture across a wide and diverse population of users. At the semantic level, it’s easy for humans to differentiate what a swipe or a flick gesture is. However, since each person makes those gestures in slightly different ways through variations in speed, hand angle, length of the gesture; the machine learning model for inferring which gesture occurs needs to be robust enough to be able to correctly infer the user’s gesture regardless of these differences.

To make sure their models were accurate, the [**Soli team trained their TensorFlow model on millions of gestures made by thousands of volunteers**](https://venturebeat.com/2020/03/12/google-soli-ai-model-millions-of-gestures-thousands-volunteers/)**.** These models were then optimized to run directly on the Pixel 4’s DSP unit; enabling the phone to recognize gestures even when the main processor is powered down — which is how the Pixel 4 is able to detect is someone is moving towards the phone using MotionSense, and then power on the FaceUnlock sensors to unlock the phone.

### PARTNERSHIP WITH INFINEON

While Google developed the machine learning algorithms, signal processing, and UX patterns for interacting with Soli, German company [Infineon developed the radar chip that is part of the Project Soli system](https://www.eetimes.com/infineon-flies-under-the-radar-on-googles-soli/). While it is possible to purchase development kits from Infineon, they only stream raw radar data — no processed signal features that could be used to train a machine learning model to recognize gestures or presence.

In their SSIGRAPH paper, [Soli: Ubiquitous Gesture Sensing with Millimeter Wave Radar](http://www.ivanpoupyrev.com/wp-content/uploads/2017/01/siggraph_final.pdf), the ATAP authors describe a HAL (Hardware Abstraction Layer) as a set of abstractions that would allow Project Soli to work across different radar sensor architectures from different manufacturers. This would allow Google to have the flexibility to use the same set of Soli feature primitives across various types of radar while keeping the same high-level interaction patterns.

### EXAMPLE APPLICATIONS FROM THE ALPHA DEV PROGRAM

Participants of the Soli Alpha Dev Program were encouraged to publish our work in academic publications; some members also created demos for showcase on various blogs, including:

- [New musical interfaces](http://homes.create.aau.dk/dano/nime17/papers/0054/index.html) (Demo [video](https://www.youtube.com/watch?v=WGlVzIlJvno))
- [An in-air gestural keyboard](https://bltt.org/project-soli-produces-first-gesture-protoype/)
- [The world’s tiniest violin](https://www.theverge.com/circuitbreaker/2016/6/10/11904036/project-soli-tiniest-violin-design-io)
- [Using Soli to identify objects for robotic arm control](https://www.youtube.com/watch?v=HMjGsInF2yM)

The HCI department at the University of St. Andrews produced a robust body of work as members of the Alpha Dev program, including

- [Radar Categorization for Input Recognition](https://www.researchgate.net/publication/317044324_RadarCat_Radar_Categorization_for_Input_Interaction) — the authors present RadarCat, a system that’s able to discriminate between “26 materials (including complex composite objects), next with 16 transparent materials (with different thickness and varying dyes) and finally 10 body parts from 6 participants”
- [Tangible UI by Object and Material Classification with Radar](https://www.researchgate.net/publication/321230369_Tangible_UI_by_object_and_material_classification_with_radar) — continuing their work from RadarCat; the authors also describe real-world application scenarios that this system could be used in, including self-checkout systems and smart medical devices.
- [Exploring Tangible Interactions with Radar Sensing](https://www.researchgate.net/publication/329954067_Exploring_Tangible_Interactions_with_Radar_Sensing) — exploring “radar as a platform for sensing tangible interaction with the counting, ordering, identification of objects and tracking the orientation, movement and distance of these objects”.

Some of the projects from the Alpha Developer program were showcased in a video that was presented in the update from ATAP at the following year’s I/O event (2016):



[![](http://img.youtube.com/vi/H41A_IWZwZI/0.jpg)](http://www.youtube.com/watch?v=H41A_IWZwZI "")



### GOOGLE PAPERS

Members of Google ATAP also published papers on their work with Project Soli:

- [Soli: Ubiquitous Gesture Sensing with Millimeter Wave Radar](http://www.ivanpoupyrev.com/wp-content/uploads/2017/01/p851-wang.pdf) — SIGGRAPH 2016
- [A Highly Integrated 60 GHz 6-Channel Transceiver With Antenna in Package for Smart Sensing and Short-Range Communications](https://drive.google.com/file/d/0B5SjnRdVtEfycXc5dXJLSmF5U1k/view) — IEEE 2016
- [Interacting with Soli: Exploring Fine-Grained Dynamic Gesture Recognition in the Radio-Frequency Spectrum](http://www.ivanpoupyrev.com/wp-content/uploads/2017/01/p851-wang.pdf) — UIST 2016
- [A Two-Tone Radar Sensor for Concurrent Detection of Absolute Distance and Relative Movement for Gesture Sensing](https://www.semanticscholar.org/paper/A-Two-Tone-Radar-Sensor-for-Concurrent-Detection-of-Gu-Lien/6f5f071341e6bd622a8b63e65694de42968ad98f) — IEEE Sensors Letters 2017



# Speculation

###  PROPERTIES OF MM-WAVE RADAR AND IT’S AFFORDANCES

Radar sensing is based on detecting the changing patterns of motion of an object in space. Radio waves are transmitted from the radar, bounce of a target (a human hand in motion), and then re-received by the radar’s antennas. The timed difference between when the waves are sent and when they are received is used to create a profile of the object that’s in the radar’s path.

In the case of human gestures, the hand moves its position through 3D space while in the line of sight of a radar sensor. The changes of position produce different profiles for the bounced off radar signals, allowing for different gestures to be detected.

Since radar detects gestures based upon different motion characteristics, it’s not well suited for detecting static gestures, such as sign language, or a peace sign. However, it is well suited for detecting dynamic, motion based gestures, like a finger snap, or a key-turning motion.

Unlike optical based sensors, radar’s performance isn’t dependent on lighting, can work through materials, and even detect gestures that occur when fingers may be occluding each other.

Micro-gestures can be [defined](https://dl.acm.org/doi/10.1145/2984511.2984565) as “interactions involving small amounts of motion and those that are performed primarily by muscles driving the fingers and articulating the wrist, rather than those involving larger muscle groups to avoid fatigue over time”. Some examples of these types of gestures are making the motion of pushing a button by tapping your forefinger against your thumb, making a slider motion by moving your thumb against the surface of your forefinger, and making a motion similar to turning a dial with your fingers and wrist.

These gestures could be used in a variety of contexts (IoT, AR/VR, etc) for interacting with user interface elements.

### GOOGLE’S AMBIENT COMPUTING FUTURE

In the [first published paper for Project Soli](https://www.cs.virginia.edu/~bjc8c/class/cs6501-f17/lien16soli.pdf), the authors (from Google ATAP) list several possible application areas:

- Virtual Reality
- Wearables and smart garments
- Internet of Things and game controllers
- “Traditional Devices” (mobile phones, tablets, laptops)

If all of these device types were to integrate Project Soli, Google could leverage a universal gestural framework that all of them would have in common. This would make it easy for people to quickly use these new devices, all interacting with Google’s array of services.

Ben Thompson’s article on Stratechery, “[Google and Ambient Computing](https://stratechery.com/2019/google-and-ambient-computing/)”, analyzes Google’s recent shift from stating they want to help organize the world’s information, to one that helps you get things done.

In his opening remarks at [Made by Google 2019](http://v/), Google Senior VP of Devices and Services, [Rick Osterloh](https://www.wired.com/story/one-mans-quest-to-make-googles-gadgets-great/) (who was formerly the head of Google ATAP), outlines a vision of Google as a company that wants “to bring a more helpful Google to you.” Sundar Pichai stated in the keynote of 20193 I/O that “We are moving from a company that helps you find answers to a company that helps you get things done”.

Ambient Computing was first coined by tech journalist Walt Mossberg in his final column, “[The Disappearing Computer](https://www.theverge.com/2017/5/25/15686870/walt-mossberg-final-column-the-disappearing-computer)”. It’ also referred to as ubiquitous or pervasive computing.

For some additional reading on this area of computing, check out the [work of Mark Weiser](http://scihi.org/mark-weiser-and-his-vision-of-ubiquituous-computing/), a Chief Scientist at Xerox PARC, especially his 1991 Scientific American article, “[The Computer for the 21st Century](https://www.lri.fr/~mbl/Stanford/CS477/papers/Weiser-SciAm.pdf)”. Weiser coined the term ubiquitous computing, which he described as computing able to occur using “any device, in any location, and in any format”.

Thompson makes the point that Google’s vision of ambient computing “does not compete with the smartphone, but rather leverages it”. Google isn’t trying to find whatever the next hardware platform is (like Facebook was doing with acquiring Oculus for VR, or Apple’s full-on push into AR); ratther, they are looking to create an ecosystem of ambient devices that all seamlessly connect (possibly using the smartphone as a hub?) and are intuitive to interact with; all connected to the services that Google’s providing.

Having a unified way to [interact with devices that exist in a variety of contexts](https://staceyoniot.com/ambient-computings-next-big-problem-is-intent/) would be hugely beneficial to Google in furthering adoption of their ambient computing vision. A small, easily embeddable sensor that can detect gestures from people regardless of lighting or other atmospheric conditions would bring this vision much closer to reality. This would make it easier for users to engage with a wide variety of devices that would offer access to Google’s services.

### CLUES FROM APPLE ON ADOPTION OF MM-WAVE RADAR

With the recent release of a [LiDAR capable iPad Pro](https://www.apple.com/newsroom/2020/03/apple-unveils-new-ipad-pro-with-lidar-scanner-and-trackpad-support-in-ipados/) in service to AR capabilities, Apple seems to be showing a willingness to put sensors of ever increasing complexity (and utility) into their products. Additionally, Apple has put up at least one posting for roles related to radar; a [now inactive posting on LinkedIn](https://www.linkedin.com/jobs/view/radar-signal-processing-engineer-at-apple-1380851968/) for a Radar Signal Processing Engineer includes the following in its description.



![AppleJob1](/blog_assets/2020/AppleJob1.jpg)

​	

![AppleJob2](/blog_assets/2020/AppleJob2.jpg)



It feels fair to me to say that at the very least, Apple is looking at millimeter-wave radar as a sensing modality; when, how, and most importantly; if a radar-enabled Apple product ever leaves the labs in Cupertino is one that only time will be able to tell.

My personal speculation is that Apple will release an AR headset with radar built-in for micro-gesture detection to augment their hand tracking capabilities. Additionally, as radar becomes better well known as a possible sensing modality (thanks mostly due to Project Soli, and whatever products Google and their partners decide to integrate it into), other AR and VR headset makers will begin integrating millimeter-wave radar chips into their headsets as a way to solve the “missing interface” problem mentioned earlier; making sure that the real-world physical objects that people interact with via AR/VR have a way to map to digital information that’s presented via the headset.

## COMPETITION

There is [at least one startup](https://www.eetimes.com/document.asp?doc_id=1335123) working on millimeter-wave radar for human-machine interfaces; Taiwan’s KaiKuTek (“CoolTech”). They claim that their radar-based gesture sensing system can match, if not surpass, Google’s Project Soli.

A Machine-Learning inference chip is integrated with the radar sensor; so all the inference is done on a *sensor-side compute* level, unlike the Pixel 4’s MotionSense system, in which the sensor (Soli) and the inference engine are on separate chip components. This is, KaiKuTek claims, they are able to achieve such a low power (1 mW) rating.

## CLOSING THOUGHTS

With Project Soli, Google has advanced the conversation on how we interact with computers across a wide range of modalities and contexts. Millimeter-wave radar offers a promising way to gesturally interact with computers without having to worry about occlusion, lighting conditions, or similar limiting conditions imposed on camera-based systems.

With the increasing pace of computers being embedded in more devices, millimeter-wave radar could end up enabling a more universal gestural language that’s familiar across these devices. Of course, each manufacturer will inevitably have differences between each other (though Google is the first to use mm-wave radar as a sensor for gestural interaction, it does not mean it will be the last), it could end up affording “similar enough” gestural interactions the same way that touchscreens are almost universal, but each OEM vendor enables different gestures for use with the touch screen.

## APPENDIX:

I’ve included additional publications dealing with millimeter-wave radar and its applications in HCI (not necessarily involving Project Soli). A good portion of these focus on the machine learning techniques used to enable gesture recognition with a radar pipeline.

- [One-Shot Learning for Robust Material Classification Using Millimeter-Wave Radar System](https://www.researchgate.net/publication/328523480_One-Shot_Learning_for_Robust_Material_Classification_Using_Millimeter-Wave_Radar_System)

- [Hand Gesture Recognition Using a Radar Echo I-Q Plot and Convolutional Neural Network](https://www.researchgate.net/publication/327147453_Hand_Gesture_Recognition_Using_a_Radar_Echo_I-Q_Plot_and_Convolutional_Neural_Network)

- [Robust Gesture Recognition using Millimetric-Wave Radar System](https://www.researchgate.net/publication/329111113_Robust_Gesture_Recognition_using_Millimetric-Wave_Radar_System)

- [Hand Gesture Recognition based on Radar Micro-Doppler Signature Envelopes](http://researchgate.net/publication/329362210_Hand_Gesture_Recognition_based_on_Radar_Micro-Doppler_Signature_Envelopes)

- [Reinventing radar: The power of 4D sensing](https://www.researchgate.net/publication/331479894_Reinventing_radar_The_power_of_4D_sensing)

- [Gesture Recognition Using mm-Wave Sensor for Human-Car Interface](http://researchgate.net/publication/323438029_Gesture_Recognition_Using_mm-Wave_Sensor_for_Human-Car_Interface)

- [Short-Range Radar-based Gesture Recognition System using 3D CNN with Triplet Loss](https://www.researchgate.net/publication/335508422_Short-Range_Radar-based_Gesture_Recognition_System_using_3D_CNN_with_Triplet_Loss)

- [Hand Gesture Recognition based on Radar Micro-Doppler Signature Envelopes](https://www.researchgate.net/publication/329362210_Hand_Gesture_Recognition_based_on_Radar_Micro-Doppler_Signature_Envelopes)

- [Robust Gesture Recognition using Millimetric-Wave Radar System](https://www.researchgate.net/publication/329111113_Robust_Gesture_Recognition_using_Millimetric-Wave_Radar_System)

- [TS-I3D based Hand Gesture Recognition Method with Radar Sensor](https://www.researchgate.net/publication/330863396_TS-I3D_based_Hand_Gesture_Recognition_Method_with_Radar_Sensor)

- [Character Recognition in Air-Writing Based on Network of Radars For Human-Machine Interface](https://www.researchgate.net/publication/333733782_Character_Recognition_in_Air-Writing_Based_on_Network_of_Radars_For_Human-Machine_Interface)

- [Doppler-Radar Based Hand Gesture Recognition System Using Convolutional Neural Networks](https://www.researchgate.net/publication/325628921_Doppler-Radar_Based_Hand_Gesture_Recognition_System_Using_Convolutional_Neural_Networks)

- [Eliciting Contact-based and Contactless Gestures with Radar-based Sensors](https://www.researchgate.net/publication/337026376_Eliciting_Contact-based_and_Contactless_Gestures_with_Radar-based_Sensors)

- [Reinventing radar: The power of 4D sensing](https://www.researchgate.net/publication/331479894_Reinventing_radar_The_power_of_4D_sensing)

- [Motion Sensing Using Radar: Gesture Interaction and Beyond](https://ieeexplore.ieee.org/document/8755821)



Patents related to Project Soli:

- [Gesture-Based Small Device Input](https://patentscope.wipo.int/search/en/detail.jsf?docId=US233946088)
- [Radar-based gesture-recognition through a wearable device](https://patents.google.com/patent/US9575560B2/en)
- [Radar Recognition-Aided Searches](https://patents.google.com/patent/US20160055201A1/en?inventor=Gaetano+Roberto+Aiello)
- [Radar-based authentication](https://patents.google.com/patent/US10503883B1/en?inventor=Ivan+Poupyrev&clustered=true)
- [Radar-Enabled Sensor Fusion](https://patents.google.com/patent/US20170097413A1/en)
- [Wide-field radar-based gesture recognition](https://patents.google.com/patent/US10139916B2/en?inventor=Ivan+Poupyrev&clustered=true)
- [Occluded gesture recognition](https://patents.google.com/patent/US10409385B2/en?inventor=Ivan+Poupyrev&clustered=true)
- [Radar-based gesture sensing and data transmission](https://patents.google.com/patent/EP3177982B1/en?inventor=Ivan+Poupyrev&clustered=true)
- [Smartphone-based Radar System Facilitating Ease and Accuracy of User Interactions With Displayed Objects in an Augmented Reality Interface](https://pdfaiw.uspto.gov/.aiw?Docid=20200064996)

Press Articles on the Pixel 4 Launch and Integration with Project Soli:

- [Rumor: Google’s Project Soli radar chip could debut in Google Pixel 4](https://9to5google.com/2019/06/11/google-pixel-4-project-soli/)
- [GOOGLE’S PROJECT Soli: THE TECH BEHIND PIXEL 4’S MOTION SENSE RADAR](https://www.theverge.com/2019/10/15/20908083/google-pixel-4-project-soli-radar-motion-sense-explainer)
- [Project Soli is the secret star of Google’s Pixel 4 self-leak](https://www.cnet.com/news/project-soli-is-the-secret-star-of-googles-pixel-4-self-leak/)
- [Pixel 4 XL hands-on details ‘Face unlock,’ back finish, more](https://9to5google.com/2019/09/23/pixel-4-xl-hands-on-face-unlock-back-finish/)
- [Gesture control thanks to Infineon-radar technology in Google Pixel 4 Smartphone](https://www.infineon.com/cms/en/about-infineon/press/press-releases/2019/INFPMM201910-003.html)
- [Monument Valley’s ustwo creates Motion Sense game ‘Headed South’ for Google Pixel 4](https://9to5google.com/2019/10/15/ustwo-motion-sense-game-headed-south-pixel-4/)
- [With Pixel 4, Google’s experimental tech bets finally enter the spotlight](https://www.cnet.com/news/with-pixel-4-googles-experimental-tech-bets-finally-enter-the-spotlight/)
- [Google: Soli Dance DJ by Swift](https://www.thedrum.com/creative-works/project/swift-google-soli-dance-dj)
