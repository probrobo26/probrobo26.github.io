---
title: "Day 6: Kalman Filtering I: Overview and Intuition"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day07/#today
  - title: For Next Time
    link: in-class/day07/#for-next-time
  - title: Filtering for Nonlinear Systems
    link: in-class/day07/#nonlinear-systems
  - title: Linearization
    link: in-class/day07/#linearization
  - title: Extended Kalman Filters
    link: in-class/day07/#extended-kalman-filters
  - title: Day Activity
    link: in-class/day07/#day-activity
---

## Today
* Filtering Nonlinear Systems
* System Linearization 
* Extended Kalman Filters
* Day Activity

## For Next Time
* Week 4 Day Activities (Next _Tuesday_ at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18432)
* State Estimation Simulation Assignment (Monday 23rd at 7PM) [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/18433)

## Filtering for Nonlinear Systems
Last time we had a look at the Kalman Filter, a method for performing a Bayesian update over continuous state variables under a linear-Gaussian approximation. There is a natural question that follows when learning about the Kalman filter: what about systems that aren't linear? 

The linearity assumption in the standard Kalman Filter dictates that the relationship between observations, actions, and states is _direct_. This is ultimately what allows relatively simple linear matrix operations to be used in the filter itself to estimate the posterior. 

But there are many instances when using robotic systems when we don't have such clean relationships between our state spaces:
* Imagine a landmark sensor that is available on a robot moving around a space. It provides metric information about the robot's surroundings, which is sufficient for navigation. However, coordinates of landmarks and the position of the robot are _not_ directly related
* Imagine a super fast flying drone, wherein the motion model needs to incorporate a notion of air density and drag; this motion model is a nonlinear differential equation and thus the state from one step to a next is _not_ directly related

Unfortunately, directly dealing with nonlinearity is _hard_. If we were to relax our linearity requirement in the Kalman filter, then we lose the ability to propagate our uncertainty under a closed form (a linear transformation of a Gaussian is a Gaussian, but a nonlinear transformation of a Gaussian is...not). We could just formulate a new type of filter entirely, but at the end of that day, the computational complexity would be enormous.

We want to keep using the nice parts of our Kalman Filter, and be able to handle nonlinear systems. And so our compromise is that we must _approximate_ our nonlinear system as a linear one, then apply our standard Kalman Filter on the _linearized_ system. This specific design choice is known as the _Extended Kalman filter_ (EKF) and is among the most common techniques utilized in the robotics industry to deal with nonlinear systems in complex localization, mapping, and control tasks.
 
## Linearization for Robot Models
You may have encountered linearization in previous classes (the QEAs, ESA, Controls...). To review in a general case, imagine we have some nonlinear function:

$$f(x) = x^3 - 2x^2$$

