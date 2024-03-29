---
title: "Talking to Plants: Touché Experiments"
date: "2017-08-04"
---

As I mentioned in a [previous post]({{< ref "Tools for Machine Learning and Sensor Inputs for Gesture Recognition - July 6, 2017.md" >}}), I was really pleased to see that the [ESP-Sensors project](https://github.com/damellis/ESP) had included code for [working with a circuit based on Touché](https://github.com/damellis/ESP/wiki/[Example]-Touché-swept-frequency-capacitive-sensing).

I had earlier come across other implementations of Touché for the Arduino, but unlike the ESP project, none of them utilized machine learning for classifying gesture types.

Touché is a [project developed at Disney Research](https://www.disneyresearch.com/project/touche-touch-and-gesture-sensing-for-the-real-world/) that uses swept-frequency capacitive sensing to "...not only detect a touch event, but simultaneously recognize complex configurations of the human hands and body during touch interaction."

In other words, it's able to not just tell *if* a touch event occurred, but what *type* of touch event occurred. This differs from most capacitive sensors, which are only able to detect whether a touch event occurred, and possibly how far away from a sensor the user's hand is.

Traditional capacitive sensing works by generating an electrical signal at a single frequency. This signal is applied to a conductive surface, such as a metal plate. When a hand is either close to or touching the surface, the value of capacitance changes - signifying that a touch event has occurred, or that a hand is close to the surface of the sensor. The [CapSense library](https://playground.arduino.cc/Main/CapacitiveSensor?from=Main.CapSense) allows for traditional capacitive sensing to be implemented on an Arduino.

Swept-frequency capacitive sensing makes use of *multiple* frequencies. In their [C](https://s3-us-west-1.amazonaws.com/disneyresearch/wp-content/uploads/20140805145650/touchechi20121.pdf)[HI 2012 paper](https://s3-us-west-1.amazonaws.com/disneyresearch/wp-content/uploads/20140805145650/touchechi20121.pdf), the Touché developers state the reason for using multiple frequencies is "Objects excited by an electrical signal respond differently at different frequencies, therefore, the changes in the return signal will also be frequency dependent." Rather than using a single data point generated by an electrical signal at a single frequency, as in traditional capacitive sensing, Touché utilizes multiple data points from multiple generated frequencies.

This capacitive profile is used to train a machine learning pipeline to differentiate between various touch interactions. This machine learning pipeline is based around a Support Vector Machine. Specifically, it uses the [SVM module from the Gesture Recognition Toolkit](http://www.nickgillian.com/wiki/pmwiki.php?n=GRT.SVM).

Check out the video that accompanied the Touché CHI 2012 paper:

[![](http://img.youtube.com/vi/E4tYpXVTjxA/0.jpg)](http://www.youtube.com/watch?v=E4tYpXVTjxA "")



There have been a few other open-source implementation of touch-sensing based off of Touché that I've come across before, but the one provided in the ESP project seemed to be the easiest to set-up, and the most usable to work with.

The authors continued their work by showing how Touché could be used to interact with plants in the [Botanicus Interacticus project](https://www.disneyresearch.com/project/botanicus-interacticus-interactive-plant-technology/), which was displayed in an exhibition at SIGGRAPH 2012:



[![](http://img.youtube.com/vi/EcRSKEIucjk/0.jpg)](https://www.youtube.com/watch?v=EcRSKEIucjk "")



I have two plants on my desk: a fern plant, and an air plant. I really enjoy the way they add some color to my work area, and am grateful for their presence.

I wanted to see if they could talk to me.



![Touche+FernPlantSetup](/blog_assets/2017/Touche+FernPlantSetup.jpg)

![Touche+AirPlant-Setup](/blog_assets/2017/Touche+AirPlant-Setup.jpg)

## The Fern

I first experimented with the fern. As suggested in the [Botanicus Interacticus paper](https://pdfs.semanticscholar.org/bad6/92a87a416a228542f5ed554503b604ad481e.pdf), I inserted a simple wire lead into the soil of the plant. This would allow the ESP system to measure the conductive profile of the plant as I touch it.

After doing some experiments, I was able to easily train the ESP system to recognize whether I was touching a single leaf:

![fern](/blog_assets/2017/fern.gif)



or whether I was lightly caressing down on the top of the leafs with the palm of my hand.

![fern2](/blog_assets/2017/fern2.gif)



Here's what that looked like in action:

[![](http://img.youtube.com/vi/ZPsU6U54CRM/0.jpg)](https://www.youtube.com/watch?v=ZPsU6U54CRM "")



I also tried experimenting as to whether or not the system was able to detect whether or not I was touching individual leaves, but was not able to get consistent results. I discuss my theory on why this may be the case at the end of this post.



## The Air Plant

I experimented with the air plant next, and successfully trained the ESP system to be able to discriminate between whether I had my hand at rest on top of the plant:



![airplant+rest](/blog_assets/2017/airplant+rest.gif)



and whether or not was "tickling" the top of the plant



![ariplant+tickle](/blog_assets/2017/ariplant+tickle.gif)

Here's what the system looked like in use:



[![](http://img.youtube.com/vi/RJ--EB5DpOc/0.jpg)](https://www.youtube.com/watch?v=RJ--EB5DpOc "")





What I was not successful at was training the ESP to discriminate between the act of touching a leaf, and rubbing my finger on a leaf as shown below:



![leaf+rub](/blog_assets/2017/leaf+rub.gif)



I tried moving the alligator clip from one of the leafs to the root - my theory being that perhaps the capacitance wasn't being spread evenly throughout the plant.



![AirPlant+Electrode](/blog_assets/2017/AirPlant+Electrode.jpg)

![AirPlant+Electrode2](/blog_assets/2017/AirPlant+Electrode2.jpg)



This appeared to have no affect, however.

I was a bit surprised at this - given the subtlety in touch which it seemed Touché was capable of measuring, I had thought the system would be capable of discriminating between touching and rubbing a single leaf. That said, there could be some missing factor (such as amount of training data/sessions) that I'm not aware of yet in order to make that happen.



## Further Learning Goals:

### Using Regression

In the Bottanicus Interactus video (at the time below), the authors show that they are able to determine where at on a long plant stem is being touched, and interact with it in a way that resembles using a slider moving continuously between two points.



[![](http://img.youtube.com/vi/EcRSKEIucjk/0.jpg)](https://www.youtube.com/watch?v=EcRSKEIucjk "")



The Touché system uses a Support Vector Machine Learning algorithm, which is capable of both classification and regression; two types of machine learning tasks. In classification, a machine learning system detects what *type* of events have occurred - in this case, the *type of touch* that occurred. In regression tasks, a machine learning system maps the distance between two points to control a parameter - so, for instance, you could map the distance travelled by the hand between two points on a plant stem to the value of a volume slider.

in the ESP system, classification is currently supported; regression is not. In order to use Touché to control a continuous stream of value between one point and another, the ESP system would need to be modified to support regression.

(For more info on the principles behind classification and regression, check Rebecca Fiebrink's Kadenze course, [Machine Learning for Musicians and Artists](https://www.kadenze.com/courses/machine-learning-for-musicians-and-artists/info)).

### Finer Discrimination

Determine whether or not it is possible to detect the touches of individual leafs, as opposed to detecting whether "a leaf" has been touched. It may be that this is possible, but dependent on the type of plant involved - a plant with thicker, more "solid" leaves may return a conductive pattern that's better at discriminating between individual leaf touches than the thin, loose leaves of the fern plant.

### Gesture Timeouts

If you watch the videos of the Touché system in action above, you may have noticed that there were occasionally instances in which there is a short "bounce" during the transition between one gesture class and another. In the Air Plant example, when the hand is moving from a "Resting Hand" position to a "No Hand" position, the ESP system will falsely recognize a "Tickling" gesture.

A potential remedy for this situation is to add a [Class Label Filter](http://nickgillian.com/grt/api/0.1.0/_class_label_filter_8h.html) to the ESP's gesture recognition pipeline. This class will allow the system to get rid of the erroneously recognized gesture class. Adding this filter to the ESP pipeline is something I'm planning on exploring in future experiments.



## Code

The Processing sketches shown in the videos, and the ESP session data, can be found [here](https://github.com/narner/Touche-Experiments). You'll need to make sure you have [Processing](http://processing.org/) installed on your system to run the sketches, and have followed the installation guide for setting the [ESP](https://github.com/damellis/ESP) if you want to run the included sessions.
