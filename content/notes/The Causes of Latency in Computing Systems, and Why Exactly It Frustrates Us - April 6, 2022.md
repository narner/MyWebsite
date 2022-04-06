---
title: "The Causes of Latency in Computing Systems, and Why Exactly It Frustrates Us"
date: "2022-04-06"

---



One of the most frustrating experiences about using computers is when you try to do something, and the system freezes up or is otherwise slow. This is latency - the unacceptably large amount of time between when an action is triggered by the user and the response to that action by the computer is relayed to the user. 

It’s something that people have come to expect as a “natural” consequence of using computers, but frankly, it’s an utterly unacceptable one. Latency is the consequence of a pile of decisions made by technologists that result in a poor user experience for the people that they’re creating computers and software applications for. No one ever intends to write software that is slow or unresponsive - but it is an experience that is the result of decisions made by people. 

In this post I want to explain exactly why computer latency is bad, outline some of the factors that contribute to latency, and then offer some possible ways that latency in software can be reduced. 



**Latency and Flow State**
When a person is using a computer and experiences latency, it breaks their flow state. In the context of computing technology, flow is described as “…the natural, fluid state of being productively engaged with a task without being aware of the technology that is driving it” [1]. 

If this state of flow is broken due to software being unresponsive or slow to respond to user input, they are distracted from the task at hand that they are trying to accomplish, and end up having to spend mental energy assessing what is wrong with the software or computer they’re using, or they have to wait for the computer to respond to their input and in the meantime get distracted by something else. 

It takes energy to focus on accomplishing a task - if someone’s focus keeps getting interrupted by their computer being slow, users’ loose focus on what they were doing. Good software helps people get into a flow state and stay there while they’re working on their task at hand. 

When SW is laggy, people lose attention + forget what they were doing in the first place, and it takes longer for them to complete their task than it otherwise normally would. Latency actually affects our perception of time - when people are in a flow state, we actually notice the passage of time less [3]. When that flow state is interrupted, we become much more attuned to the passing of time - and how long it may take the software we’re using to respond to our input or show us the result of some task. When this happens, the User Response Time - “the time span between the moment receives a complete reply to one command enters the next command” [2] - increases. 




**Latency and the Materiality of Computation**
Latency increases the distance between the human and the material of computation. When we interact with objects in the physical world, we have immediate, direct feedback to our sense based on the response of the object to our interactions with it. We act on and in the world around us, and receive immediate feedback for our actions. 

When we use a computer, we use it as an intermediary to the realm of computation. We interact with this world limited by two things: 

* The speed of our thoughts
*  The speed at which we can communicate those thoughts to the computer

This communication occurs through physical peripherals like mice, keyboards, and perhaps someday; brain-computer interfaces. 

It is vital that are thoughts and intentions are communicated to the computer, processed by the OS and the software program that we’re using, and display the results back to the user as fast as possible. 

Any time there is some latency in this loop, the process of a person communicating intent and then receiving results from the computer gets delayed.

This latency adds up over time - it may start with a bad keyboard, followed by a block at the OS layer, followed by an application written in a UI framework with many different layers between processing the keyboard input and actually showing the result of the input on the screen, oh and there may be some other bug that’s caused by some just bad code in the application…

Latency is something that software engineers have to take seriously - you cannot always guarantee that your software will be running on the latest and greatest hardware, nor can you guarantee how many other programs may be running in the background at the same time as the one you worked on. Your snippet of code may only be slightly un-optimized, but, in the whole of the entire program, all of these un-optimized code snippets can add up to a slow piece of software!

Additionally, even if your users *are* using the latest and greatest hardware, that in no way is a guarantee that they will never experience latency as a result of your application. Speed and power are no guarantees that software will always be responsive - well written software, without a dearth of complexity, will. 

Doherty and Sorenson, in discussing a passage from “The Design of Everyday Things”, state “…every system inherently has a model built into it, and every user has a mental model that corresponds to a system model. When devices and products are designed to fit with users’ mental models of what they expect of operations and technology - where their instincts about what should happen next and how to make something happen are right - those devices and products will be perceived by the user as intuitive and desirable” [5]. The reason latency is so unacceptable from a cognitive level is that the connection between a user’s mental model and the system model becomes stressed due to the flow of interaction being interrupted by a system with latency. 




**Adaptation to Latency**

If latency in a system is consistent and predictable, people will be able to adapt their usage to the latency. If the latency is random and difficult to predict, their confidence in the system will diminish [5].

If the latency is consistent and predictable, then people will know what to expect when using the system, and adjust their behavior accordingly. They’ll be less likely to get frustrated with something that’s consistently latent, as opposed to a system that is latent at random occurrences. 




**Latency and Trust**
Latency in a computing system will inspire distrust in the person using the system. They’ll either:

* Think the system is inherently broken, and liable to lose their data + work 
* They will distrust themselves…whatever they do, whether typing or pressing a button, is what is causing the system to be slow and unresponsive. They’ll start to think “I’m bad with computers”

