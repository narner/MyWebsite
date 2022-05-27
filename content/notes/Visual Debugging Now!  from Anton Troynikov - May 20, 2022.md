---
title: "Visual Debugging Now! - from Anton Troynikov"
date: "2022-05-20"

---



*Guest post from Anton Troynikov*



Over the last seven or so years of working in robotics and computer vision, the bitter lesson I’ve learned is that the bug is almost always in the part of the algorithm you didn’t visualize. 

This is a call to fix that problem once and for all. 



## The Problem

Many domains, including robotics, foundationally rely on highly performant numerical software. But since humans are adapted to understand space visually, we don’t have good intuitions for numerical data. Things that are obvious to human vision like “does this have a realistic pose in space” are very unclear in numerical representations - can you picture a quaternion intuitively from its vector representation? Can you see from a Hessian matrix whether your optimization is converging? 

Visualizing this type of data is the obvious thing to do to make it understandable by humans, but this is very difficult for a variety of reasons. 

Code in these domains is usually written in Python (prototyping) or C++ (production). 

* In Python there are readily available viz modules, and people use them. They suck because they’re mostly not interactive and are effectively stuck in the 70s. There are JavaScript things you can use to make interactive but you have to understand JavaScript as well as Python (on top of understanding the work you’re actually trying to get done) to try to use them. 

* Interactive viz in C++ is difficult. You need to know some viz framework (Pangolin, QT if you’re a crazy person, or some in-house flavor), and even then it’s not fluid with your actual development work. You write viz when you’re done, not as an aid to development, and that’s if you get around to it. The viz you write isn’t interoperable or collaborative. If you somehow magically did have all this, you would still have to recompile and re-run to update the viz when you change the algorithm, or want to visualize something different. 

There are plenty of additional problems this causes:

* Nobody’s viz matches up with anybody else’s. You have no idea if you’re even looking at the same thing as your colleague. So sharing sucks. 

* C++ visualizations are local (unless you really want to fuck around with X lol) so you can’t even share them remotely, just screenshots (robotics is about the dynamic world so…)

How do teams get around this? 

* They simply don’t and life sucks
* They develop some big monolithic in-house visualizer which is mostly designed to visualize the final system or module output, so at least everyone speaks the same viz language and some form of collaboration is possible. 

Rendering out just the final output of the algorithms (e.g. robot behavior) or ROS messages or whatever, is too far removed from the inner loop of development. You have to build the whole thing end-to-end before you even start finding the bugs in the core loops, which might not even appear for a long time if the behavior is sufficiently constrained. 

The real world is complex, and bugs in numerical computing can be very subtle. The system can still ‘basically work’. I have horror stories about absolutely incredible bugs in perception systems discovered years after the system was ‘in production’. I have seen hundreds of engineer-hours wasted chasing a simple sign change which would have been obvious if it was easy to look into the tight inner optimization loops. 

Naturally the situation creates a huge drag on velocity and overall just a train wreck. Is my bug in my code or in the code for my viz? The bug is probably in the part we can’t see, so we have to infer something from the output about what’s broken. Hope you like printf.



## What To Build

You know what doesn’t suck as an environment for numerical computing? MATLAB. 

MATLAB kicks ass at viz. Every single object is a matrix, an array, or a struct which eventually recurses to either a matrix or an array at the leaves. MATLAB visualizes its basic data structures natively and interactively, and can display and update them in real-time. The execution environment is also the IDE and debugger, so you can add a breakpoint, pause execution conditionally, and call a viz function in the middle of your code running, then change some variables (but not the code) and see what happens. 
It’s not perfect - you can’t alter code in-place while debugging, and it’s not performant as a production language. For prototyping spatial computing algorithms it beats the shit out of Python (and has faster numerics and better libraries), but ML people use Python so everyone uses Python. 

So build a MATLAB-like environment but for Python and C++. Here’s how you do it:

* Extend your favorite C++ compiler with a flag that generates python bindings for all data variable symbols. The user could annotate this or you can just press some “do it smart” button so you don’t include e.g. symbols in 3rd party libs. 

* Execute the compiled C++ code in a GDB-like debug context so the user can insert breakpoints etc. in a running process. But the debugger is programmable itself, and uses Python natively as a scripting language. At each breakpoint you can read variables from the running C++ process into the Python environment. Instantly you can do two things: alter variables in flight (like MATLAB), and use Python native viz (not interactive, but we can improve here). You can then fix bugs by patching them in Python in the debugger and modifying the data in-place, then when you’re confident everything is good, update your C++ code, compile and run. 

* Python viz sucks by default but it *is* amenable to the web. The other thing this debug environment does is spin up a little webserver, that’s where the viz is rendered. You standardize the visualization interface, not the primitives - i.e., just like MATLAB says “everything is a matrix”, you treat the user’s data as a matrix of numerical values (the user can write transforms); for spatial computing almost all of it already is. Then you have primitives that render that data just like MATLAB does, but interactively. 

* Web-based rendering means collaboration is built in and you can share viz in real-time. There’s a bunch of other features like state playback etc. that are harder to build and not strictly necessary right away but would be very useful too. 

Combined with a library of shared visualization transforms, this ‘visual numerics IDE’ will accelerate a large fraction of the programming work for some of the most important disciplines in technology, and meets programmers where they are instead of forcing them to learn something new or creating another standard they must switch to. 

There is probably initially very little money in trying to sell this but there could be huge and important developer adoption, and so it must be built as an open source project. If you are interested in building this, you can find me on twitter at [@atroyn](https://twitter.com/atroyn) .
