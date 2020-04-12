---
title: "Touché, and Water as an Interface"
date: 2017-08-15
draft: true
---

After experimenting with learning how [Touché could be used to interact with plants](https://www.nickarner.com/blog/2017/7/8/talking-to-plants-touché-experiments), I wanted to see how I could use it to interact with water.

In the [Touché paper](https://s3-us-west-1.amazonaws.com/disneyresearch/wp-content/uploads/20140805145650/touchechi20121.pdf), the authors demonstrate that the sensor is capable of detecting how a user is touching the surface of, or submerging their hand in water. The system is able to distinguish among a variety of touch interactions: no hand, 1 finger, 3 fingers, and hand submerged:

​																				[![](http://img.youtube.com/vi/E4tYpXVTjxA/0.jpg)](http://www.youtube.com/watch?v=E4tYpXVTjxA "")



I decided to see if I could replicate the same results using the [ESP-system](https://github.com/damellis/ESP).

This experiment was not as successful as the previous one using plants as an interface (I wasn't able to exactly replicate all of the types of interactions that the author's of the Touché paper did), but, what I learned may provide some clues with how to go about improving a Touché based - water interface in future, longer term projects. Additionally, I was able to train the system to distinguish between two types of touches:

First, the default state of not touching the water:

![no+hands](/blog_assets/2017/no+hands.gif)

​	

Touching the water with one finger-tip:

![Touche+One+Finger](/blog_assets/2017/Touche+One+Finger.gif)

​	

Touching the water with all five finger-tips :

![Touche+Five+Fingers](/blog_assets/2017/Touche+Five+Fingers.gif)



SETUP CONFIGURATIONS

In this still from the Touché paper video, you can see what looks like a metal plate on the bottom of the water tank, which is connected to the circuit via a wire. The narration in the video stated that the electrode was placed *under* the container - I attempted this, but my sensor readings did not appear as strong as the electrodes I used (shown below) had *some* direct contact with the water.

![Touche+Video+Electrode](/blog_assets/2017/Touche+Video+Electrode.jpg)

I ended up trying a variety of different configurations for working with the sensor.

My first thought was to try using aluminum foil, since it was cheap and easy to work with:

![Aluminum+Conductor](/blog_assets/2017/Aluminum+Conductor.jpg)

I went to Lowe's, and got a sheet of steel, and crudely hacked off a small square that would fit in my water container. I first tried having the plate submerged, but this seemed to make the system less responsive than if the plate was put upright against a corner of the Tupperware container, as shown below:

![Upright+plate](/blog_assets/2017/Upright+plate.jpg)

This seemed to work *OK*, but I wanted to experiment further with electrodes and water containers. I tried wrapping the steel plate in solder, and then connecting the alligator clip to one of the ends of the solder:



![Aluminum+Solder](/blog_assets/2017/Aluminum+Solder.jpg)



That configuration did not prove to be very effective.

Finally, I took a glass cooking dish, and placed the steel-electrode horizontally as shown below:



![AluminumPlate+Dish](/blog_assets/2017/AluminumPlate+Dish.jpg)

Here's a short clip of the system in use:

[![](http://img.youtube.com/vi/6-5DdljQmEk/0.jpg)](http://www.youtube.com/watch?v=6-5DdljQmEk "")



## CODE

The Processing sketch + ESP model are on GitHub [here](https://www.nickarner.com/blog/2017/7/10/touch-and-water-as-an-interface#). If you're trying to replicate the experiments shown here, you'll most likely need to train your own model; as the accuracy will vary based on individual setups.

## FURTHER GOALS

Though I wasn't able to get the results I necessarily wanted with this experiment, I believe there is a lot of room for further research and exploration that may shed some insight into a deeper understanding of how the Touché system works, and how it can be applied to create a water-based interface.

**Test out a greater variety of containers and electrodes** - I have a feeling that this is probably the key to building a better water-based interface. It will be important to keep in mind that the results of this study will determine how the interface is used in-context; whether it's an installation, a game, or a VR scenario.

**Better understand of Support-Vector Machines** - This would be useful so that I can understand why the Touché developers chose this particular algorithm. Currently, I'd say I have a fairly shallow knowledge of the algorithms used in interactive machine learning contexts...often times, the parameters used in these situations still feel a bit like magic numbers. I think that diving a bit deeper into the nature of the algorithms will be useful in creating more accurate gesture-recognition systems.

**Experiment with pre-and-post processing modules for the machine learning pipeline** - The GRT has several modules for processing incoming data before and after it's sent through a classification or regression module. Some of these modules may be useful in filtering sensor the data from the sensor, potentially providing for more accurate gesture classification.

I still consider this prototyping experiment a success, as I was able to use everyday objects (a Tupperware container, a sheet of steel, an off-the-shelf electronics) to build a system that could distinguish whether I was touching the surface of water with one finger, five fingers, or not touching the water at all.

## IMPLICATIONS

Given its multi-sensory characteristics and emotional resonance, water can be an interesting choice for an interface in a game, installation, or in an AR/VR context.

Pier and Goldberg, in the their paper "[Using water as interface media in VR applications](https://www.researchgate.net/publication/232725076_Using_water_as_interface_media_in_VR_applications)", make a compelling case for why using water may be an interesting and engaging digital interface. They state

"In our experience, touching real water has proven to be rather dramatic as a tactile interface since it involves much more than the mere skin contact with the water, the effect is magnified by the temperature and wetness which affect the perception of the user."

The aspects of the water that the authors measured were pressure, flux, and movement. It could be interesting to combine these with a Touché sensor in a sensor-fusion setup (whereby several different types of sensors are combined into one data stream that's fed to a machine-learning classifier, which would then detect meaningful data based on the combination of inputs).

In "Capturing Water and Sound Waves to Interact with Virtual Nature", Díaz et al. propose interacting with water (and wind, in their specific experiments) as a way to "....give the user the sensation that his/her presence affects the virtual world and then to let the user perceive that the actions he/she takes on the real world can change the virtual one in a smooth nature way in order to achieve virtual biofeedback"...in other words, using a water-based interface as a bridge between the physical and virtual worlds.

There remains a lot of work left in experimenting with water-based interfaces - sensors like Touché have the ability to allow for more interactions that help make use of our everyday environment.