This is all unacceptable! Computers should never make people feel dumb or incapable; they should help us accomplish our goals, whatever those may be, and then get out of the way. 




**Software Development**

Pekka Väänänenn writes that “The big problem with latency is it accumulates. Once some component introduces delay somewhere in the input chain you aren’t going to get it back. That’s why it’s really important to eliminate latency where you can” [4].

Latency can get baked into software programs at a variety of levels. It may not even be noticed by the people writing the software due to a variety of factors:

* Testing only in a development environment, and never 
in real world conditions
* Not testing on a variety of OS versions
* Not testing across all possible hardware or hardware configuration options

Software has gotten increasingly complex as the history of computing has evolved. This has made it increasingly harder to write and *maintain* quality software, the latter a point that Jonathan Blow expounds upon [here](https://www.youtube.com/watch?v=ZSRHeXYDLko).

This ever-increasing complexity can have detrimental side effects, including: 

* More difficulty for developers to add or remove components from a SW system
* Difficulty finding and fixing bugs
* Latency 

Overall software system performance is determined by the way that all the sub-components of a system interact with each other. If one component has introduced an amount of latency, then that will affect the rest of the components it interacts with.



**Closing Thoughts**

Latency is a detriment on the experience of using a computer - what could be a productive, or in some cases even joyful experience ruined by the fact that the computer does not respond to the input or actions of the person using it. 

There’s not necessarily any one singular cause of latency in modern software - it’s caused by many different things up and down the HW/SW stack. None of these things in and of themselves may cause a significant amount of latency. However, all of these things can add up to produce a poor experience for the person sitting down at their computer to do the task that they had in mind. 

Technologists can not assume that simply having ever increasingly faster and more powerful CPU’s or GPU’s will keep people from having an experience where their computer is frustratingly slow or even non-responsive. We have to ensure that care is taken not to introduce latency at all levels of the HW/SW stack - from ensuring that there is high bandwidth between the CPU and memory to well designed OS’s to SW frameworks that are free from cruft and easy to modify to well designed and built applications. 




[1] [Keeping users in the flow: Mapping system responsiveness with user experience](https://nickarner.com/cited_papers/keeping-users-in-the-flow-mapping-system-responsiveness-with-user-experience.pdf) 

[2] [The Economic Value of Rapid Response](https://jlelliotton.blogspot.com/p/the-economic-value-of-rapid-response.html)

[3] [Subjectively Experienced Time in HCI](https://nickarner.com/cited_papers/Subjectively_Experienced_Time_in_HCI.pdf)

[4] [Desktop compositing latency is real and it annoys me](http://www.lofibucket.com/articles/dwm_latency.html)

[5] [40 years of searching for the *best* computer system response time](https://nickarner.com/cited_papers/Experience_of_time_in_HCI.pdf)



**Other Resources**

* [Computer latency: 1977 - 2017](https://danluu.com/input-lag/)
* [Delays in Human-Computer Interaction and Their Effects on Brain Activity](https://nickarner.com/cited_papers/Delays_in_Human-Computer_Interaction_and_Their_Effects_on_Brain_Activity.pdf)
* [Keyboard latency](https://danluu.com/keyboard-latency/)
* [Keyboard v. mouse](https://danluu.com/keyboard-v-mouse/)
* [Lag as a Determinant of Human Performance in Interactive Systems](https://nickarner.com/cited_papers/Lag_as_a_determinant_of_human_performance_in_inter.pdf)
* [Latency - the *sine qua non* of AR and VR](https://perma.cc/J29Q-KEQ8)
* [Performance Matters](https://www.hillelwayne.com/post/performance-matters/)
* [Response Time and Display Rate in Human Performance with Computers](https://nickarner.com/cited_papers/Response_Time%20and_Display_Rate_in_Human_Performance_with_Computers.pdf)
* [Response Times: The 3 Important Limits](https://www.nngroup.com/articles/response-times-3-important-limits/)
* [Scrolling with pleasure](https://pavelfatin.com/scrolling-with-pleasure/)
* [System Latency Guidelines Then and Now - is Zero Latency Really Considered Necessary?](https://nickarner.com/cited_papers/System_Latency_Guidelines_Then_and_Now_is_Zero_Latency_Really_Considered_Necessary.pdf)
* [Typing with pleasure](https://pavelfatin.com/typing-with-pleasure/)
* [The importance of percent-done progress indicators for computer-human interfaces](https://nickarner.com/cited_papers/The_importance_of_percent-done_progress_indicators_for_computer-human_interfaces.pdf)
* [User-Acceptance of Latency in Touch Interactions](https://link.springer.com/chapter/10.1007/978-3-319-20681-3_13#:~:text=Our%20results%20show%20acceptable%20latency,be%20fulfilled%20for%20drawing%20conclusions)
