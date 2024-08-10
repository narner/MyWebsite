---
title: "Dusty Robotics iPad App"
date: "2023-04-13"

---

I was apporached by [Dusty Robotics](https://www.dustyrobotics.com) to build a prototype of an iPad app that would control their product, the [FieldPrinter](https://www.dustyrobotics.com/fieldprinter).

Dusty's FieldPrinter is a robotic layout system for printing elements of floor plans on a construction site. These elements indicate to the various trade workers where something needs to be built on the construction floor plan.  

At the time, Dusty had a test app they had built in Python for a Windows tablet to control the Field Printer, and wanted to have a more polished product to roll out ot their customers built for the iPad. 

I enlisted the help of a designer friend, [Haris Butt](http://haris.computer), and we worked with the Dusty team to design and build a prototype of this new iPad - focused app. 

My work involved: 

* Building a Swift SDK that wrapped their C-based [Capn'Proto](https://capnproto.org) library for communication with the FieldPrinter.
* Using this to build out 2-way communications between the FieldPrinter and the iPad app
* Built a Sprite-Kit - based interface that displayed
  * The elements of the floorplan from a custom file format
  * The real-time position of the FieldPrinter 
* Teaching the Dusty team the basics of iOS development, Swift programming, and Xcode workflows. 



The project was handed off to the Dusty team, who are continuing to build out the app for their customers. 



![Dusty-Graph](/post_assets/dusty/DustyGraph1.png)

![Dusty-Graph](/post_assets/dusty/DustyGraph2.png)

![Dusty-Graph](/post_assets/dusty/DustyGraph3.png)
