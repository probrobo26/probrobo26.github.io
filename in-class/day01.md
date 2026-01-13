---
title: "Day 1: Welcome! And Probability Axioms"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day01/#today
  - title: For Next Time
    link: in-class/day01/#for-next-time
  - title: Welcome
    link: in-class/day01/#welcome
  - title: Probability Axioms
    link: in-class/day01/#probability-axioms
  - title: Day Activity
    link: in-class/day01/#day-activity
---

## Today
* Welcome to ProbRobo
* Probability Axioms (and Robotics)
* Day Activity

## For Next Time
* Complete the day activity for today's topics (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17542)
* Complete YOGA: Initialization assignment (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17537)

## Welcome
Welcome to the first iteration of _Probabilistic Robotics_. This is a class inspired by the broader field of intelligent robotics (referred to as "embodied intelligence" in modern writing), and focuses on how robots represent and interact with the messy, complex, and uncertain environments in which they find themselves. 

This class is intended to be an advanced algorithms course for robotic systems, and an applied introduction to probabilistic and statistical thinking. To further motivate this class, and set some ground rules for this semester, an introductory lecture with notes is provided [here](https://docs.google.com/presentation/d/1_jppam3AdlEDJVs64v4FJkvvowJL6GSP_NeY8L6qt6g/edit?usp=sharing).

## Probability Axioms (And Robotics)
Continuing with our [welcome lecture](https://docs.google.com/presentation/d/1_jppam3AdlEDJVs64v4FJkvvowJL6GSP_NeY8L6qt6g/edit?usp=sharing), today we'll be kicking off with a few _probability axioms and defining statistics_ -- fundamental formalisms, notation, and ideas that will start us off on our journey.

### Motivation for ProbStats in Robotics
At the highest level, let's imagine a robot as a sensory-motor loop.

<p align="center">
<img alt="A diagram showing a robot sensory motor mapping interacting with an environment" src="../website_graphics/sensorymotorloops.jpg"/>
</p>

A robot uses sensors to measure the world, and then uses actuators to interact with that world. The "magic" in intelligent robotics is the process by which sensor measurements get turned into useful work. Probability and statistics provide fundamental principles and processes by which robots "make sense" of measurements and translate them into motor commands. 

For instance, imagine a robot moving around a building. It is equipped with an inertial measurement unit (IMU) which provides streaming measurements of velocity. Handling this streaming data to get trends in the vehicle's motion, to estimate average speed and heading, and so on -- that's statistics. Going a step further and understanding that these measurements are noisy and imperfect, and thus we label how trustworthy we think those averages are -- that's probability.

Probability and statistics work very closely together in the field of robotics, especially as tasks for robots becoming increasingly complex compared to the sensing information a robot might have available to it to accomplish that tasks. For instance, let's take that same robot from earlier and give it a camera. We ask the robot to navigate to the mailroom on Olin's campus. Given IMU and visual information (pictures) alone, how does the robot conceptualize what a "mailroom" is? How does it plan a trajectory through the space it is in, given that it's never mapped the campus before? How does it know when it is on the right track? These are the types of questions we will explore through the lens of probstats in this class.


### Sample Spaces, Events, and Partitions
Callback to ModSim: probability and statistics are fields related to abstracting the world around us into atomic forms that we can perform work over (i.e., computation). Thus, we first need to become acquainted with the language we'll be using to express the world (or robot, or actions):

**Sample Space**: (aka outcome space, possibility space) the set of all possible outcomes or results of an experiment or random trial. Example: Imagine a door-opening robot. Let's assume it makes an attempt to open a door (the experiment). Then the sample space is the set of all possible states the world could be in after a robot attempts to open a door (e.g., {door closed, door open, door ajar}).

**Event**: a specific outcome of an experiment.
Example: Take our door-opening robot again; an event would be "door closed" following an attempt at door-opening.

**Partition**: a way of dividing the sample space (and/or event). Partitions express subsets of a sampling space. Importantly, partitions are typically defined to be non-overlapping. Example: Imagine our door-opening robot once again. We might partition the sampling space into "closed" and "open" subsets, with the closed partition including the "door closed" event, and the open partition including the "door open" and "door ajar" events. 

In a classical probability class, you might learn about these topics with respect to a population, since this is easy to conceptualize. Let's try that here:

* **Exercise**: Imagine the whole Olin student population; draw partitions representing class years. Then represent the event "taking ProbRobo this semester."

Of course, we're not a classical probability class, so let's think about a robotics scenario and how we might define a sampling space, partition, and event:

* **Exercise**: Imagine a robot manipulator affixed to a table with 3 blocks, 2 apples, and a bin in front of it. Assume it knows that these objects exist in the world, but not exactly where they are. Assume the robot is equipped with a camera and a machine vision algorithm that can classify objects and roughly determine their pose in the world (but that is a noisy measurement). Further, assume that the robot is equipped with a sufficient controller to move the arm to intended locations, but it is imperfect. The robot is tasked with picking up an object and placing it in the bin. For this task, what are some possible sampling spaces we could be operating in? Pick one of those sampling spaces, and define a useful partition over that space (you may argue that there is not a useful partition for the sampling space you chose, but it must be justified). Where might an event in your sampling space appear amongst your partitions?  

Some further properties and aspects on notation:

* A sample space in probability theory may be referred to as $$S$$, $$\Omega$$, or $$U$$ (the latter representing the universal set). Importantly for this class, the sample space should be represented by a capital letter, and should always be defined as the variable representing the sample space when expressing your formalisms.
* An event is a sample of a sample space, such that $$E \in \Omega$$. All events contained in a sample space must be _mutually exclusive_ -- that is, an event must be distinct.
* Partitions are subsets of the sampling space, such that $$A \subset \Omega$$. They have the following properties:
* * $$A_i \neq 0 \forall i$$
* * They are pairwise disjoint (mutually exclusive)
* * All partitions together form the sampling space, $$\Omega$$


### Set Theory
Now we have a way of expressing some environment/robot, what can we _do_ with this formalism (asked another way, what does expressing the world like this afford us)? In this case, at least one useful thing is unlocked for us: set theory (callback to Discrete).

#### A Set
Definition:
Example:

#### Set Operations (for ProbStats)
If we're going to be using this notation to describe the world, we would like it to be able to express some of the complexities about that world -- that's where the notion of set operations comes into play. Some common ones we might encounter include:

* Union
* Intersection
* Mutual Exclusion
* Equal Set

Another aspect that sets support is the notion of applying functions over a set. For instance, if I express $$f(\Omega)$$ then I am performing function $$f(\cdot)$$ over the elements that compose the set $\Omega$. So, if I define $$\Omega = \{0, 1, 2, 3\}$$ and set $$f(x) = x^2$$, then $$f(\Omega) = \{0, 1, 4, 9\}$$. Do note that the operation of the function and the elements inside of the set must be compatible (I could not square a list of strings, for instance). 


### Defining Probability
Sets are statements about the world; how do we evaluate their truth or their ability to come to pass? Enter probability. Formally, probability is a set function whose outcome is a numerical indicator of how likely (probable) the input set of events is to occur.Notationally, we might denote taking a probability as $$\mathbb{P}(\Omega)$$, which can be read as "the probability of events in $$\Omega$$". 

You can find classic examples for understanding probability in discrete mathematics. For instance: what is the probability of a fair die rolling a 5? 

By treating probability as a function, we can express more complicated ideas mathematically: what is the probability that on 10 rolls of a fair die, the outcome is 5 every time? What is the probability that a fair die rolls a 6 and a fair coin flip lands on tails? 

But of course, we're ultimately interested in expressing very, very complicated ideas in the language of probability: what is the probability that there is a deadend around this corner of a building? What is the probability that this is the right place to grab an object to successfully lift it off the table? 

We're going to get there, but first, we need to start to set some ground rules about how probabilities work. Cue the _axioms of probability_.

### Probability Axioms
For probability to provide a meaningful numeric measurement about how probable a given event is to occur, the function must be bounded. There are three common axioms used to define the valid space for probabilities (initially developed by Kolmogorov, with some modern interpretation of the third):

Axiom 1 (Non-Negativity): $$\mathbb{P}(A) \geq 0 \forall A \subset \Omega$$

Axiom 2 (Normalizaton): $$\mathbb{P}(\Omega) = 1$$

Axiom 3 (Countable Additivity): $$\mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B) \text{ if } A \cap B = 0$$

With these axioms, we can make the following observations:

* Numeric probabilities fall within the range of 0 and 1.
* In a given sampling space, the probability of discrete events needs to sum to one. This also implies that events have reciprocal events to balance them (the probability of "A" and the probability of "not A" must sum to 1).
* The probability of the null (or empty) set is 0; it is impossible that nothing occurs. 

**Why this Matters** So, what does this mean for us? Well, we can rely on probability to be a _measure_ (this is a formal mathematical idea; a measure is...) which means that wherever we can define our sampling and event spaces, our probability will be meaningful. The upshot of this: as a robot, we can use probabilities to help us make decisions about actions to take or places to go. Probabilities are _informative_ (another formal mathematical idea related to information theory...we'll come back to this later in the class. For now, assume the colloquial definition holds), and thus can allow us to distinguish between all the options laid out in front of us for how the world might work or how successful our interactions with the world might be.

### Wait...what about Statistics?
We've talked a lot about probability, but what about this whole statistics thing? 

### Going Beyond
To learn more about the topics that we discussed today, you are invited to use our class resources. The following books and chapters are applicable:
* A (Chap A)
* B (Chap B)

## Day Activity
For today's activity, we'll be practicing with and exploring the probstats axioms we just discussed. Please submit a typeset response to all questions provided here. While everyone will turn-in an individual assignment, collaboration is encouraged! Please note down who you worked with during this assignment to practice appropriate attribution and acknowledgement.

### Problem 1
Practice with identifying state spaces, events, partitions for a given scenario. 

### Problem 2
Practice with set theory.

### Problem 3
Practice with probability axioms.

### Problem 4
Practice setting probabilities from word problems (set-up for the robot door-opening scenario)

### Problem 5
Practice with probability axioms.

### Problem 6
Identifying independent and conditional variables; a preview for what's to come.

### Reflection / Looking Ahead / Implications



