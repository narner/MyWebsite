---
title: "Tools for Machine Learning and Sensor Inputs for Gesture Recognition"
date: "2017-06-06"
---

The past several years have seen an explosion in machine learning, including in creative contexts - everything from[ hallucinating puppyslugs](https://research.googleblog.com/2015/06/inceptionism-going-deeper-into-neural.html), to[ generating new melodies](https://magenta.tensorflow.org/welcome-to-magenta), machine learning is already beginning to revolutionize the way artists and musicians execute their craft.

&nbsp;

My personal interest in the area of machine learning relates to using it to recognize human gestural input via sensors. This interest was sparked from [working with Project Soli as a member of the Alpha Developer program](https://www.nickarner.com/project-soli-alpha-developers-program).

&nbsp;

Sensors offer a bridge between the physical world and the digital. Rich sensor input combined with machine learning allows for new interfaces to be developed that are novel, expressive, and can be configured to a specialized creative task.

&nbsp;

In order to make sense of the potential that sensors offer technologists and artists, it's often necessary to utilize machine learning to build a system that can take advantage of a sensor's capabilities. Sensors by themselves aren't always easy to make sense of right away.

&nbsp;

With something like an infrared sensor, you can get a[ simple interactive system](https://github.com/narner/ProximitySensing-With-Chirpino) working with a sensor and some if-then-else-that type of logic. Something like LIDAR, or a [depth-field camera](https://software.intel.com/en-us/realsense/home), on the other hand, will have a much larger data footprint - the only way to make sense of the sensor's data is to use machine learning to recognize what patterns are in the real-time data that the sensor is gathering. It’s important to be aware of what type of data a sensor is capable of providing, and whether or not it will be appropriate for the interactive system you are trying to build.

&nbsp;

Often, the more complex the data is that the sensor provides, the more interesting things you can do with it.

&nbsp;

I wanted to go over some of the open-source tools that are currently available to the creative technology community to leverage the power of sensors for adding gestural interactivity to creative coding projects:

&nbsp;

- Wekinator
- Gesture Recognition Toolkit / ofxGrt
- ESP

&nbsp;

&nbsp;

## WEKINATOR

&nbsp;

The[ Wekinator](http://www.wekinator.org/) is a machine-learning middleware program developed by[ Dr. Rebecca Fiebrink](http://www.doc.gold.ac.uk/~mas01rf/Rebecca_Fiebrink_Goldsmiths/welcome.html) of Goldsmith's University in London. The basic idea of its use is that it receives data from a sensor via[ OSC (open-sound control)](http://opensoundcontrol.org/) from a program that’s acquiring the data from the sensor, such as an Arduino or a Processing sketch. Wekinator is used to train a machine learning system on this incoming data to recognize which gesture has occurred, or to map the start and end times of a gesture to a value that can be used to control a parameter range. These values are sent out from Wekinator via OSC, and can then be received by a program that maps those values to to control audio/visual elements.

&nbsp;

[Lots of examples](http://www.wekinator.org/examples/) are provided showing how to use a plethora of sensors with the Wekinator, such as a Wii-Mote, Kinect, Leap Motion, etc, and map them to parameters various receiver programs.

&nbsp;

What's great about the Wekinator is that you don't have to know much about how machine learning works in order to use it - its strength is in the fact that it's user friendly and easy to experiment with quickly.

&nbsp;

If you're interested in exploring how you can use Wekinator to add interactivity to your projects, I highly recommend Dr. Fiebrink's[ Machine Learning for Artists and Musicians](https://www.kadenze.com/courses/machine-learning-for-musicians-and-artists/info) course on Kadenze.

&nbsp;&nbsp;

&nbsp;

## GESTURE RECOGNITION TOOLKIT

&nbsp;

The[ GRT](https://github.com/nickgillian/grt) is a cross-platform toolkit for interfacing with sensors for gesture-recognition systems. It's developed and maintained by[ Nick Gillian](http://nickgillian.com/), who is currently a lead machine learning researcher for Google's[ Project Soli](https://atap.google.com/soli/).

&nbsp;

The GRT can be used as a[ GUI-based application](http://www.nickgillian.com/wiki/pmwiki.php/GRT/GUI) that acts as middleware between your sensor input via OSC, and a receiver program that reacts to the gesture events detected by your sensor. It’s useage follows the same pattern as the Wekinator.

&nbsp;

However, the real benefit of the GRT is how you can write your gesture-recognition pipeline, train your model, and use your code on multiple platforms. This is useful if you want to prototype a gesture-recognition system on your desktop that may need to be deployed on custom, embedded hardware. Additionally, you can write a custom module for the GRT in order to customize your pipeline based on some unique characteristics of the sensor you're using.

&nbsp;

Be sure to read Nick’s [paper](http://jmlr.org/papers/volume15/gillian14a/gillian14a.pdf) on the toolkit in the Journal of Machine Learning Research.

&nbsp;

### OFXGRT

&nbsp;&nbsp;

The GRT also comes embedded in a wrapper for use in Open Frameworks, a C++ toolkit for creative coding,[ ofxGrt](https://github.com/nickgillian/ofxGrt). This type of wrapper is known as an addon. This makes it extremely easy to integrate into new or existing Open Frameworks projects. Combined with the numerous other Open Frameworks[ addons](http://ofxaddons.com/categories), ofxGrt allows coders to integrate with various types of sensors for adding an interface to physical world with their creative projects.

&nbsp;

&nbsp;

## EXAMPLE-BASED SENSOR PREDICTIONS

&nbsp;

The[ Example-based Sensor Predictions](https://github.com/damellis/ESP) project by[ David Mellis](http://alumni.media.mit.edu/~mellis/) and[ Ben Zhang](https://www.benzhang.name/) was created to make sensor-based machine-learning systems accessible to the Maker and Arduino community. Though the maker community was very familiar with how to work with sensors, fully utilizing their potential for rich interactions really wasn't possible to do in the Arduino ecosystem until this project was developed.

&nbsp;

Here’s a short video overview of the project from the creators:

&nbsp;

[![](http://img.youtube.com/vi/5nDCG4vkFP0/0.jpg)](http://www.youtube.com/watch?v=5nDCG4vkFP0 "")

&nbsp;

The project is built so that users can interface with sensors via Processing, and the gesture recognition pipeline is built using the GRT. It contains four examples:

&nbsp;

- Audio Detection

- Color Detection

- Gesture Recognition

- Touché touch detection

  &nbsp;

Developers are also given API documentation for how to write their own sensor-based gesture recognition examples as part of the ESP environment.

&nbsp;

I've recently been doing some experiments with the ESP project, and hope to share some of those examples soon.

&nbsp;

There’s still a lot of work to be done exploring how new sensing technologies can be leveraged to improve the way that people interact with the devices around them. In order to understand how these technologies may be useful, artists and musicians need to be at the forefront of using them...pushing sensors and machine learning systems to the limits of how they can be used in expressive contexts. When artists push interfaces to their full potential, everyone benefits as a result - as Bill Buxton said in Artists and the Art of the Luthier, “I also discovered that in the grand scheme of things, there are three levels of design: standard spec., military spec., and artist spec. Most significantly, I learned that the third was the hardest (and most important), but if you could nail it, then everything else was easy.”

&nbsp;

The tools described above should encourage artists, musicians, designers, and technologists to explore using sensor-based gesture recognition systems in their creative practice, and imagine ways in which interactivity can be added to new products, pieces, designs, and compositions.
