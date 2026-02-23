---
title: "Deep Dive 1: State Estimation"
toc_sticky: true
toc_h_max: 3
---

Over the last few weeks we've learned some of the fundamental language of _state estimation_ in modern robotics systems -- from Bayesian estimation to Markov chains to Kalman filtering. This deep dive project provides an opportunity for you to dig deeper into any one of the topics we've discussed in class or alternative modern techniques. You will decide on a topic to explore, an artifact to create, and present your learnings to members of the class.

## Learning Objectives
* Practice independent research skills (including, e.g., literature search, technical writing/communication)
* Gain technical depth in an area of state estimation, where depth comes from either theoretical derivations and/or applied implementations
* Practice technical critique, both providing meaningful feedback and receiving feedback

## Deliverables

### Project Proposal (Due Thursday 2/26 at 1PM)
Before class on Thursday 2/26, please prepare a short (~1-2 page) project proposal for your deep dive. Please include in this proposal:
* The title of the project
* A brief description of the project topic and learning objectives
* The proposed deep dive artifact (technical paper, teach-in presentation/learning activity, open-source implementation, extension of our in-class simulator, etc.) -- consider proposing an MVP and a set of stretch goals
* An initial list of resources that will be used to learn more about the deep dive topic
* A timeline for completing your deep dive (when developing your timeline, please keep in mind work for other classes, ongoing Candidates' Weekend activities, or other commitments)
* Any questions or support you would like from the teaching team

Please submit your project proposals [to Canvas](https://canvas.olin.edu/courses/1002/assignments/17540); these will be reviewed with you in class on Thursday.

### Deep Dive Check-In (Due Thursday 3/5 at 1PM)
In class on Thursday 3/5, the teaching team will briefly check-in with you to discuss any changes in scope for your deep dive and deep dive proposal, particularly with an eye towards the in-class presentation day on Thursday 3/12. For this check-in, please be prepared to discuss your current project status, show a draft of your in-class presentation, and discuss any support you might need to complete your project.

### In-Class Presentation / Demo (Due Thursday 3/12 at 1PM)
We will be sharing our work with one another in-class on Thursday 3/12. Participation is required; if you anticipate being absent on this day, please inform the teaching team as soon as possible to work out an alternate schedule with you.

Each person will have 12 minutes to present their project in a format that they choose (discussion, lecture, teaching activity, demonstration, technical presentation, etc.). There will then be 3 minutes for audience Q&A, followed by 3 minutes of transition period in which the next presenter will set-up and audience members will complete a feedback survey for the previous presenter.

Presentation materials can be submitted [to Canvas](https://canvas.olin.edu/courses/1002/assignments/18512) as a record for your in-class participation. Feedback survey results will be made available to presenters immediately after class.

### Written / Computation Deliverables (Due Friday 3/13 at 5PM)
All deep dive written materials (including technical reports, project documentation, publicly-available code, etc.) are due on 3/13 at 5PM. In addition to all project materials, please also include a brief written reflection about the deep dive project. While the substance of the written reflection is up to you, some suggested points of discussion may be:
* A response to presentation-day feedback collected from peers
* Your approach to completing your deep dive project, identifying what worked well and what might be done differently in the future
* Aspects of your topic you found particularly interesting or surprising
* Remaining questions about your topic
* What you felt like you got out of your deep dive topic
* What you felt like you got out of hearing about others' deep dive topics
* How your topic complemented the course material for this unit

All materials can be submitted [to Canvas](https://canvas.olin.edu/courses/1002/assignments/18513).


## State Estimation in Robotics
For this project you will be selecting a topic related to state estimation in robotics to perform a deep dive. State estimation is simply the process of inferring the state of a robot or an environment from collected actions and observations. 

In class, we've talked about two big application areas for state estimation: localization and mapping (including doing these things simultaneously). And we've talked about some of the foundational modern methods for performing localization and mapping, including Kalman Filtering, Extended Kalman Filtering, and EKF-SLAM. 

But there is so much more to learn! 

### Possible Topics
For this deep dive, you should feel like you have some creative freedom to pursue a topic and artifact to create. Here are a few possible topics that we think might be good material for a deep dive to kick-start your own brainstorming process:
* Logic systems (an alternative to Bayesian methods) for state estimation
* Monte Carlo Methods
* Kalman Smoothing
* Unscented Kalman Filtering 
* Graph-Pose SLAM
* Visual SLAM (e.g., ORB-SLAM)
* Reinforcement Learning
* Unsupervised Learning
* Uncertainty Quantification in Learning-Based Estimators
* ...your ideas here!

### Possible Modalities
Your artifact can take any form, but do remember that you'll be presenting your deep dive in class at the end of this project. Some possible forms of your artifact could include: a mini-lecture, a technical presentation, a hands-on activity, a rapid paper reading, a discussion, a demonstration of something you implemented. Here are a few ideas to get you thinking about what you might like to create:
* Expanding the simulator assignment to handle SLAM
* Designing a teach-in around data collection and filtering
* Testing a localization method with a Neato
* Creating a Python applet that simulates the Wumpus World and teaches Bayesian estimation 
* Writing a brief synthesis paper on papers focused on modern uses of EKF
* Developing a mini-lecture on Bayesian Estimation used within Reinforcement Learning
* ...your ideas here!


## Resources

### Project Advice
* This is a relatively snappy project -- start with an MVP you feel confident in completing in the allotted time, and have a few stretch goals you can use to further deepen you project.
* Choose a topic and/or modality that you are personally excited or curious about.
* Feel free to use this project to further your learning in other classes or projects; connecting to other material you know about or want to learn is welcome!
* Keep track of all the resources you used along the way in an annotated bibliography; this is a great "bonus artifact" to have, and is useful for tracking attributions/citations you will need to make in your final products.
* Have a draft of your final presentation by the mid-project check-in -- even if it's just a rough outline!
* You will have some time in class to work on this project; come in to each class with a mini-goal or clear idea of what you would like to work on to make this time productive.

### Online and Textbook Materials
This class has a [variety of texts set-aside in the library](https://library.olin.edu/reserves.php) that can be great primary material for this project.

We have been using [Roger Labbe's Python Notebooks](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python) for localization and filtering in class, and there are even more chapters we haven't discussed that might be nice material to review.

Cyrill Stachniss [taught an entire course about SLAM](https://www.youtube.com/playlist?list=PLgnQpQtFTOGQrZ4O5QzbIHgl3b1JHimN_) and uploaded it to Youtube in 2013; the material is still relevant for today!

For modern published literature, Google Scholar is a reasonably good search engine. You can also look for papers directly on conference or journal websites. Some reputable sources that might be of interest to you include:
* IEEE Transactions on Field Robotics
* IEEE Transactions on Robotics
* IEEE Robotics and Automation Letters
* IEEE Conference on Robotics and Automation (ICRA)
* IEEE/RSJ Conference on Intelligence Robots and Systems (IROS)
* Conference on Robot Learning (CoRL)
* Robotics: Science and Systems (RSS)
* International Journal of Robotics Research
* AAAS Science Robotics

