---
title: "Day 8: Kalman Filtering III: Further Extensions and Review"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day08/#today
  - title: For Next Time
    link: in-class/day08/#for-next-time
  - title: Kalman and Beyond
    link: in-class/day08/#kalman-and-beyond
  - title: Review of State Estimation
    link: in-class/day08/#state-estimation-review
  - title: Day Activity
    link: in-class/day08/#day-activity
---

## Today
* Kalman and Beyond
* State Estimation Review
* Day Activity

## For Next Time
* Week 5 Day Activities (Next Monday at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18480)
* State Estimation Simulation Assignment (Next Monday at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18433)

## Kalman And Beyond
We have had a close look at the _Linear Kalman Filter_ and the _Extended Kalman Filter_ in the last two classes; techniques which extend the Bayes Filter to continuous-valued systems we would like to estimate the state of from series of observations and actions. But these are not the only techniques in town! This section outlines a list of other state estimation techniques that are common in Bayesian Estimation and Robotics, with a starting resource for learning more. You are welcome to use this list as a jumping off point for your upcoming deep dive projects...

**Unscented Kalman Filter**: another technique for dealing with non-linearity in a state, observation, or action framework. [Roger Labbe's Textbook Chapter](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/10-Unscented-Kalman-Filter.ipynb) or [this tutorial paper](https://groups.seas.harvard.edu/courses/cs281/papers/unscented.pdf) are good places to get started learning this technique.

**Ensemble Kalman Filter**: this is a blend of linear Kalman filtering and sampling based / particle techniques, which allows for very large state-space systems to be estimated from data. [This tutorial paper from UMD](https://www.math.umd.edu/~slud/RITF17/enkf-tutorial.pdf) is a good place to start to learn more about the technique.

**Particle Filter**: this is a sampling-based technique used to convert large continuous systems (which might be nonlinear!) into something relatively easy to perform computation over, using standard Bayesian Estimation formulas. There is a [whole project in CompRobo](https://comprobo25.github.io/in-class/day07) that investigates this technique, and you can read [Roger Labbe's textbook chapter](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/12-Particle-Filters.ipynb) on the same idea to learn more.

**Monte Carlo Sampling**: In Bayesian Estimation, the process of estimating a state from any number of known/unknown modeling functions is often done using sampling-based techniques (as closed-form analytical techniques is often impossible/intractable). One family of techniques, Markov Chain Monte Carlo samplers, is particularly common. Some specific algorithms you could investigate:  
* **Metropolis-Hastings**: A technique for guiding samples based on a history of prior samples in a space. Here is [an implementation guide in Python](https://medium.com/@keruchen/markov-chain-monte-carlo-and-metropolis-hasting-1dbb9c33f40) you can explore.
* **Hamiltonian**: This is also a guided sampling technique, but uses motion physics to select where to sample next, in an effort to more efficiently explore a state space. This [is a nice tutorial explaining and implementing this sampling technique](https://bayesianbrad.github.io/posts/2019_hmc.html) that also links to other useful learning resources; and this [is a detailed lecture](https://faculty.washington.edu/yenchic/19A_stat535/Lec9_HMC.pdf) on the topic for those who want more math!
* **Reversible-Jump**: This is one of the few sampling techniques that explicitly grapples with the idea that even the dimensionality of the state space may be unknown; this technique allows for samples to be drawn across dimensions. This [overview paper](https://www.colorado.edu/amath/sites/default/files/attached-files/rjmcmc.pdf) provides the math and cites the original papers in this space which provide grounding examples.

**Gaussian Processes**: Leaning in on the Gaussian assumption, a Gaussian process is a way of representing a probability distribution over an (unknown) stochastic process. These have seen increasing adoption in the robotics community, particularly for decision-making or adaptive-sampling systems (spoilers: we'll definitely be talking about these in a few weeks). [This is a simple implementation tutorial](https://scikit-learn.org/stable/modules/gaussian_process.html) showing you all the things that a Gaussian process can do, and this is [an influential paper describing how Gaussian processes can be used in robotics](https://arxiv.org/pdf/1502.02860). 

**Factor Graphs**: Inspired by Kalman Filters, factor graphs are an optimization-based graphical framework that can be used for state estimation. You might commonly find these in navigation and SLAM applications in robotics. [This tutorial paper](https://navi.ion.org/content/71/3/navi.653) provides the derivation, and [this textbook](https://www.cs.cmu.edu/~kaess/pub/Dellaert17fnt.pdf) goes into the nitty-gritty details.



## A Review of State Estimation
We have been focused the last several weeks on understanding Bayesian Estimation to infer the state of a robotic system. Along the way, we have touched on a number of topics:

<p align="center">
<img alt="List of key vocabulary from Days 1-7 in ProbRobo." src="../website_graphics/state_estimation_recap.png"/>
</p>

Today, we'll be taking some time to review and reinforce these concepts before we start in on our culminating topic in this module: simultaneous localization and mapping (SLAM). In the rest of this section there are exercises designed to review the course material. See the _Day Activity_ section for specific instructions.

### Exercise: Concept Mapping
Pair up with one or two other students in the class. Using the list of provided terminology, as well as notes from the last seven classes and days activities, generate a concept map that demonstrates the relationships between ideas in the class, and highlights key takeaways / lessons learned. Your concept map can be in any format - virtual, on the whiteboard, with words, with visuals, mixed media - but does need to be recorded in some way to be submitted. As you are conducting this activity, make note of any areas that you feel strong in, areas that might need more practice, or areas where they are remaining questions. Capture your questions in your concept map with annotations, and consider using these questions as a guide for selecting further practice problems to work through.

### Exercise: Filtering and Smoothing

### Exercise: A Simple Bayes Filter

### Exercise: Continuous Random Variables and Covariances

### Exercise: Linear Kalman Filtering

### Exercise: Extended Kalman Filtering


## Day Activity

### Problem 1: Document Your Concept Map(s)
Please document the concept maps you developed in class; note who you worked with, and feel free to further annotate or reflect on your concept map from the activity. 

### Problem 2: Show Your Work
Choose any one of the exercises listed in today's notes to work through in detail, and submit your work. Let us know why you chose that problem, and whether there are any remaining questions or concerns.
