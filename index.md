---
layout: splash
title: "Probabilistic Robotics 2026"
header:
  overlay_image: "website_graphics/robo_splash.jpg"
  overlay_color: "#000"
  overlay_filter: "0.4"


feature_row_setup:
  - image_path: website_graphics/neato_overview.jpeg
    alt: "A picture of a Neato robotic vacuum with a custom remote control interface based on Raspberry Pi"
    excerpt: "This documentation describes the environmental setup and tools used for this class.

    ### Student Facing Documentation\n
    * [ProbRobo Environmental Setup](documentation/setup)\n
    * [CompRobo Environmental Setup](https://comprobo25.github.io/) (optional for deep dives)\n"

feature_row_robot_localization:
  - image_path: website_graphics/rviz_mapping_frontpage.png
    alt: "The Neato robot in the simulated Guantlet world with a small number of laser scans collected to localize."
    excerpt: "Students will learn the fundamentals of localization and mapping from first principles.

    ### Core Activities\n
    * Filtering, Kalman Filtering, and Localization\n
    * SLAM (EKF-SLAM, Pose-Graph SLAM) and Optimization\n
    * Deep Dive on a topic of choice\n"

feature_row_robot_planning:
  - image_path: website_graphics/final_projects_collage_2024.png
    alt: "A series of 3 images, moving clockwise starting on the top left, two Neatos coordinating paths around one another, an RViz diagram for a Neato moving quickly through unknown space, and a 6DOF manipulator playing chess."
    excerpt: "Students will learn about planning under uncertainty, covering decision processes, belief representations, and policy search.

    ### Core Activities\n
    * Markov Devision Processes, Value Iteration, and Policy Iteration\n
    * Partially-Observable Markov Decision Processes\n
    * Belief Representations\n
    * Bounded, Heuristic, and Sampling-Based Search\n
    * Deep Dive on a topic of choice\n"

feature_row_robot_implications:
  - image_path: website_graphics/real_robots.png
    alt: "A collage of robots including AUV Sentry, Asimo, Waymo car, a coffee maker, factory assembly arms, and a Boston Dynamics Spot."
    excerpt: "By virtue of being embodied, robots can literally change the world. We will develop a definition for intelligent robots, and discuss implications in current and future society.

    ### Core Activities\n
    * Discussions on Theory, Practice, and Implications\n
    * What, Why, and How of Embodied Intelligence\n
    * Review of modern markets for and research on intelligent robots\n"

---

This class is all about the intersection of robotics and probabilistic reasoning. Robots, by virtue of being embodied, interact with the world -- and as it turns out, the world is an incredibly complex and uncertain place. For robots to perform what we might consider "intelligent" or complex tasks, such as navigating previously unseen terrain in search of a nebulous goal (e.g., planetary exploration, search and rescue) or collaborating with people in dynamic environments (e.g., factory floors, households), those robots need a way of representing the world, and leveraging that representation to plan out actions and action sequences. This is the essence of _Probabilistic Robotics_: the fundamentals needed to create and use representations (abstractions) of the world for a robot. 


<!-- {% include feature_row %}-->

## <a name="module-details"/> Robot Localization and Mapping

{% include feature_row id="feature_row_robot_localization" type="left" %}

## <a name="module-details"/> Robot Planning Under Uncertainty

{% include feature_row id="feature_row_robot_planning" type="right" %}

## <a name="module-details"/> Implications of "Robot Intelligence"

{% include feature_row id="feature_row_robot_implications" type="left" %}


## In-class Activities
Note: see [Site-wide TOC for an easy to navigate outline of each day's activities](toc)
Note: Subject to change as the semester unfolds!

### A Primer On Probability and Bayesian Estimation
* [Day 1: Welcome! And Probability Axioms](in-class/day01)
* [Day 2: Independence, Conditional Independence, and Distributions](in-class/day02)
* [Day 3: An Introduction to Bayesian Estimation](in-class/day03)
* [Day 4: Noise and Covariance](in-class/day04)

### State Estimation, Localization, and Mapping (Kalman Filtering and SLAM)
* [Day 5: Smoothing and Filtering](in-class/day05)
* [Day 6: Kalman Filtering I: Overview and Intuition](in-class/day06) 
* [Day 7: Kalman Filtering II: Linearization and Assumptions](in-class/day07)
* [Day 8: Kalman Filtering III: Extensions](in-class/day08)
* [Day 9: SLAM I: Overview and Intuition](in-class/day09)
* [Day 10: Deep Dive 1 // SLAM II: Probabilistic Optimization](in-class/day10)
* [Day 11: Deep Dive 1 // SLAM III: Extensions and Modern Approaches](in-class/day11)
* [Day 12: Deep Dive 1 // Theory, Practicalities, and Implications](in-class/day12)
* [Day 13: Deep Dive 1 Share-Outs](in-class/day13)

### Planning Under Uncertainty (Decision Processes and Beliefs)
* [Day 14: Decision-Making Processes and Formalism](in-class/day14)
* [Day 15: Markov Decision Processes and Value Iteration](in-class/day15)
* [Day 16: Partially-Observable Markov Decision Processes and Policy Iteration](in-class/day16)
* [Day 17: Planning Under Uncertainty I: Belief Spaces](in-class/day17)
* [Day 18: Planning Under Uncertainty II: Search Methods](in-class/day18)
* [Day 19: Deep Dive 2 // Planning Under Uncertainty III: Information-Theoretic Heuristics](in-class/day19)
* [Day 20: Deep Dive 2 // Planning Under Uncertainty IV: Extensions and Modern Approaches](in-class/day20)
* [Day 21: Deep Dive 2 // Theory, Practicalities, and Implications](in-class/day21)
* [Day 22: Deep Dive 2 Share-Outs](in-class/day22)

### Defining Intelligent Robots and Examining Implications
* [Day 23: Intelligent Robots and Embodied Intelligence: What, Why, How](in-class/day23)
* [Day 24: Markets and Research for Intelligent Robots](in-class/day24)
* [Day 25: A Conceptual Roadmap for Probabilistic Robotics](in-class/day25)
* [Day 26: Final Class: Summary, Implications, and Reflection](in-class/day26)


