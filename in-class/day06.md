---
title: "Day 6: Kalman Filtering I: Overview and Intuition"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day05/#today
  - title: For Next Time
    link: in-class/day05/#for-next-time
  - title: The Linear-Gaussian Approximation
    link: in-class/day05/#linear-gaussian
  - title: Kalman Filters
    link: in-class/day05/#kalman-filters
  - title: Day Activity
    link: in-class/day05/#day-activity
---

## Today
* The Linear-Gaussian Approximation
* The Basic Kalman Filter
* Day Activity

## For Next Time
* Due Today:
    * The Week 3 Day Activities [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18359)
* Due in the Future
    * Week 4 Day Activities (Next _Tuesday_ at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18432)
    * State Estimation Simulation Assignment (Monday 23rd at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18433)



## Limitations of the Simple Bayes Filter
We have talked at length about the Bayes Filter; first understanding Bayes Rule, operationalizing it, and then connecting it to simple state estimation problems. So far, the worlds we've described have generally been:

* Discrete and countable (all outcomes have been possible to enumerate)
* Actions are singular and apply directly to the state
* Measurements are singular and apply directly to the state

But, what of more complex worlds, instances of multiple sensors, control systems that don't directly map to changes in the state space? 

If we wanted to apply a Bayes Filter directly in these cases, we would find that it is technically correct, but computationally challenging or even intractable. As was mentioned in the [day 5 notes][./day5.md], to address this issue, there are a menu of _approximations_ that can be made that allow us to make small adjustments to our Bayes filter that can deal with more complex scenarios.

We will be spending the next several classes discussing _Kalman Filtering_, perhaps one of the most essential Bayesian filters used in modern robotics today, and which hinges on a series of simple approximations that we will analyze. A roadmap for our discussions:

* Today: Overview and Intuition for the Standard Filter
* Thursday 12th: Extended Kalman Filtering and Linearization
* Monday 16th: _No Class, President's Day_
* Thursday 19th: Further Extensions, Assumptions, and Discussion

## The Linear-Gaussian Approximation
To transform our simple Bayes Filter into something that can handle more complex problems, we will apply two key assumptions:

### Linearity
We assume that the relationship between states and observations is linear, the relationship between actions and states is linear, and the relationship between the previous state and the next state is linear. 

Linearity means that there is a _direct_ relationship between these state spaces:

* Example: A GPS measurement is directly related to position of a robot outdoors
* Example: Wheel velocity commands are directly related to distance traveled by that wheel
* Example: The instantaneous position of a manipulator arm is linearly related (through velocity) to its previous position

By assuming linearity, our lives are much easier, because we can use _linear algebra_ to then encode the relationships between our states, observations, and actions in practice. It is truly that simple.

### Gaussian / "Normalness"
We assume that all random variables (beliefs) are drawn from multivariate normal (Gaussian) distributions, with the probability density function:

$$\mathcal{P}(x) = \text{det}(2\pi \Sigma)^{1/2} \exp(\frac{-1}{2}(x - \mu)^T \Sigma^{-1}(x-\mu))$$

where $$\mu$$ is the mean of the distribution, and $$\Sigma$$ is the covariance.

Gaussians are convenient for their closed-form expressions, their unimodality (there will be a single "best estimate" at every timestep), and their relatively easy/flexible definition.


### Properties of Linear-Gaussian Systems
The combination of linearity and Gaussian assumptions defines a class of approximators as _linear-Gaussian systems_. This class of approximator has the following useful properties:

* The _joint distribution_ of variables with linear-Gaussian properties is also Gaussian
* The _posterior distribution_ of linear-Gaussian variables is also Gaussian
* Finding the posterior distribution of linear-Gaussian variables is $$\mathcal{O}(n^3)$$ where n is the number of states

You may see the first two properties in texts stated as: "the linear-Gaussian assumption remains closed under Bayesian inference"; this just means that the Gaussian-ness of the variables is preserved. This is great news for us in our recursive framework for Bayes Filtering, as this means we can continue to iterate on a posterior distribution as actions and observations are collected without any need to modify the form of our estimate or estimation process.



