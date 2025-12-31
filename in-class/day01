---
title: "Day 1: Welcome!"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day01/#today
  - title: For Next Time
    link: in-class/day01/#for-next-time
  - title: Broader Impacts of Robots in the World
    link: in-class/day01/#robots-in-the-world
  - title: Sensory-Motor Loops
    link: in-class/day01/#sensory-motor-loops-legacy-material-if-you-are-interested
---

## Today

* Intro to CompRobo (<a href="https://docs.google.com/presentation/d/1T-gY0nTB7Gx2i21T01CFSnopaKo8oz7pxvk4z9us4Jk/edit?usp=sharing">slides</a>)
* Broader Impacts of Robots in the World

## For Next Time

* Mandatory Tasks:
* *  Fill out <a href="https://forms.gle/Di9V4Kbj8Tuv3rJh6">the course entrance survey</a>
* * <a href="../How to/setup_your_environment">Get your environment setup</a>
* * Go through the following ROS2 tutorials ([turtlesim and rqt](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html), [Understanding nodes](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html), [Understanding topics](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html))

* Suggested Tasks:
* * Check out and consider joining the class [Discord](https://discord.gg/jZVnNMEk)
* * Start thinking about <a href="../assignments/class_yoga">your course goals</a> (Due Sept 9th at 1PM)
* * <a href="../How to/use_the_neatos">Connect a Neato and run the Neato simulator</a>.  Explore the topics that are published and see what you are able to do with them given the tools you learned about in the ROS2 tutorials.
* * Start working on <a href="../assignments/warmup_project">the RoboBehaviors and FSMs Project</a> (Due Sept 23rd at 7PM)
* * Start thinking about <a href="../assignments/broader_impacts">what robot you might want to contemplate for the semester</a> (Due Sept 30th at 7PM)


## Broader Impacts of Robots in the World

Robots have already transformed our society, and their impact seems likely to only increase over time.  One thing we will do this semester is discuss the effects of robots on our world and our responsibilities as roboticists to understand their implications.

To kick-off these discussions, and to jumpstart your thinking on the first phase of the Broader Impacts project, please get into a group of 3-4 and work through the following exercises. We'll have a share-out as a class at the end where your group can present a response to one of the exercises.

1. _Suppose you are working as an engineer on one of the robotics projects highlighted in the slides (it will probably help for you to have a specific one in mind)_. What are the key benefits of your robot? What potential issues might you consider? What engineering processes would you follow to ensure your design works as you intend it to? What is one burning question you currently feel unequipped to resolve (this might be an opportunity for us to learn together as a group).

2. _Suppose you are one of the decision-makers at a robotics company that had developed some incredible new technology (you can pick a specific piece of tech or think about this in a more general sense)_.  How would you decide who to license your technology to? What factors would you consider?  What safeguards (if any) might you need to put in place when licensing this technology? What is one burning question you currently feel unequipped to resolve (this might be an opportunity for us to learn together as a group).

3. _Suppose you are a policy-maker tasked with considering whether new laws, regulations, or guidelines on an emerging robot technology should be developed (you can pick a specific piece of tech or think about this in a more general sense)_. What would assist you in determining your policies and laws regarding robotics? Who would you want to talk to about the technology? Are there any blanket regulations or laws you would put in place around the use of robots and robotics technology?  

4. What do you think is the most beneficial _application_ of robotics technology? Why?


## Sensory-Motor Loops (legacy material if you are interested)

In our view, at their very core, robots are about sensory-motor loops.  We can visualize this relationship in the following way.

<p align="center">
<img alt="A diagram showing a robot sensory motor mapping interacting with an environment" src="day01images/sensorymotorloops.jpg"/>
</p>

This feedback loop can cause very simple sensory-motor mappings to exhibit complex behavior.  In fact, you will be amazed at the capabilities of some of the very first robots.  Meet the Neato's predecessor! (apologies for the antiquated language)

<iframe width="560" height="315"
src="https://www.youtube.com/embed/lLULRlmXkKo"
frameborder="0" 
allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" 
allowfullscreen></iframe>

The pioneering work of Grey Walter was followed up by a number of others.  One particularly interesting work was by Valentino Braitenberg.  Valentino Braitenberg was interested in how very simple sensory-motor loops could elicit behaviors that when viewed by humans would evoke emotion and feelings of intelligence and intentionality (on the part of the robot).  The name typically used to refer to these hypothetical robots is "Braitenberg Vehicles".  While Braitenberg never actually built these robots (he was more interested in how these simple robots might inform various philosophical issues, particularly in the philosophy of mind), others have followed up and actually built these robots.

Here is a video from a group at MIT that built several of Braitenberg's vehicles.

<iframe width="560" height="315"
src="https://www.youtube.com/embed/VWeRC6j0fW4"
frameborder="0" 
allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" 
allowfullscreen></iframe>

### Base Robot

The robots in the video are quite simple.

**Motor system:** the robot moves using something called differential drive.  All this means is that when both wheels move in the same direction (either forwards or backwards), the robot moves in a straight line in the same direction (assuming the wheels move at roughly constant speed).  If the wheels move in opposite directions the robot will spin in place (which direction the robot spins depends on which wheel is moving forward and which is moving backward).

**Sensory system:** the robot has three different types of sensors:
* A whisker sensor that can tell when something is touching it (you can see that in the video under "insecure")
* A bump sensor that turns on when it runs into something
* A light sensor that emits higher values when in the presence of bright light and lower values when in darkness (you can think of this as very primitive single pixel camera!)

In your breakout rooms you will design your first sensory-motor mappings, and by doing so, we will define our first robot programs.  Instead of using a programming language, you will write your programs graphically on (Zoom) whiteboards.

For instance, suppose we wanted to implement the behavior of the wary robot whose behavior mainly consists of moving forward until it encounters a shadow (and then stopping).  First, let's draw our robot and label it's motors and sensors:

<p align="center">
<img alt="A diagram of a two-wheeled robot with a single light senosr" src="day01images/diagram01.jpg"/>
</p>

We will use the symbol $$m_1$$ to refer to the left wheel (positive means the wheel is moving forward, negative is backwards, and 0 is stopped).  $$m_2$$ is similar to $$m_1$$ but for the right wheel.  the symbol $$l$$ refers to the value of a light sensor (the higher the value the more light detected).

In the case of robots whose behavior is purely based on their current sensor values, we can fully specify the robot's behavior by defining the sensory-motor mapping.  We know we want the robot to move at full speed in situations with a lot of light and stop in shadows.  In order to express this mapping, we can use a simple plot to show the relationship between the two motor outputs and the light sensor input.

<p align="center">
<img alt="A functioning with light on the x-axi sand motor speed on the y-axis" src="day01images/diagram02.jpg"/>
</p>

This seems reasonable, but how can we build confidence that this is the correct mapping before we implement.  One way is to put the robot in a specific situation and to simulate the behavior that would be generated by the sensory-motor mapping above.  Next, I will show three key frames form a situation that help to build confidence that the mapping above is reasonable.

<p align="center">
<img alt="A key frame of the robot approaching a darkened area" src="day01images/diagram03.jpg"/>
</p>

First, the robot is in full light (a.l.l. stands for ambient light level) and thus the two motors are at roughly full speed.

<p align="center">
<img alt="A key frame of the robot approaching a darkened area" src="day01images/diagram04.jpg"/>
</p>

Next, the robot has moved partially into the shadow (depicted with black lines) which causes the light sensor value to drop and the speed to drop to about half speed.

<p align="center">
<img alt="A key frame of the robot approaching a darkened area" src="day01images/diagram05.jpg"/>
</p>

Finally, the robot is sufficiently into the shadow that it stops entirely.
In your breakout rooms, work through generating robot programs to realize the behaviors in the video. 

***Questions to keep in mind while doing this activity***
1. Depending on some of the assumptions you make about how the motor system works, one of the behaviors cannot be reproduced without some form of memory.  Which behaviors are these?  How can you tell? 
2. In what interesting ways could the light sensor and the whisker sensor be combined in a single video?  What would the resultant behavior of this vehicle be?  What would you call your new vehicle?

### Braitenberg Vehicle Simulations

In this next part of the assignment, we'll be using a [simulator for Braitenberg vehicles](http://www.harmendeweerd.nl/braitenberg-vehicles/) to build a more quantitative understanding of sensory-motor loops.  In order to use the simulator, you can navigate to the linked page and perform the steps shown in the video below.  This example shows how to load the "love" Braitenberg vehicle.

<p align="center">
<img alt="a Braitenberg vehicle called Love being simulated" src="../website_graphics/braitenberg_love.gif"/>
</p>

As you can see in this video, this robot uses its light sensors in order to approach and then stop at light sources.  To explore what the "love" vehicle is doing we can click on the various light sensors of the robot to see how they are wired to the robot's motors.

<p align="center">
<img alt="exploring the light sensors on the love vehicle" src="../website_graphics/braitenberg_love_sensors.gif"/>
</p>

Notice how the only difference when we click on one sensor or the other is which motor it is wired to.  The right sensor is wired to the right motor and the left sensor is wired to the left motor.  From the visualization you can see that the baseline speed of the motor is 5, but that for each unit of light that the sensor sees, the speed goes down be 0.4 (since sensor strength is $$-0.4$$).

1. Similar to what you did before, draw the relationship between the left and right motor speeds and the light sensor value (you will have to make some assumptions here).
2. Suppose you wanted the love vehicle to stop farther away from the light source, how would you modify the vehicle to achieve this?  Are there multiple approaches?  Try one of your solutions out in the simulator.
3. Once you've implemented your solution on the simulator, try placing your robot in various locations.  Are you surprised by any of the behaviors?  Try to make sense of what you see by referring back what you know about the sensory-motor mapping.


### Additional Exercises

At this point, you can feel free to come up with your desired behavior and see if you can create a Braitenberg vehicle to realize this behavior.  Instead, you can also work on creating vehicle for each of the following behaviors.  ***We are intentionally giving more than you can probably do in during class, so don't feel like you should be able to finish all of these.***

1. Build a vehicle that will turn towards a light source if it passes it on its left side and turn away from it if it passes a light source on its right side.
2. Build a vehicle that, like the love vehicle, approaches light sources.  The key difference is that this vehicle should stop with its front at a 45 degree angle from the light source.
3. Build a vehicle that orbits around light sources. 
4. Build a vehicle that drives back and forth over a light source.
5. Can you think of a behavior that *could not* be realized in the simulator (i.e., there is no possible way to configure the vehicle to achieve the behavior)?
6. Come up with an interesting behavior and see if you can replicate it in the simulator. 

### Additional resources

* [Braitenberg creatures](https://cosmo.nyu.edu/hogg/lego/braitenberg_vehicles.pdf) (this paper describes the LEGO robot in the video embedded above).
* [Social Integration of Robots into Groups of Cockroaches to Control Self-organized Choices](https://science.sciencemag.org/content/318/5853/1155) cool paper showing robots influencing actual cockroach collective behavior!
* For even more crazy ideas for sensory-motor loops, checkout [the Wikipedia page on BEAM robotics](https://en.wikipedia.org/wiki/BEAM_robotics).
