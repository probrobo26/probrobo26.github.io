---
title: "Day 5: Bayesian Prediction, Smoothing, and Filtering"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day05/#today
  - title: For Next Time
    link: in-class/day05/#for-next-time
  - title: Bayes Filters
    link: in-class/day05/#bayes-filters
  - title: Prediction, Smoothing, and Filtering
    link: in-class/day05/#prediction-smoothing-filtering
  - title: Gaussian Approximation
    link: in-class/day05/#gaussian-approximation
  - title: Day Activity
    link: in-class/day05/#day-activity
---

## Today
* Bayesian Estimation Continued: Bayes Filters
* Prediction, Smoothing, and Filtering
* The Gaussian Approximation
* Day Activity

## For Next Time
* Complete the day activity for today's topics (Due next Monday at 7PM): [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18359)


## Bayesian Estimation, Continued: Bayes Filters
In our door-opening robot example last week, we started to operationalize the standard form of Bayes Rule in order to estimate the state of the world (whether the door was open or closed) given a chain of robot actions and measurements. 

As a refresher, the generic form of Bayes Rule is:
<p align="center">
<img alt="Bayes Rule, with different components of the formula labelled." src="../website_graphics/bayes_rule.png"/>
</p>

And when we have multiple pieces of information (such as actions AND observations), we can write the same rule in the form:

$$\mathcal{P}(x_t \vert z_t, u_t) = \frac{\mathcal{P}(z_t \vert x_t, u_t)\mathcal{P}(x_t | u_t)}{\mathcal{P}(z_t | u_t)}$$

where $$x_t$$ is some state of the world (that we would like to infer), $$u_t$$ is an action that the robot takes, and $$z_t$$ is some observation that the robot measures. 

When we operationalize an equation, we're implementing an algorithm. We can write the generic algorithm that describes a _Bayes Filter_ then as follows:

> Algorithm 1: A Bayes Filter
>> bayes_filter($$bel(x_{t-1}), u_t, z_t$$)
>>> for all $$x_t$$ do
>>>> $$\overline{bel(x_t)} = \int \mathcal{P}(x_t \vert u_t, x_{t-1})bel(x_{t-1})dx_{t-1}$$
>>>> $$bel(x_t) = \mu \mathcal{P}(z_t \vert x_t) \overline{bel(x_t)}$$

>>> end for

>>> return $$bel(x_t)$$


