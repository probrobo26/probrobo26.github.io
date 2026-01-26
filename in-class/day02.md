---
title: "Day 2: Independence, Conditional Independence, and Distributions"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day02/#today
  - title: For Next Time
    link: in-class/day02/#for-next-time
  - title: Simulators in Robotics
    link: in-class/day02/#simulators-in-robotics
  - title: Independence, Conditional Independence, and Distributions
    link: in-class/day02/#independence-distributions
---

## Today
* Simulators in Robotics
* Independence, Conditional Independence, and Distributions
* Day Activity

## For Next Time
* Due Today
  * Last Week's Day Activity: [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17542)
  * YOGA: Initialization Assignment: [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17537)
* Due In The Future
  * Complete the day activity for today's topics (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17543)
  * Start to work on the out-of-class simulation assignment (Due Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18319)

## Simulators in Robotics
Last class, we proposed a conceptual and computational framework for modeling a robot. Where there is a model, there is a way to leverage that model into a simulator environment. 

_What makes a simulator useful?_ Or put another way, why is the use of simulation so widespread in robotics? There are a few good reasons; here are some common ones:
* Hardware is expensive! And so is exhaustive sequential testing.
* Evaluation of complex algorithms is easier when you know the actual "ground truth" of your desired output.
* Edge cases abound, but definitionally don't always come up in day-to-day testing; simulators allow for targeted scenario validation
* Debugging

Since simulators serve different purposes, they are created with varying levels of computational complexity -- for instance, a high-fidelity simulator may embed the laws of physics and require a virtual 3D model of the robot system; a low-fidelity simulator may extract away the robot as a point object and just focus on path-planning or trajectory optimization.

**Exercise:** Find 5 unique examples of robotics simulators online (excluding Gazebo), filling in the following table:

| Name of Simulator | Maintainer(s)/Sponsor(s) | Use Case |
| --- | --- | --- |
| _e.g., Gazebo_ | _e.g., Open Robotics_ | _e.g., open source, general use mobile or manipulator-based physics engine for real-time testing_ |

**Exercise Continued:** What are some commonalities or differences among these simulators? Where do each of these simulators fall on the fidelity/computational complexity spectrum?


As a part of this class experience, we'll be building our own robot simulator from scratch since simulators play a huge role in software development, and can be an incredible pedagogical tool for reinforcing the concepts from State-Action-Sense frameworks. We will build on this simulator throughout the class as a test-bed for future day activities, discussions, and your own deep dives (if you so choose).

