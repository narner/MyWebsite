---
title: "Integrating Arduino-Bluetooth Sensors with iOS"
date: "2017-09-05"
---

One area that I've been exploring recently is Bluetooth communication between sensor-circuits and iOS apps. I wanted to share one of these studies, based on some of the examples provided from the good folks at Adafruit. It consists of a sensor that can detect the presence of a flame, and send that information over Bluetooth to an iPhone app, which displays the reading from the sensor.

Here's what it looks like in action:

![Demo](/blog_assets/2017/Demo.gif)

For this study, I used Adafruit's [FLORA](https://www.adafruit.com/product/659) family of Arduino-based boards for prototyping. The FLORA boards are fully compatible with the Arduino IDE,  and can be easily used in wearables projects.

The Bluetooth board I used was the [FLORA-Bluetooth Module](https://www.adafruit.com/product/2487), and the sensor is a flame sensor that was included in the [Inland 24 Sensors Kit](http://www.microcenter.com/product/435332/24_Sensors_Kit).

Here's the completed circuit:



![iOS_Arduino](/blog_assets/2017/iOS_Arduino.jpg)

&nbsp;

## ARDUINO SKETCH

In order to run the Arduino sketch, you'll need to make sure you have:

- The latest version of the [Arduino IDE](https://www.arduino.cc/en/Main/Software)
- The [Adafruit AVR boards package](https://learn.adafruit.com/add-boards-arduino-v164/installing-boards) installed
- The [Adafruit BluefruitLE nRF51 library](https://learn.adafruit.com/adafruit-flora-bluefruit-le/installing-software) installed

The sketch is build off of the [blueart_cmdmode sketch](https://github.com/adafruit/Adafruit_BluefruitLE_nRF51/tree/master/examples/bleuart_cmdmode) that's included in the Adafruit BluefruitLE nRF51 library. Most of the sketch is boilerplate code that's used to configure the FLORA and the FLORA Bluetooth module.

Inside the loop function is the code that will get the value from the flame sensor, scale it, and determine whether a close flame (less than 1.5 feet away), a far flame (1-3 feet away), or no flame was detected:



```c
// read the sensor on analog A9:
int sensorReading = analogRead(A9);
// map the sensor range (four options):
// ex: 'long int map(long int, long int, long int, long int, long int)'
int range = map(sensorReading, sensorMin, sensorMax, 0, 3);

// range value:
switch (range) {
		case 0:    // A fire closer than 1.5 feet away.
			Serial.println(0);
			Serial.flush();
		 break;
		case 1:    // A fire between 1-3 feet away.
			Serial.println(1);
			Serial.flush();
		 break;
		case 2:    // No fire detected.
			Serial.println(2);
			Serial.flush();
		break;
	}
...
}
```

&nbsp;

## iOS APP

The iOS app is a stripped-down version of the [Basic Chat app](https://github.com/adafruit/Basic-Chat) made by Adafruit, which shows how to implement two-way communication between an iOS app and a Bluetooth-enabled Arduino.

I modified the app for this demo so that it only receives information from the Bluetooth-sensor, as for this application, I only need to send information from the sensor to the iPhone; there's no need for two-way communication.

The [Adafruit Basic Chat tutorial](https://learn.adafruit.com/crack-the-code/sending-and-receiving-data-with-arduino) gives a good overview on how Bluetooth communication works with iOS. For this example, the most important part is the updateIncomingData function, which will listen for incoming Bluetooth data from the FLORA sensor, and update the UI accordingly:



```swift
func updateIncomingData () {
     NotificationCenter.default.addObserver(forName: NSNotification.Name(rawValue: "Notify"), object: nil , queue: nil){
     notification in

            let dataString = characteristicASCIIValue as String
            if (dataString == "0"){
                self.stateLabel.text = "Close Fire"
                self.backgroundView.backgroundColor = UIColor.red
            } else if (dataString == "1") {
                self.stateLabel.text = "Distant Fire"
                self.backgroundView.backgroundColor = UIColor.red
            } else {
                self.stateLabel.text = "No Fire"
                self.backgroundView.backgroundColor = UIColor.white
            }
        }
    }
```

&nbsp;

## RUNNING THE SKETCH AND APP

- Upload the sketch to your FLORA board. Open the Serial Monitor to verify that the sketch has been uploaded, and that the Bluetooth sensor has been configured.
- Run the iPhone app on a physical device. Make sure you have Bluetooth enabled. When you see your Arduino show up, click "Connect".
- Use a lighter to test that the system is working.

The repository for the project can be found [here](https://github.com/narner/iOS-FlameSensor-Bluetooth-Study).