where $$bel(\cdot)$$ represents the _belief_ that the robot holds (which isn't necessarily a proper probability, as we discussed last week).

As we observed in the door-opening robot example, Bayes Rule can be used _recursively_ , where the posterior at t=1 can become the prior for t=2 (and so on). In a recursive algorithm, the `bayes_filter` function would be nested in a for-loop, with the previous belief and latest action/measurements supplied at each iteration.

**Exercise:** (Problem inspired by Chapter 2.8 Exercise 1 from the _Probabilistic Robotics_ textbook) Let's say we have a robot with a scalar range sensor with bounds from 0 to 3 meters, with actual ranges in the world distributed uniformly in this interval. This sensor is imperfect, and when it faults, it consistently outputs a range reading below 1 meter, regardless of the true range in the world. The prior probability that the sensor is faulty is $$\mathcal{P}(x_0 = \text{faulty}) = 0.015$$. The robot queries its sensor $$N$$ times, and every measurement is below 1 meter. What is the posterior probability that the sensor is faulting, for $$N = 1, 2, ..., 10$$? Formulate the corresponding probabilistic model and solve using an implemented Bayes Filter.  

### Assumptions and Considerations for Bayes Filters
While Bayes Filters are a very useful tool, they come with some baggage that is worth knowing about.

**Markov Assumption** As we discussed last week, one key assumption of a Bayes Filter is that the world is _Markovian_. This assumption posits that past and future observations/data are independent of one another, given the current state of the world. While this allows for some computational niceties, the real world often violates this assumption:
  * When there are un-modeled, time-varying dynamics (e.g., dynamic obstacles)
  * Inaccurate probability models
  * Approximation errors for the belief
  * Changes to the action or observation model

Despite the ease with which this assumption is violated; Bayes Filters remain the backbone of most modern estimation systems as they have been shown to be empirically robust to most of these violations. Whenever that has failed to be the case, that's when you might see _learned_ approximators, which attempt to capture the un-modeled, nonlinear, or otherwise complex state dynamics.

**Uninformed/Noninformative Prior** Setting the initial prior for the belief of the world state is important, as it will provide an initial bias for the filter. In practice, it can be hard to access this prior probability, and so a common assumption is to use the _uninformed_ prior, which sets a uniform distribution over all possible outcomes. This is the most conservative assumption to make, and the only down-side is that for very complex systems it may take time for the filter to converge (whereas the number of recursions is often reduced if you make a good first guess at the probability).

**Computational Time** Implementing a naive Bayes filter can be exponential in the dimension of the state space. Some approximations for the belief distribution (spoiler: we'll be utilizing these in the Kalman Filter) can improve this to polynomial time. But this still isn't great. To improve on time, the Bayes Filter needs to be fully-reimplemented; one type of Bayes Filter, the Particle Filter, is an _any-time_ algorithm, which means it will yield an answer at any point that it is interrupted. 

**Accuracy** Whether an underlying distribution is unimodal or multimodal can make a difference for how quickly and accurately a Bayes Filter can converge on an answer. Since a Bayes Filter is also a system-level approximation operating over numerical approximations, the accuracy of a Bayes Filter should be skeptically considered. However, unless there are many, many un-modeled features and dynamics in a state-space, the Bayes Filter will provide a reasonable estimate and uncertainty bounds over that estimate, providing useful distinguishing signal.


## Prediction, Smoothing, and Filtering with Bayesian Methods
In signal processing, _prediction, smoothing, and filtering_ are common utilities often discussed together for time-varying systems. We can define these processes under our inference and state estimation framework. 

**Prediction** is the ability to estimate state $$x_t$$ given only the history from $$x_{t-1}$$. Note that this is different from direct filtering, as the latest observation or action is _unknown_ at the prediction step. We can note this as $$\mathcal{P}(x_{k+n} \vert x_{1:k}, z_{1:k}, u_{1:k})$$.

**Smoothing** is the ability to estimate the state $$x_t$$ given a range of historical and future state estimates and associated actions and observations. Smoothing benefits from a notion of _hindsight_. We can note this as $$\mathcal{P}(x_{k} \vert x_{1:N}, z_{1:N}, u_{1:N}), k \leq N$$.

**Filtering** is the ability to estimate state $$x_t$$ given the history of $$x_{t-1}$$ and the latest observation $$z_t$$ and action $$u_t$$. We've been talking about filtering quite a lot already! We can note this as $$\mathcal{P}(x_{k} \vert x_{1:k-1}, z_{1:k}, u_{1:k})$$.


### Bayesian Filtering
We've already gone ahead and derived this earlier in the notes, but to restate, the Bayesian filter estimates the current state from the most recent state and observation/action. 

Mathematically, we want to find the probability of any state $$s \in \mathbf{X}$$ at time $$k$$. We can express this as:

<!-- $$\mathcal{P}(x_k = s \vert z_{1:k}, u_{1:k}) = \frac{\mathcal{P}(x_k = s, z_{1:k} \vert u_{1:k})}{\mathcal{P}(z_{1:k} \vert u_{1:k})}$$ -->


### Bayesian Prediction
Prediction asks us to estimate a future state of the world, to which we do not have a complete historical record (thus we have to make naive updates). 

Mathematically, we want to find the probability of any state $$s \in \mathbf{X}$$ at time $$k > N$$ where $$N$$ is time at which we stop receiving measurement or action updates. 

To perform prediction, we can first use a Bayesian Filter to compute $$\mathcal{P}(x_N = s \vert z_{1:N}, u_{1:N})$$ to the point at which the observations run out.

From there, a naive update is performed using only the transition model, for $$k - N$$ steps:

<!-- $$\mathcal{P}(x_{N+1} = s_j \vert x_{N} = s_i)$$ -->



### Bayesian Smoothers
Smoothing allows us to use future information to infer the state of the world at some particular time. If we think about filtering as a "forward step", smoothing adds a "backwards step" so that future states and observation sequences can be considered. 

Mathematically, we want to find the probability of any state $$s \in \mathbf{X}$$ at time $$k < N$$ where $$N$$ is the maximum range of all known observations. We can express this as:

$$\mathcal{P}(x_k = s \vert z_{1:N}, u_{1:N}) = \frac{\mathcal{P}(x_k = s, z_{1:N} \vert u_{1:N})}{\mathcal{P}(z_{1:N}\vert u_{1:N})}$$

$$ = \frac{\mathcal{P}(x_k = s, z_{1:k}, z_{k+1:N} \vert u_{1:k}, u_{k+1:N})}{\mathcal{P}(z_{1:N}\vert u_{1:N})}$$

$$ = \frac{\mathcal{P}(z_{k+1:N} \vert x_k, z_{1:k}, u_{1:k}, u_{k+1:N}) \mathcal{P}(x_k = s, z_{1:k} \vert u_{1:k}, u_{k+1:N})}{\mathcal{P}(z_{1:N}\vert u_{1:N})}$$

$$ = \frac{\mathcal{P}(z_{k+1:N} \vert x_k = s, u_{k+1:N}) \mathcal{P}(x_k = s, z_{1:k} \vert u_{1:k})}{\mathcal{P}(z_{1:N}\vert u_{1:N})}$$

In the final derived expression here, the first term in the numerator encodes the information of future measurements, and we can think of this as the "backwards" look in our smoother (very literally, this term computes the probability of future observations given that the current state estimate is what we think it is). The second term should look familiar -- this is just forward filtering (un-normalized)! 

Note that computing the backward step can be a little tricky. This is going to end up being a recursive trick, where you will want to start at the last possible timestep and assume a probability of 1 for any of the last states, and then repeat the following:

<!-- $$\beta_k(s \in \mathbf{X}) = \sum_{q \in \mathbf{X}}\beta_{k+1}(q)\mathcal{P}(x_k = s \vert x_{k+1} = q)\mathcal{P}(z_{k+1} \vert x_{k+1} = q)$$ -->

where $$\beta(\cdot)$$ is a function representing the cumulative backwards step.

**Exercise:** (Problem inspired by the "whack-a-mole" problem in MIT's _Principles of Autonomy_ Lecture 20 notes) Two robots are playing tag in a three-room space. The "it" robot would like to estimate where the other robot will be to tag them. The robot that is being chased has some probability of moving between the rooms associated with the room it was previously in (represented in the table). We know for a fact that the robot being chased started in room 1, $$\mathcal{P}(x_1 = 1) = 1$$, since the game always initializes there. 


<center>

|||||    
| --- | --- | --- | --- |
| Robot is in  &#8594;      | Room 1 | Room 2 | Room 3 |
| Robot transitions to &#8595; | | | |
| Room 1| 0.0   | 0.0    | 0.0     |
| Room 2| 0.5   | 0.8    | 0.3     |
| Room 3| 0.5   | 0.2    | 0.7     |

</center>


We can use Bayesian prediction, filtering, and smoothing to answer the following questions about our scenario:

* In one version of the game, the tagged robot shuts down for a few seconds to give the other robot a chance to run away. Our "it" robot just wakes up after some set time, and would like to estimate where the other robot is in the world. After one world timestep, what is the _probability distribution_ over where the other robot is? Over two timesteps? Continue computing a prediction further into the future -- what do you notice about the distribution? 

* In another version of the game, there is no shutdown period, but our "it" robot can only take noisy measurements (model in the table below) of where the other robot is according to a measurement model. While our "it" robot is still certain that the other robot started in Room 1, over the next 5 timesteps it observes the other robot in {Room 2, Room 3, Room 3, Room 2, Room 3}. Using filtering, what is the point-wise most likely trajectory of the chased robot? Using smoothing, what is the most likely trajectory of the chased robot?


<center>

|     |      | | |    
| --- | --- | --- | --- |
| Sensor reads  &#8594;      | Room 1 | Room 2 | Room 3 |
| Actual location &#8595; | | | |
| Room 1| 0   | 0.5    | 0.5     |
| Room 2| 0   | 0.9    | 0.1     |
| Room 3| 0   | 0.1    | 0.9     |

</center>


Hint: You might find it useful to think about populating several tables that keep track of timesteps and probabilities for each step, such as:


<center>

| Timestep | Room 1 | Room 2 | Room 3 | 
| --- | --- | --- | --- |
| 1 | 1 | 0 | 0 |
| 2 | ... | ... | ...|

</center>


Hint 2: Make sure to keep track of your _unnormalized_ forward and backward step values, especially useful when performing smoothing. Your final answer for your filtered output should be a table with normalized probabilities, but if you use these directly in your smoothing step you're going to have a bad time! 

## The Gaussian Approximation
In general, straight-up Bayes filters (and smoothers!) are not tractable to compute directly for large discrete spaces or continuous domains. It's also the case that in real scenarios we also don't typically have access to the real transition/action models and measurement models, or the full extent of the state space. So, we need to _approximate_ these things in a principled way. 

To do this, we want to pick an approximation scheme that is relatively flexible for large ranges of domains, analytically well-defined, and ideally something easy to draw samples from. Enter, the _Gaussian_ family of approximators -- one of the most popular approximation schemes in robotics for utilizing Bayesian estimation.

The Gaussian approximation assumes that all beliefs can be represented as normal distributions, that is:

$$\mathcal{P}(x) = \text{det}(2\pi \Sigma)^{1/2} \exp(\frac{-1}{2}(x - \mu)^T \Sigma^{-1}(x-\mu))$$

where $$\mu$$ is the mean of the distribution, and $$\Sigma$$ is the covariance (we're clearly assuming here that the state space $x$ is multivariate).

### Implications
So, what makes this family of approximators so popular? 

First, the normal distribution is relatively easy to define -- just a mean and a covariance -- which as we saw last class, can potentially be computed trivially from recorded data, or might be intuitive to estimate under some circumstances.

Second, it comes with computational benefits -- this is a well-defined function, with easy-to-compute sample draws and a closed-form equation for performing prediction and correction. 

Third, it is unimodal -- for tracking or localization problems, it is nice to ensure that there is going to be a single "best" estimate that is made.

But here is also the rub; this approximation scheme is limited because it is unimodal. There are lots of scenarios in which different hypotheses may have unique or multimodal distributions. 

Moreover, not everything in the world falls on a normal distribution, and poorly matched distributions between the real world and this approximation can mean significantly longer convergence times when performing state estimation (i.e., more observations are needed to estimate the state than would otherwise be needed).

We'll be spending the next few classes deep-diving into a specific instance of Gaussian Bayesian filters: the _Kalman filter_ and its extensions (which attempt to overcome some pitfalls of this approximator).


## Today's So What
State estimation in robotics is ultimately a game of data handling -- from streaming measurements and control signals, something from the world needs to be elucidated. **Inference frameworks, like Bayesian Estimation, provide a data-handling technique that provides us with these state estimates _and_ a notion of how certain we are about those estimates.** This is unique from classical signal processing, which focuses on outlier rejection, pattern recognition, or regressive lines of fit with statistical measures of fit.

Prediction, Filtering, and Smoothing are common data handling functions that can be expressed probabilistically and can be leveraged by a robot in decision-making and state estimation: prediction can help select a new place to go or action to take based on the expected state of the world; filtering can provide a real-time estimate of the true state of the world; and smoothing can provide a retrospective on the history of the world to possibly improve future performance.

### Going Further
We only touched the surface on these topics today. In fact, there is a whole textbook dedicated to this topic that you might find interesting! Relevant resources:

* [_Bayesian Filtering and Smoothing_](https://users.aalto.fi/~ssarkka/pub/cup_book_online_20131111.pdf) textbook, especially Chapters 1, 4, and 8
* Chapter 1, Sections 2 and 3 of _Probabilistic Robotics_
* _Principles of Autonomy_ MIT [Course Notes, Lecture 20](https://ocw.mit.edu/courses/16-410-principles-of-autonomy-and-decision-making-fall-2010/resources/mit16_410f10_lec20/)


## Day Activity
Today's activity focuses on practicing with the concepts of prediction, smoothing, and filtering.

### Problem 1: Recap of Today's Notes
Go back through today's written notes on this page and work through each of the exercises / be sure to document your answers to the exercises discussed in class (there should be a total of 2 exercises in today's notes).

### Problem 2: State Estimation for our Door Opening Robot
Let's revisit our door-opening robot, and apply some of the principles of prediction, smoothing, and filtering:

* **Part A: Prediction** Our door-opening robot is thinking about making the following actions: {Open, Nothing, Nothing, Open, Open}. There was a 50/50 shot of the door being open or closed at the beginning of the sequence. What is the probability distribution over the state of the door after this sequence? What is the most likely state of the door?

* **Part B: Filtering** Our door-opening robot goes ahead and starts to execute the series of actions in Part A. As it executes those actions, it makes the following observations: {Closed, Closed, Open, Closed, Open}. What is the real-time belief that the robot holds about the door state as it executes each action and makes each observation?

* **Part C: Smoothing** The door-opening robot is now finished its actions and observations, and would like to estimate the most likely _trajectory_ (state sequence) of the door with the benefit of hindsight. What is the distribution over this trajectory, and the resulting most-likely trajectory?

### Problem 3: A Simple HVAC System
(Problem inspired by Exercise 3, Section 2.8 in _Probabilistic Robotics_) Indoor comfort is impacted by outdoor weather conditions -- sunny days can cause the greenhouse warming effect, cloudy days can keep things chilly, rainy days modulate the humidity, and so on. You're tasked with building a very simple HVAC system that monitors the weather and adjusts its controls accordingly to maintain comfortable indoor set points. Since this is a prototype, the initial sensor you get to measure the weather is pretty cost-efficient (aka noisy), so for more stable control, you decide to implement a state estimator for the weather given your sensor observations.

You pull up historical weather trends in your area, and the transition pattern from day-to-day can be modeled as follows:


<center>

|||||    
| --- | --- | --- | --- |
| Tomorrow will be &#8594;| Sunny | Cloudy | Rainy |
| Today is &#8595;||||
| Sunny | 0.8   | 0.2    | 0     |
| Cloudy| 0.4   | 0.4    | 0.2     |
| Rainy | 0.2   | 0.6    | 0.2     |

</center>


Then through experimentation, you find that your sensor has the following characteristics:


<center>

|||||    
| --- | --- | --- | --- |
| Sensor reads &#8594;| Sunny | Cloudy | Rainy |
| Actual weather &#8595;||||
| Sunny | 0.6   | 0.4    | 0     |
| Cloudy| 0.3   | 0.7    | 0     |
| Rainy | 0     | 0      | 1     |

</center>


* **Part A** While predicting the weather is always fraught, let's say that you know for a fact that today (day 1) is sunny. What is the weather going to be on day 5?

* **Part B** Your system is now in use, and you've got your sensor integrated. Today (day 1) it was rainy. In the next several days, your system observes {cloudy, cloudy, rainy, sunny}. What is the probability that the last day (day 5) it was actually sunny?

* **Part C** What was the most likely weather on each of days 2-4, using the observations from Part B?

* **Part D** Given what you're observing about your state estimation capabilities, how would you go about evaluating your prototype HVAC system? What caveats of your empirical performance would you need to communicate to a possible stakeholder?

