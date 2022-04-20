---

title: "Machine-Learning Powered Gesture Recognition on iOS"
date: "2017-10-07"
---

It’s almost hard not to be reading about machine learning these days — and that trend will only increase. Machine learning is opening up powerful new capabilities for smartphone apps, from image classification to facial recognition. 

It’s also capable of identifying how someone is interacting with their smartphone.

This project shows how to use the [Gesture Recognition Toolkit](https://github.com/nickgillian/grt) to detect how a user is moving their phone through physical space. (I wrote up a quick guide [here](https://medium.com/@narner/integrating-the-grt-into-an-iphone-app-part-one-1b12dc69c5bc) on how to begin integrating the GRT into an iPhone app project). 

The iPhone’s accelerometer will be used as the input for the gesture recognition system. This all for the app to extract gestural intent from the way the iPhone is moved by the user. The data will be fed into a classifier that’s part of a GRT pipeline, which will predict what gesture the user is performing. To access this sensor data, the app will make use of Apple’s [CoreMotion Framework](https://developer.apple.com/documentation/coremotion). The fine folks at NSHipster have a good overview of the framework [here](http://nshipster.com/cmdevicemotion/). 

The app will have two modes, and two view controllers accordingly. The first one, shown below, is the training mode. While we’re making the corresponding gesture with the phone, the red “Train” button is held down. While it’s held down, data from the phone’s accelerometer is being saved to a data structure that we’ll use to train our pipeline. As soon as the button is let go from being held down, that process is stopped. A segment controller will serve as a way to set the class-label for the gesture that’s being performed.

![Training+VC](/blog_assets/2017/Training+VC.jpg)



The second View Controller is the real-time prediction mode. When we get to this screen, our pipeline will be quickly trained on the data structure. 

When gestures are performed, the app will display the predicted gesture output on the screen, as well as print the predicted class to the console when in debug mode. 

![Prediction+VC](/blog_assets/2017/Prediction+VC.jpg)

&nbsp;



The App Delegate contains a global instance of a GRT pipeline, so that it can be accessed by both View Controllers:

```swift
var pipeline: GestureRecognitionPipeline?

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

    //Create an instance of a gesture recognition pipeline to be used as a global variable, accesible by both our training and prediction view controllers
    self.pipeline = GestureRecognitionPipeline()

    return true
}
```

&nbsp;



An AccelerometerManager class will allow the two View Controllers to have access to the X, Y, and Z coordinates from the phone’s accelerometer. The start method, shown below, will create an object that will return the accelerometer data:

```swift
func start(_ accHandler: @escaping (_ x: Double, _ y: Double, _ z: Double) -> Void) {
    let handler: CMAccelerometerHandler  = {(data: CMAccelerometerData?, error: Error?) -> Void in
        guard let acceleration = data?.acceleration else {
            print("Error: data is nil: \(String(describing: error))")
            return
        }

        accHandler(acceleration.x, acceleration.x, acceleration.z)
    } 

    motionManager.startAccelerometerUpdates(to: motionQueue, withHandler: handler)
}
```

The stop method will halt this process, so that the app is only gathering data from the phone’s accelerometer when the pipeline is being trained, or performing real-time recognition. 

&nbsp;

## TRAINING VIEW CONTROLLER

The heart of the Training View Controller is the `TrainBtnPressed` function. It’s called whenever the red “Train” button is held down, takes the accelerometer coordinates, and creates a vector out of them. This vector is then sent to the pipeline wrapper’s `addSamplesToClassificationData` function.

```swift
func TrainBtnPressed(_ sender: Any) {
        trainButton.isSelected = true
        let gestureClass = self.gestureSelector.selectedSegmentIndex

        accelerometerManager.start({ (x, y, z) -> Void in

            //Add the accellerometer data to a vector, which is how we'll store the classification data
            let vector = VectorFloat()
            vector.clear()
            vector.pushBack(x)
            vector.pushBack(y)
            vector.pushBack(z)

            print("x", x)
            print("y", y)
            print("z", z)
            print("Gesture class is %@", gestureClass);
            self.pipeline!.addSamplesToClassificationData(forGesture: UInt(gestureClass), vector)
        })
    }
```

Once the gesture data has been recorded, and the “Save Pipeline” button is pressed, the pipeline and gesture training data is saved. 

&nbsp;

## PREDICTION VIEW CONTROLLER  

Once the pipeline has been saved, the app can be used in real-time prediction mode. This view controller will take the accelerometer data, and pass that to the pipeline using the “predict” function.

What’s happening here is that the pipeline is comparing the incoming accelerometer data against the labeled training data, and making a prediction as to what gesture the user has made with their phone. This data is represented with the classLabel property.

The app will first make sure sure that it can successfully load the `train.grt` pipeline file and `trainingData.csv` classification data files. If it can, then a call to the pipeline’s `train` method is made, which is located in the Objective-C wrapper for the GRT:

```objective-c
- (BOOL)trainPipeline {
    *self.trainingData = self.classificationData->split(80);
    BOOL trainSuccess = self.instance->train( *(self.trainingData) );
    ...
    return trainSuccess;
}
```

Back in the Prediction Controller, the performGesturePrediction() method takes the real-time accelerometer coordinates, and puts them inside of a Vector:

```swift
func performGesturePrediction() {
        accelerometerManager.start { (x, y, z) -> Void in
            self.vector.clear()
            self.vector.pushBack(x)
            self.vector.pushBack(y)
            self.vector.pushBack(z)
            ...
```

This vector is then passed to the pipeline’s Objective-C wrapper:

```swift
self.pipeline?.predict(self.vector)
```

The remainder of the function gets the predicted class label from the pipeline, and displays the predicted gesture result in the user interface:

```swift
...
            DispatchQueue.main.async {
                self.predictedGestureLabel.text = String(describing:                 self.pipeline?.predictedClassLabel ?? 0)
            }

            print("PREDICTED GESTURE", self.pipeline?.predictedClassLabel ?? 0);
        }
    }
```

&nbsp;

## USING THE APP 

One important consideration to keep in mind when implementing a real-time gesture recognition system is to train the system what *not* to recognize. In this case, the resting state of the phone (when a gesture isn’t being performed with it) is when the phone is held stationary:

![StationaryPosition](/blog_assets/2017/StationaryPosition.jpg)

​	

I was able to train the gesture recognition system to be able to recognize a rapid shaking of the phone:			

![Gesture1](/blog_assets/2017/Gesture1.gif)



…a side swipe:

![Swipe+Gesture](/blog_assets/2017/Swipe+Gesture.gif)



…and a “wave motion”:

![Swipe+Gesture](/blog_assets/2017/WaveGesture.gif)



If you end up using this demo to recognize some interesting gestures, please feel free to share your results in the comments! Remember that the robustness of the gesture recognition system will depend on how well you train for the specific gestures that you want to be recognized. 

The project code is on GitHub [here](https://github.com/narner/GRT-iOS-HelloWorld).