## The Kalman Filter
By applying a linear-Gaussian assumption to the states, actions, and observations of our estimation problem, we can transform our simple Bayes filter into a _Kalman filter_ which can take advantage of the linear-Gaussian properties. 

The Kalman filter, invented by Thorvald Thiele, Peter Swerling, and Rudolph Kalman in the late 1950s/early 1960s, is a _continuous space_ Bayesian filter for linear-Gaussian systems. The Kalman filter yields a state update and corresponding estimate of covariance (we can think of this as yielding a state estimate and a measure of _uncertainty_ over that estimate).

The Kalman Filter is typically implemented in two steps, a _prediction_ step (motion model!) and an _update_ step (observation model / likelihood!). This follows directly from our definition of a Bayes filter.

### Prediction
In the first step, we want to simply compute the estimate of the state given our control inputs and transformation behavior of the system, and its corresponding covariance. For the Kalman filter, this is written simply as:

$$
\text{Kalman State Prediction: } x_t = F_t x_{t-1} + B_t u_t + w_t
$$

$$
\text{Kalman Covariance Prediction: } P_t = F_t P_{t-1} F_t^T + Q_t
$$

where:
* $x$ is our state vector
* $u$ is our input control vector
* $F$ is our transformation / transition matrix which maps from the previous state to the current state
* $B$ is our control matrix which maps from the control input to the state space
* $w$ is process noise, which is drawn from a Gaussian distribution with covariance $Q_t$
* $P$ is the process covariance
* $t$ is an index indicating time step

The update step showcases the advantage of the linear-Gaussian assumption clearly. We are able to compute the distribution over multiple state variables _completely_ in a set of only two linear algebraic expressions.

### Update
Of course, the world doesn't always behave perfectly according to our transition and control model, so we would like to update our posterior estimate of state and state covariance from observations. 

For this, we will start with computing the _residual_ which is the difference between an actual measurement we took in the world, and the prediction of what that measurement ought to have been according to our state estimate:

$$\text{Kalman Update Residual: } y_t = z_t - H_t \hat{x_t}$$

where:
* $y_t$ is our residual data vector
* $z_t$ is our actual measurement
* $H_t$ is our measurement model matrix
* $\hat{x_t}$ is our _preficted_ state, following from the prediction step

Similarly, we can compute the _innovation_ (measurement residual) on the covariance as well:

$$\text{Kalman Update Covariance Innovation: } S_t = H_t \hat{P_t} H_t^T + R_t$$

where:
* $S_t$ is the innovation on covariance
* $\hat{P_t}$ is the _predicted_ process covariance, following from the prediction step
* $R_t$ is the measurement noise covariance

When computing the residuals, we are effectively computing an "error" term in state and an "uncertainty propogation" term in covariance. 

Ultimately, we want to use these residuals to correct our state estimate, essentially tempering our prediction with a dose of reality from our observations. To do this, we will make use of the "secret sauce" of a Kalman filter -- the Kalman Gain:

$$\text{Kalman Gain: } K_t = \hat{P_t} H_t^T S_t^{-1}$$

and apply it as:

$$\text{Posterior Kalman State Estimate: } x_t = \hat{x_t} + K_t y_t$$

$$\text{Posterior Kalman Process Covariance: } P_t = (I - K_t H_t)\hat{P_t}$$

We can think of the Kalman gain as a principled "tuning" parameter for our update step; it modifies how much we trust our residual statement (since after all, measurement models can be wrong too!). It is directly derived from an optimization problem that seeks to minimize the mean square error between the true state and the predicted state estimate.  

For completeness, you may see the final posterior estimates for the Kalman filter written in the following forms (for either ease of interpretation or numerical stability):

$$\text{Posterior Kalman State Estimate (Equivalent Form): } x_t = (I - K_t H_t) \hat{x_t} + K_t z_t$$

