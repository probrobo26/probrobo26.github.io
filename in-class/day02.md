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

**Exercise A:** Find 5 unique examples of robotics simulators online (excluding Gazebo), filling in the following table:

| Name of Simulator | Maintainer(s)/Sponsor(s) | Use Case |
| --- | --- | --- |
| _e.g., Gazebo_ | _e.g., Open Robotics_ | _e.g., open source, general use mobile or manipulator-based physics engine for real-time testing_ |

**Exercise A Continued:** What are some commonalities or differences among these simulators? Where do each of these simulators fall on the fidelity/computational complexity spectrum?


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
* What is the probability that the robot grabbing for the apple will ultimately drop the apple **given that** the robot's machine vision localizer has an error of up to 15 cm?

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

**Exercise B:** What is the probability of drawing a King from a deck of cards, rolling a 4 on a fair die, and flipping tails on a fair coin? Does this probability increase, decrease, or stay the same, assuming you initially drew a Queen and _did not replace that card in the deck_? What aspects of this situation are independent, and which aspects are dependent?

### Conditional Probability and the Law of Total Probability
Let's say we are in the middle of playing our coin-die game. We flip our coin, and know for a fact it is heads. What then is the probability of landing a 5 on our die? Well, for independent events, this is still 1/6th, but we can write out the _conditional statement_ as follows:

$$\mathcal{P}(A = 4 | B = \text{heads})$$

Read as _the probability of rolling a 4 **given** that the coin flip is heads_. Conditioning gives us the ability to provide context to our probability query. For a situation in which the events are independent, we find that:

$$\mathcal{P}(A) = \mathcal{P}(A|B)$$


It is perhaps most interesting when events are _not_ independent, since conditioning on knowledge would therefore not be trivial. So let's consider our card drawing scenario. What is the probability of drawing a King, given that the last draw (without replacement) was a Queen?

$$\mathcal{P} (A=\text{King} | B=\text{Queen})$$

Now, what if we want to ask _what is the probability of drawing a Queen AND King, in that order?_. Here is where conditioning gets useful. The probability of drawing a King *given* a prior Queen is independent from drawing a Queen in the first place. So, we can express this more complicated probabilty using our definition for independent events AND a conditional probability:

$$\mathcal{P}(A = \text{King} \cap B = \text{Queen}) = \mathcal{P}(A = \text{King} | B = \text{Queen})\mathcal{P}(B = \text{Queen})$$

This leads us to a simple expression, which you will find over and over in robotics probstats materials:

$$\mathcal{P}(A \cap B) = \mathcal{P}(A | B) \mathcal{P}(B)$$

You might alternatively see this written as a _joint probability_ of the form:

$$\mathcal{P}(A, B) = \mathcal{P}(A | B) \mathcal{P}(B)$$

**Exercise C:** Let's take a second to inspect this statement. Why is it important to condition the probability of A with the outcome of B when computing intersections? (Hint: you might find drawing a Venn diagram model useful here). 

**The Law of Total Probability** is commonly stated as:

$$\mathcal{P}(A) = \sum_n \mathcal{P}(A|B_n)\mathcal{P}(B_n)$$

This statement _marginalizes_ the event B so we can compute the probability over A alone. Another interpretation is that the law of total probability captures the notion that the probability of some event A is equivalent to the probability of that event happening under every condition of a predicating event B. 

**Exercise D:** Express the probabilty of drawing a King from a deck of cards, given that the last draw was a Queen: (1) with replacement (independent events), (2) without replacement (dependent events), (3) as a sequence of events (joint probability).

### Random Variables and Distributions
So far we've been dealing with scenarios in these notes which are based around "fair" or uniform odds amongst outcomes. But what if our outcome space is not uniform? 

Enter the notion of random variables and probability distributions. Let's start with the following idea: an event in a sampling space is a _random variable_, which is a variable which takes on the value of any experiment/sample from a sampling space. The value of the random variable is determined by a _probability distribution_ over that variable, which weights valid outcomes according to some underlying principles of the world. 

