---
title: "Day 1: Welcome! And Statistical Axioms"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day01/#today
  - title: For Next Time
    link: in-class/day01/#for-next-time
  - title: Welcome
    link: in-class/day01/#welcome
  - title: Statistical Axioms
    link: in-class/day01/#statistical-axioms
  - title: Day Activity
    link: in-class/day01/#day-activity
---

## Today
* Welcome to ProbRobo
* ProbStats Axioms (and Robotics)
* Day Activity

## For Next Time
* Complete the day activity for today's topics (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17542)
* Complete YOGA: Initialization assignment (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17537)

## Welcome
Welcome to the first iteration of _Probabilistic Robotics_. This is a class inspired by the broader field of intelligent robotics (often referred to as embodied intelligence in modern writing), and focuses on how robots represent and navigate the messy, complex, and uncertain environments in which they find themselves. This class is intended to be an advanced algorithms course for robotic systems, and an applied introduction to probabilistic and statistical thinking. To further motivate this class, and set some ground rules for this offering, an introductory lecture with notes is provided [here](https://docs.google.com/presentation/d/1_jppam3AdlEDJVs64v4FJkvvowJL6GSP_NeY8L6qt6g/edit?usp=sharing).

## ProbStats Axioms (And Robotics)
Continuing with our [welcome lecture](https://docs.google.com/presentation/d/1_jppam3AdlEDJVs64v4FJkvvowJL6GSP_NeY8L6qt6g/edit?usp=sharing), today we'll be kicking off with a few _probability and statistics axioms_ -- fundamental formalisms, notation, and ideas that will start us off on our journey.

### Motivation for ProbStats in Robotics
At the highest level, let's imagine a robot as a sensory-motor loop.

[Sensory Motor Loop Graphic]

A robot uses sensors to measure the world, and then uses actuators to interact with that world. The "magic" in intelligent robotics is the process by which sensor measurements get turned into useful work. Probability and statistics provide fundamental principles and processes by which robots "make sense" of measurements and translate them into motor commands. 

For instance -- imagine a robot moving around a building. It is equipped with an inertial measurement unit (IMU) which provides streaming measurements of velocity. Handling this streaming data to get trends in the vehicle's motion, to estimate average speed and heading, and so on -- that's statistics. Going a step further and understanding that these measurements are noisy and imperfect, and thus we label how trustworthy we think those averages are -- that's probability.

Probability and statistics work very closely together in the field of robotics, especially as tasks for robots becoming increasingly complex compared to the sensing information a robot might have available to it to accomplish that tasks. For instance, let's take that same robot from earlier and give it a camera. We ask the robot to navigate to the mailroom on Olin's campus. Given IMU and visual information (pictures) alone, how does the robot conceptualize what a "mailroom" is? How does it plan a trajectory through the space it is in, given that it's never mapped the campus before? How does it know when it is on the right track? These are the types of questions we will explore through the lens of probstats in this class.

### Sample Spaces, Events, and Partitions
To get started, we need to first understand the elements that make up a probability. Some definitions:
* Sample Space:
* Event:
* Partition:

### Set Theory


### Defining Probability


### Defining Statistics


### Independence and Conditioning

### Going Beyond
To learn more about the topics that we discussed today, you are invited to use our class resources. The following books and chapters are applicable:
* A (Chap A)
* B (Chap B)

## Day Activity
For today's activity, we'll be practicing with and exploring the probstats axioms we just discussed. Please submit a typeset response to all questions provided here. While everyone will turn-in an individual assignment, collaboration is encouraged! Please note down who you worked with during this assignment to practice appropriate attribution and acknowledgement.

### Problem 1
Practice wtih identifying state spaces, events, partitions for a given scenario. 

### Problem 2
Practice with set theory.

### Problem 3
Practice with probability axioms.

### Problem 4
Practice setting probabilities from word problems (set-up for the robot door-opening scenario)

### Problem 5
Practice with statistical axioms.

### Problem 6
Identifying independent and conditional variables.

### Reflection / Looking Ahead / Implications