The bones of our simulator (and your simulation assignment for this week) can be found on Github: [https://github.com/probrobo26/probo_playground/tree/week1](https://github.com/probrobo26/probo_playground/tree/week1).



## Independence, Conditional Independence, and Distributions
On Day 1, we introduced the notion of probability -- a numerical measure of how probable something is to occur. We can evaluate probabilities over events, which are experiments or outcomes contained within a particular sampling/outcome space. 

Some examples of probability questions we might pose could be:
* What is the probability that the warehouse robot is in Aisle 17?
* What is the probability that the robot will encounter a dead-end in the maze it is navigating?
* What is the probability that the robot grabbing for the apple will ultimately drop the apple?

In robotics, it is common that when we want to compute a probabilty of an event, there is some additional _context_ we have. For instance, we might modify the last statements like:
* What is the probability that the warehouse robot is in Aisle 17 **given that** it was just in Aisle 16 and turned right?
* What is the probability that the robot will encounter a dead-end in the maze it is navigating **given** the last three turns it has made?
* What is the probability that the robot grabbing for the apple will ultimately drop the apple **given that** the robot's machine vision localizer has a noise of 15 cm?

When we pose probability questions with context, we are posing _conditional probability_ statements. These notes will provide some notation for conditional probability and probability distributions, and today's Day Assignment will focus on a robotics application of these ideas.

### Independent Events (Revisited)
Last time, the following idea was presented: the probability of independent events if $$A \cap B \neq \{\mathbf{\emptyset}\}$$ is $$\mathcal{P}(A \cap B) = \mathcal{P}(A)\mathcal{P}(B)$$. 

But what is "an independent event" in the first place? And why does it make sense to multiply the individual probabilities together to get their intersection? 

**Independence** is the notion that the outcome of one event/experiment _does not_ influence the outcome of another event/experiment. A classic example would be playing a game with a fair coin and a die -- the outcome of a coin flip has no bearing on the outcome of the die roll. 

Independence should not be confused with mutual exclusion; mutually exclusive events can never happen at the same time within a sampling space, and so the probability of their union is 0 (impossible). 

We now want to reason over why the probability of the intersection of two events is the multiplication of these events. To do this, let's revisit one of the rules of probability we learned last time: the sum of all possible events in a sampling space must be 1. Let's go back to our coin flip and die roll example, and enumerate all possible outcomes of our sampling space:

* Coin Flip: Tails or Heads (1/2 probability of either)
* Die Roll: 1, 2, 3, 4, 5, or 6 (1/6 probability of either)
* All possible outcomes: {Tails, 1}, {Tails, 2}, {Tails, 3}, {Tails, 4}, {Tails, 5}, {Tails, 6}, {Heads, 1}, {Heads, 2}, {Heads, 3}, {Heads, 4}, {Heads, 5}, {Heads, 6}. 

There are 12 possible outcomes in our sampling space, each with an equal probability of 1/12. And indeed, 1/2 * 1/6 = 1/12. 

**Exercise:** What is the probability of drawing a King from a deck of cards, rolling a 4 on a fair die, and flipping tails on a fair coin? Does this probability increase, decrease, or stay the same, assuming you initially drew a Queen and _did not replace that card in the deck_? What aspects of this situation are independent, and which aspects are dependent?

### Conditional Probability and the Law of Total Probability
Let's say we are in the middle of playing our coin-die game. We flip our coin, and know for a fact it is heads. What then is the probability of landing a 5 on our die? Well, for independent events, this is still 1/6th, but we can write out the _conditional statement_ as follows:

$$\mathcal{P}(A = 4 | B = \text{heads})$$

Read as _the probability of rolling a 4 **given** that the coin flip is heads_. Conditioning gives us the ability to provide context to our probability query. It is perhaps most interesting when events are _not_ independent, since conditioning on knowledge would therefore not be trivial.

Using the idea of conditional statements, we can re-write the intersection between events as a conditional statement:

$$\mathcal{P}(A \cap B) = \mathcal{P}(A | B) \mathcal{P}(B)$$

**Exercise:** Let's take a second to inspect this statement. Why is it important to condition the probability of A with the outcome of B when computing intersections? (Hint: you might find drawing a Venn diagram model useful here). 

**The Law of Total Probability** is commonly stated as:

$$\mathcal{P}(A) = \sum_n \mathcal{P}(A|B_n)\mathcal{P}(B_n)$$

This is a very useful rule, because it applies to both independent and dependent events in a sampling space, and captures the notion that the probability of some event A is equivalent to the probability of that event happening under every condition of a predicating event.

**Exercise:** Express the probabilty of drawing a King from a deck of cards, given that the last draw was a Queen: (1) with replacement (independent events) (2) without replacement (dependent events).

### Random Variables and Distributions




### Today's "So What"
With conditional probability, we can express what we think we know based on what we think we have seen or what we think we have done. This effectively allows is to use our Action and Sense definitions to _infer_ our State. This is obviously useful to us, since the robot only ever has access to sensor measurements or its history of commands. 

### Going Beyond
If you want to dive deeper on the topics discussed today, the following are relevant resources:



## Day Activity
We have a lighter activity today, designed mostly to reinforce some of the concepts we've discussed in class and to give you ample outside-of-class time to start the simulator assignment.

### Problem 1: Recap of Today's Notes
Go back through today's written notes on this page and work through each of the exercises / be sure to document your answers to the exercises discussed in class (there should be a total of 3 exercises in today's notes).

### Problem 2: Revisiting our Trash-Sorter
_(Note: This Problem is Inspired by Frank Daellart's Robotics Book, Chapter 2)_ Let's revisit our trash-sorter from last time, and given this robot some capabilities we'll express as conditional statements.

* **Part 1: Sense**
* **Part 2: Act**
* **Part 3: Inferring State**

