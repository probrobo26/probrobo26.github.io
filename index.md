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
    * [CompRobo Environmental Setup](https://comprobo25.github.io/) (optional for deep dives)\n
    * [Useful Resources](documentation/useful_resources) and Sample Code (TODO)\n"

feature_row_simulator:
  - image_path: website_graphics/barrel_follow.gif
    alt: "The Neato robot in a simulated world tracking a barrel"
    excerpt: "Throughout this course, we will be incrementally building our own simulators and tools for experimentation. 

    ### Supporting Documents
    * TODO\n"

feature_row_robot_localization:
  - image_path: website_graphics/rviz_mapping_frontpage.png
    alt: "The Neato robot in the simulated Guantlet world with a small number of laser scans collected to localize."
    excerpt: "Students will learn the fundamentals of localization and mapping from first principles.

    ### Supporting Documents
    * Kalman Filtering and Extensions (TODO)\n
    * EKF-SLAM (TODO)\n"

feature_row_robot_planning:
  - image_path: website_graphics/final_projects_collage_2024.png
    alt: "A series of 3 images, moving clockwise starting on the top left, two Neatos coordinating paths around one another, an RViz diagram for a Neato moving quickly through unknown space, and a 6DOF manipulator playing chess."
    excerpt: "Students will learn about planning under uncertainty, covering decision processes, belief representations, and policy search.

    ### Supporting Documents
    * MDPs and Value Iteration (TODO)\n
    * Policy Iteration (TODO)\n
    * POMDPS (TODO)\n
    * Bounded, Heuristic, and Sampling-Based Search (TODO)\n"

feature_row_robot_implications:
  - image_path: website_graphics/real_robots.png
    alt: "A collage of robots including AUV Sentry, Asimo, Waymo car, a coffee maker, factory assembly arms, and a Boston Dynamics Spot."
    excerpt: "By virtue of being embodied, robots can literally change the world. We will develop a definition for intelligent robots, and discuss implications in current and future society.

    ### Supporting Documents
    * Implications Discussions (TODO)\n"

---

The Olin College course "Probabilistic Robotics" (ProbRobo) serves as an advanced algorithms and computational course that explores the intersection of probability, statistics, and robotics for perception and decision-making. 

<!-- {% include feature_row %}-->

## <a name="robot-details"/> Environment Details and Documentation

{% include feature_row id="feature_row_setup" type="left" %}

## <a name="module-details"/> Simulator Development

{% include feature_row id="feature_row_simulator" type="right" %}

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
* [Day 1: Welcome!](in-class/day01)
<!-- * [Day 2: The Landscape of Modern Robotics // Basic ROS Concepts + Teleoperation](in-class/day02)
* [Day 3: Writing Sensory-Motor Loops in ROS2](in-class/day03)
* [Day 4: What are Broader Impacts? // Threading, Params, Propostional Control, and Wall-Following](in-class/day04)
* [Day 5: Debugging, Coordinate Frames, and Finite State Machines](in-class/day05) -->

### State Estimation, Localization, and Mapping (Kalman Filtering and SLAM)
<!-- * [Day 6: RoboBehaviors Debrief // Intro to State Estimation](in-class/day06) 
* [Day 7: Broader Impacts Discussions I // The Particle Filter for Robot Localization](in-class/day07)
* [Day 8: Applications I // Conceptual Particle Filter](in-class/day08)
* [Day 9: Broader Impacts Part 1 // Conceptual Particle Filter](in-class/day09)
* [Day 10: Applications II // Bayesian Estimation](in-class/day10)
* [Day 11: Debugging Strategies and Extensions // Studio Day](in-class/day11)
* [Day 12: Beyond Particle Filtering and Studio Time](in-class/day12)
* [Day 13: Localiztion Debrief // Machine Vision Project Ideation](in-class/day13)
* [Day 14: Applications III // Neato Soccer + Discuss Project Proposals](in-class/day14) -->

### Planning Under Uncertainty (Decision Processes and Beliefs)
<!-- * [Day 15: Keypoints // Studio Time](in-class/day15)
* [Day 16: CA Lecture: Light Fields // Camera Calibration // Studio Time](in-class/day16)
* [Day 17: Broader Impacts Discussions II // Image Segmentation](in-class/day17)
* [Day 18: Studio Time](in-class/day18)
* [Day 19: Machine Vision Showcase + Final Project Kickoff](in-class/day19)
* [Day 20: Project Proposal Generation](in-class/day20)
* [Day 21: Applications IV // Project Work Time](in-class/day21)
* [Day 22: CA Lecture: Launch Files // Project Work Time](in-class/day22) -->

### Defining Intelligent Robots and Examining Implications
<!-- * [Day 23: Applications V // Project Work Time](in-class/day23)
* [Day 24: Mini-Lectures // Project Work Time](in-class/day24)
* [Day 25: Applications VI // Project Work Time](in-class/day25)
* [Day 26: Project Work Time](in-class/day26)
* [Day 27: Final Project Showcase and Semester Reflection](in-class/day27) -->

## Bonus Materials
* [Recitation Example Code](https://github.com/comprobo25/recitation_examples)
* [On Kalman Filtering](https://github.com/comprobo25/recitation_examples/tree/main/kalman_filters)
* [On Computing Tools for Machine Vision](https://docs.google.com/presentation/d/1grR6uVaMEtdOn7u8L0aYVQq3NUlKZGraBVoBiZiWSqc/edit?usp=sharing)
* [On Simple Image Handling with OpenCV](https://github.com/comprobo25/recitation_examples/tree/main/image_processing)
* [Resources for an Introduction to Factor Graphs](recitations/factor_graphs.md)
* [An A* and RRT* Crash Course](https://github.com/comprobo25/recitation_examples/tree/main/path_planning)
* [Basics of Manipulation](recitations/manipulation.md)

## Conclusion and Learning More
ProbRobo serves as a fun, hands-on introduction to key ideas in robotics algorithms and toolsets.  Despite the fact that the course is successful at Olin, we realize that everyone's institutional context is different. To connect with folks at Olin College to learn more about this module or determine how you might build off of this at your own institution, e-mail <a href="mailto:oepp@olin.edu">Olin's External Programs and Partnerships</a> to start the conversation.