To "linearize" this function means to find the line which most closely matches the curve at some defined point along that curve. For instance, at $$x = 2$$, we would want to find the line that is _tangent_ to the the curve at that point. For some functions, this may be as simple as taking the derivative of the function, but a more general approach is to use the equation for a tangent line using the [Taylor Series Expansion](https://en.wikipedia.org/wiki/Taylor_series):

$$
T(x) = f(\hat{x}) + \frac{df}{dx}(\hat{x}x)(x-\hat{x})
$$

where $$\hat{x}$$ is the query point at which the tangent line is to be approximated. For our original setup, the linearization of our function at $$x = 2$$ would be:

$$
T(x) = f(2) + \frac{df}{dx}(2)(x-2)
$$

$$
T(x) = 0 + (3(2)^2 - 4(2))(x-2)
$$

$$
T(x) = 4x - 8
$$

<p align="center">
<img alt="Demonstration of a nonlinear function linearized at a specific query point." src="../website_graphics/linearization_example.png"/>
</p>

### The Jacobian
In our Kalman filter, recall that we can approximate the state space and observations with the following (linear!) expressions:

$$
x_t = F_t x_t + B_t u_t
$$

$$
z_t = H_t x_t 
$$

In our nonlinear system, we replace the $$F$$ and $$B$$ matrices in state with a nonlinear equation $$f(x_t, u_t)$$, and in our observation space, we replace out measurement matrix $$H$$ with a nonlinear function $$h(x_t)$$:

$$
x_t = f(x_t, u_t) + w_t
$$

$$
z_t = h(x_t)
$$

To linearize these equations means to get back out a transition matrix $$F$$ and measurement matrix $$H$$ at specific query points of $$x_t$$ and $$u_t$$. Thus, we need to linearize these equations with respect to different reference points. To do this, we will use partial derivatives with respect to the input variables and store these in a matrix. You might know this as computing the _Jacobian_:

$$
F = \frac{\partial f(x_t, u_t)}{\partial x} \Big\vert_{x_t,u_t} = \begin{bmatrix} \frac{\partial f_1}{x_1} & \frac{\partial f_1}{x_2} & \dots \\ \frac{\partial f_2}{x_1} & \frac{\partial f_2}{x_2} & \dots \\ \vdots & \vdots &  \end{bmatrix} 
$$

$$
H = \frac{\partial h(x_t)}{\partial x} \Big\vert_{x_t} = \begin{bmatrix} \frac{\partial h_1}{x_1} & \frac{\partial h_1}{x_2} & \dots \\ \frac{\partial h_2}{x_1} & \frac{\partial h_2}{x_2} & \dots \\ \vdots & \vdots &  \end{bmatrix}
$$


**Exercise**: Compute the following:

**Problem 1** Let $$X = [x, \dot{x}, y]$$ and $$f(X) = \sqrt{x^2 + y^2}$$. Compute the Jacobian $$\frac{\partial f(X)}{\partial X} \Big\vert_{x_t}$$ 

**Problem 2** Assume we have a motion model defined by the set of equations:

$$
x_{t+1} = x_{t} - R \sin(\theta_t) + R\sin(\theta_t + \beta) \\
y_{t+1} = y_{t} + R \cos(\theta_t) - R\cos(\theta_t + \beta) \\
\theta_{t+1} = \theta_{t} + \beta 
$$

Let the state vector be composed of variables $$x$$, $$y$$, and $$\theta$$. Compute the Jacobian of the nonlinear system of equations with respect to the state vector.

## Extended Kalman Filtering
When we compute our linearized system, how do we end up using this in our Kalman Filter? 

| | Linear Kalman Filter | EKF | 
| --- | --- | --- |
| Prediction Step | $$\hat{x} = Fx + Bu \\ \hat{P} = FPF^T + Q $$ | $$F = \frac{\partial f}{\partial x} \Big\vert_{x,u} \\ \hat{x} = f(x,u) \\ \hat{P} = FPF^T + Q$$ |
| Update Step| $$y = z - H\hat{x} \\ S = H\hat{P}H^T + R \\ K = \hat{P}H^TS^{-1} \\ x = \hat{x} + Ky \\ P = (I - KH)\hat{P}$$ | $$ H = \frac{\partial h}{\partial x} \Big\vert_{\hat{x}} \\ y = z - h(\hat{x}) \\ S = H\hat{P}H^T + R \\ K = \hat{P}H^TS^{-1} \\ x = \hat{x} + Ky \\ P = (I - KH)\hat{P} $$ | 

In general, we use our nonlinear equations to set our predictive state and update residual, and use our linearized matrices to estimate covariances and Kalman gain.


**Exercise**: Read Roger Labbe's [_Kalman and Bayesian Filters in Python_ Chapter 11](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/11-Extended-Kalman-Filters.ipynb) entry (feel free to pull and run his notebook for this! you do _not_ need to do the embedded exercises in the notebook) and answer the following questions:
* In the tracking airplane problem, is the state space linear or nonlinear? Is the measurement space linear or nonlinear?
* In the tracking airplane problem, the dimensionality of all of the vectors is not necessarily super obvious. Re-summarize each of the elements of the model and perform the dimensional analysis for each of the prediction and update steps. Do these results make sense to you?
* Do a close reading of the robot localization problem (you may have to go to the [unscented Kalman filter chapter](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/10-Unscented-Kalman-Filter.ipynb) to read about the derivation of the motion model for this problem). 
  * Explain how the control inputs in this problem are incorporated into the Kalman filter prediction step.
  * Summarize the model for this problem (state, actions, measurements and choices for noise covariances).
  * Describe implementation considerations made to translate the EKF math into a computable set of functions.
  * What do the plots demonstrate about the Kalman Filter performance?
  * What are some potential weaknesses or sensitivities of this filter? Under what conditions might this filter _diverge_?

## Going Further

* For a further discussion on why nonlinearity is hard and how it impacts the standard Kalman Filter assumptions, Roger Labbe has an excellent [book chapter with Python examples](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python/blob/master/09-Nonlinear-Filtering.ipynb) that can be reviewed.
* For a complete breakdown of the math of the extended Kalman filter, Chapter 3.3 in the textbook _Probabilistic Robotics_ provides a thorough review.


## Day Activity
Today's day activity is designed to provide time on the [simulation assignment](https://canvas.olin.edu/courses/1002/assignments/18433) which aims to complement the mathematical discussions in class with practical implementation.

### Problem 1: Recap of Today's Notes
Go back through today's written notes on this page and work through each of the exercises / be sure to document your answers to the exercises discussed in class (there should be a total of 2 exercises in today's notes).

### Work On The Simulation Assignment
Nothing to turn in for day-activity work here; just get started with the simulation assignment!