$$\text{Posterior Kalman Process Covariance (Joseph Form): } P_t = (I - K_t H_t)\hat{P_t}(I - K_t H_t)^T + K_t R_t K_t^T$$

## Kalman Filter Principles
The Kalman filter provides an _optimal_ state estimation whenever the model matches the real system, noise in the system is uncorrelated, and the covariance matrices for noise are known perfectly. Perhaps this is a tall order, so it is useful to know how violations in this set-up impact filter performance.

Kalman filtering is popular in robotics (especially for localization problems) for a reason -- it is empirically pretty good at the task. More formally, we can say the following:

* The prediction step will always _accumulate_ uncertainty if unchecked by the update step
* The update step will typically _reduce_ uncertainty, unless the Kalman Gain is very, very small (which can happen if the measurement noise is very large)

Given this set-up, when a system being modeled violates the key assumptions of the Kalman Filter, yet the filter is applied anyway, the Kalman Filter will typically yield accumulating uncertainty per the process covariance model of the system. This may still be an overly confident estimate of state, depending on the nature of the violations, but this guarantee is useful for testing the quality of the filter for a given task.

Another way to test the quality of the Kalman filter is to plot the prediction error (the residual) over time, and check that it is uncorrelated noise -- if so, the filter is working as well as it possibly could under stochastic conditions. If there are trends in the residual, then it might indicate import unmodeled dynamics.


**Exercise**: Read Roger Labbe's [_Kalman and Bayesian Filters in Python_ Chapter 6](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/06-Multivariate-Kalman-Filters.ipynb) entry (feel free to pull and run his notebook for this! you do _not_ need to do the embedded exercises in the notebook) and answer the following questions:
* Summarize the "Tracking a Dog" model, using the notation in these course notes. Describe and defend the design decisions made in the formulation of the model.
* Interpret the figure produced by cell block 18 (a plot of the covariance ellipses before and after the prediction step). Given that no process noise was added, what causes the change in the covariance shape? 
* Interpret the three plots produced by cell block 31 (the track of the dog, and the variances of position and velocity). What does it mean for the variances to decrease with each time step? How does filtering performance appear to change with time for position estimate? Is this expected?
* In the _Adjusting the Filter_ section, several examples showing how $$Q$$ and $$R$$ act as controls for the filter are shown. Summarize the impact that each of these matrices has on the output of the filter (both state estimate and covariance), and suggest some ways that you might think about setting these matrices in the real world (feel free to look at external references for ideas on this one!).
* All models are wrong, some are useful. Describe what an engineer needs to investigate or consider when designing a Kalman filter for a task. Be thorough -- how does each piece of the model influence potential outputs, and what are the implications (big or small) of poor definition for any one of these pieces?


## Going Further
There are many excellent resources to support your learning and understanding of the Kalman Filter. Here are several we recommend:
* Chapter 4.3 Kalman Filter in [_Bayesian Smoothing and Filtering_](https://users.aalto.fi/~ssarkka/pub/cup_book_online_20131111.pdf)
* Chapter 3.1 Gaussian filters in [_Probabilistic Robotics_](https://share.libbyapp.com/title/5578912#library-minuteman-olin)
* Chapter 15 and Chapter 25 of _Artificial Intelligence: A Modern Approach_ (find a physical copy of this in the Olin library!)
* Roger Labbe's [Book on Kalman and Bayesian filtering in Python](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/tree/master) (Note: you might find this a very useful reference for your simulation assignment!)
    * [Chapter 7 of the same book](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/07-Kalman-Filter-Math.ipynb) provides further mathematical detail to complement today's notes


## Day Activity
Today's day activity is designed to give you ample time to get started on the [simulation assignment](https://canvas.olin.edu/courses/1002/assignments/18433) which aims to complement the mathematical discussions in class with practical implementation.

### Problem 1: Recap of Today's Notes
Go back through today's written notes on this page and work through each of the exercises / be sure to document your answers to the exercises discussed in class (there should be a total of 1 exercise in today's notes).

### Work On The Simulation Assignment
Nothing to turn in for day-activity work here; just get started with the simulation assignment!
