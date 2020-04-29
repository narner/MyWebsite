---
title: "Real-time data receiving and rendering in Processing"
date: 2017-08-21
---

I wanted to talk a little bit about one of the technical challenges I had faced while writing the Processing receiver sketches for the Touché experiments in some previous blog posts ([here](https://www.nickarner.com/blog/2017/7/8/talking-to-plants-touché-experiments) and [here](https://www.nickarner.com/blog/2017/7/10/touch-and-water-as-an-interface)).

The problem I was experiencing was that the Processing sketches that would receive the gesture-classification data from the ESP program seemed to updating *incredibly* slowly. As in, I could clearly see the gesture-classification results being updated in the ESP program, but in the Processing receiver sketches, the results would be displayed several seconds behind what the ESP system was sending! Several seconds behind, actually.

Take the plant experiments, specifically, When the sketch started, the default state would be "No Hand". However...even after I began touching one of the plants, the receiver would still show a "No Hand" result, even though I could verify that the gesture was being classified correctly in the ESP program (for clarification here: the [ESP program](https://github.com/damellis/ESP) was what was performing the gesture classification; the receiver sketches were displaying the results).

Let's take a look quick at how the receiver sketches are set-up to receive data from the ESP program. The sketches use [Processing Networking library](https://processing.org/reference/libraries/net/index.html)  to set up a server for receiving data.

A server is created as a global parameter at the top of the sketch:


```
Server server;
```


And, in the setup method, is configured to listen for data on port 5204:


```
server = new Server(this, 5204); // listen on port 5204
```

In the standard draw() method, as long as a client connection exists, we'll extract the data. The value of a text label (the set-up code for which isn't shown here for brevity) is set depending on the value of the extracted data. In this case, we're checking if we receive a 1, a 2, or a 3 from the ESP program, and then update the text value accordingly.


```
void draw() {
  // check for incoming data
  Client client = server.available();
  if (client != null) {
    // check for a full line of incoming data
    String line = client.readStringUntil('\n');

    if (line != null) {
      println(line);
      int val = int(trim(line)); // extract the predicted class from the ESP sender
      println(val);
      if (val == 1) {
        messageText = "GESTURE 1";
      } else if (val == 2) {
        messageText = "GESTURE 2";
      } else if (val == 3) {
        messageText = "GESTURE 3";
      }
    }
  }
...
}
```


This was actually just a slightly modified version of an [example program that was included with the ESP repository,](https://github.com/damellis/ESP/blob/master/Processing/BallDrop/BallDrop.pde) so I was fairly certain that the problem I was experiencing wasn't a fault of that particular bit of code.

The ESP sends the gesture results out via a UDP stream - as gesture recognition is being performed, the program takes the predicted results, and sends them out over a network connection. I wondered whether or not it would be worth trying to modify the ESP program to send out the gesture results via an OSC stream, rather than a pure UPD network stream, since I was much more familiar with OSC configuration and usage.

While looking at the Processing [oscP5 library](http://www.sojamo.de/libraries/oscP5/), I realized something - there's a callback function that gets OSC data as it's coming into the program. Arguments from the stream can then be parsed out, and events can be triggered or parameters updated accordingly.

```
void oscEvent(final OscMessage msg) {
  receivedValue = msg.get(0).intValue();
  println("Received value was:", receivedValue);
}
```

This actually ended up giving me a flash of insight - the code I had for getting the UDP data was housed inside Processing's [draw()](https://processing.org/reference/draw_.html) method. This method is called repeatedly throughout the lifecycle of a Processing sketch, and is updated at whatever Processing's frame-rate is.

So, I finally had a hunch as to what the issue was - the Receiver sketch wasn't reading the network results fast enough because the frame-rate at which the draw() function was being called (where the network receiving code was) was not being called fast enough!

Processing's default frame-rate is 60 FPS. Bumping that up to 480 FPS made a huge improvement in performance - but not enough.

Processing also has different *types* of renderers. The default is JAVA2D, which, according to this [StackOverflow discussion](https://stackoverflow.com/questions/22808318/choosing-a-renderer-difference-between-default-and-j2d), is the slowest of the renderers that Processing ships with. Processing also comes with the P2D, P3D, and PDF (for rendering PDF's) renderers. Both P2D and P3D use OpenGL; the 2D and 3D versions are optimized for 2-D and 3-D rendering, respectively.

Swapping out the default renderer from JAVA2D to P3D improved the performance from being unacceptably latent, to being effective for a real-time system.

Here's what the sketches' setup function looked like before these modifications:

```
void setup() {
  fullScreen();
  ...

}
```

And after setup() was modified with a different renderer, and a higher frame-rate:

```
void setup() {
  fullScreen(P3D);
  frameRate(600);
  ...
}
```

Using one of Processing's OpenGL based renderers seems to be the way to go if your sketch is receiving network data that's not called in a callback, but rather in the draw() method. Make sure to test the value you pass to frameRate(), as an optimal value will be dependent on how complex your sketch is.