For instance, in our coin-die game, there is a uniform distribution over all outcomes -- the probabilty of all outcomes is equal (and are normalized to sum to 1). In an unfair coin-die game where the die is weighted to roll 6 more often, then we no longer have a uniform distribution and instead have a skewed distribution towards outcomes where there die roll is a 6 (where the sum of probabilities of all outcomes still must be 1). 

Distributions can be discrete or continuous. We've been discussing discrete distributions for ease, but we can also represent possible outcomes continuously. For instance, a single sonar range measurement can take on a continuous value, and a probability distribution can be defined to represent the noise added to any individual sensor measurement over a continuous range of values characteristic to the particular sensor.

As we continue through this module, we will revisit the notion of random variables and distributions as we start compuing probabilities over increasingly complex scenarios.


### Today's "So What"
A robot only ever has access to sensor measurements and actuator commands, and must guess the state of the world with that information. **With conditional probability, we can get closer to expressing our belief about the probable state of the world, based on our context (what we think we have seen or what we think we have done)**. This allows us to leverage our Action and Sense definitions to _infer_ our State. 


### Going Beyond
If you want to dive deeper on the topics discussed today, the following are relevant resources:
* Chapter 13 of _Principles of Artificial Intelligence_ by Russell and Norvig for a treatment of probability with respect to intelligent systems.
* Chapters 1-2 of _Statistical Inference_ by Casella and Berger.


## Day Activity
We have a lighter activity today, designed mostly to reinforce some of the concepts we've discussed in class and to give you ample outside-of-class time to start the simulator assignment.

### Problem 1: Recap of Today's Notes
Go back through today's written notes on this page and work through each of the exercises / be sure to document your answers to the exercises discussed in class (there should be a total of 4 exercises in today's notes).

### Problem 2: Revisiting our Trash-Sorter
_(Note: This Problem is Inspired by [Frank Daellart's Robotics Book, Chapter 2](https://www.roboticsbook.org/S20_sorter_intro.html) -- this is suggested reading for this problem and the day activity)_ Let's revisit our salvage-sorter from last time, and give this robot some capabilities we'll express as conditional statements. Note that you might find you'll want to implement a simple script (in python, matlab, or any language of choice) to automate the last part of this problem.

**Problem Set-Up**
Let's imagine a robot tasked with sorting salvage for recycling. It is set up to recieve objects on a conveyor belt and exists as a large manipulator arm with a camera and joint encoders. When a piece of salvage arrives on the conveyor belt, the robot must sort the salvage accordingly into its matching bin: metal, plastic, organic, composite, and toxic. 

On a representative day, the robot encountered the following:
* 100 pieces of metal
* 230 pieces of plastic
* 73 organics
* 20 composites
* 5 toxic materials

The robot's sensor has limited capabilities, and it classifies materials with the following accuracy:
* Metal: classified as metal (80%); classified as composites (20%)
* Plastic: classified as plastic (90%); classified as composites (5%); classified as toxic (5%)
* Organics: classified as organics (60%); classified as toxic (40%)
* Composites: classified as composites (100%)
* Toxic: classified as toxic (100%)

Whenever the robot makes a wrong sorting decision, it costs the salvage company $150 to fix. 

* **Part 1: Sense and Act** Given the set-up for our system, state/compute the following (note: you might find this easiest to express in tables).
  * The probability of encountering any particular category of salvage, e.g., $$\mathcal{P}(Category)$$
  * The probability of each label given each category (e.g., probability that an object is labelled metal when it is metal, labelled metal when it is plastic, etc.), also known as the classification accuracy, e.g., $$\mathcal{P}(Label|Category)$$
  * The probabiilty of a particular type of salvage given a label e.g., $$\mathcal{P}(Category | Label)$$ (Note: this is a tough one! You might want to visit the Robotics Book for more details, or work with friends to justify your approach)
* **Part 2: Evaluation** Given the probabilities that you just computed, what is the _expected_ (estimated) cost to the company for wrong sorts? Where is the biggest room for improvement for our robot?


