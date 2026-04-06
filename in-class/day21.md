---
title: "Day 21: Planning Under Uncertainty -- Search"
toc_sticky: true
toc_data:
  - title: Today
    link: in-class/day21/#today
  - title: For Next Time
    link: in-class/day21/#for-next-time
  - title: Approximate POMDP Solvers
    link: in-class/day21/#pomdps-revisited
  - title: Greedy Myopic Search
    link: in-class/day21/#greedy-search
  - title: Nonmyopic Search
    link: in-class/day21/#nonmyopic-search
  - title: Deep Dive Brainstorming
    link: in-class/day21/#deep-dive-brainstorming
---

## Today
* Approximate POMDP Solvers: Compositional Algorithms
* Greedy (Myopic) Search
* Nonmyopic Search
* Brainstorming for Deep Dives

## For Next Time
* Work on the [Week 12 Day Assignments](https://canvas.olin.edu/courses/1002/assignments/18649) (Due April 13th, 7PM).
* Submit your [deep dive](../projects/deepdive_2.md) proposals -- [Canvas Submission](https://canvas.olin.edu/courses/1002/assignments/17541) (Due April 13th, 1PM).


## Approximate POMDP Solvers
We have been dissecting a modern POMDP solver over the last few classes.

<p align="center">
<img alt="Diagram of the components of a compositional POMDP solver." src="../website_graphics/compositional_pomdp_solver.png"/>
</p>

Across all solvers, the key is that we need to turn robot observations into actions that the robot can take, which hopefully lead to the robot successfully fulfilling its mission.

There are several key design decisions that need to be made for the solver:
* The robot belief representation
* The reward / value function, which is used to distinguish between actions
* The search heuristic, used to determine which actions to "score" in order to pick the best one and the form of the search itself (horizon, over what spaces, what simulator to use, etc.)

Today, we will carefully inspect the _search_ algorithm used within an approximate solver for a POMDP. 

### Search in a POMDP Solver
The search part of the approximate solver is the method to select the action that a robot should next check. Thus, we are typically _searching over the space of actions_ and using some distinguishing criteria to select the best next action for a robot to take. 

The "next best action" is likely the one that maximized approximated reward -- but over what horizon? This is the first design decision one must answer: over what _planning horizon_ should a next action be considered? There are two classes of search:
* **Myopic (Greedy) Search** -- this is a one-step look-ahead, where, based on the approximate reward function, the next best action is the one that maximized immediate expected reward.
* **Nonmyopic Search** -- this is a multi-step look-ahead, where _chains of actions_ are simulated and scored according to the reward heuristics, and the best next action to take is the one that maximizes expected future rewards. Within a nonmyopic search, there are additional design considerations, such as the length of each action chain (the horizon), and how chains are formed (the tree search policy).

Coupled with the search algorithm is how the state of actions is represented. You may have an action library (a discrete list of actions that the robot can execute) or you may have a continuous action space, which will need to be sampled in order to have atomic actions to evaluate. For our work today we will assume that the action space is discrete (or can be discretized).


## Greedy Myopic Search
Myopic planners (named after myopia, or short-sightedness) simply select the single best action to take at some planning iteration $$t$$ according to the approximate reward function. The simplest interpretation is:

$$
a^* = \arg \max_{a \in \mathcal{A}} R(s,a)
$$

where the most rewarding action, $$a^*$$ is selected from all actions in $$\mathcal{A}$$ such that the reward is maximized with respect to the robot's current estimated state. In pseudocode, this search might be executed as:

```
a* = default_policy()
for a in A:
    expected_reward = R(s, a)
    if expected_reward > R(s, a*):
        a* = a
return a*
```
It may seem obvious here that the choice of a myopic search is sensitive to the form of the action set and the reward function: making only local choices can lead to arbitrarily poor performance in even simple worlds if action sets can't "step" far enough, or the reward function fails to converge effectively to the true task reward. Consider for example the MSS POMDP -- how might a greedy myopic search fail depending on choice of action set or reward function?


## Nonmyopic Search
In an attempt to address issues with myopic planning, nonmyopic planners trade computational time for more robust action selections. In a nonmyopic planner, the _expected finite horizon reward_ is approximated for each immediate action in the action space. This is often done by simulating many possible chains of actions from a starting action and scoring the expected cumulative reward from each chain.

In practice, the most common form that one of these planners takes is a _tree search_ through which action chains, possible observations, and possible rewards are computed down different branches of the tree.

[Image of vanilla tree search]

In practice, however, such a tree can get very, very large -- if any of the action, state, or observation spaces are continuous, then each node in the tree has effectively a probability of 0 in being re-visited, leading to challenges in robust estimation of cumulative reward.

Luckily, there are some forms of tree search that are designed to monitor the expansion of a tree, utilizing _a tree policy_ to select nodes to revisit and control the growth of the tree itself. This is like embedding another planner in a planner, where the action space is which node to re-explore or newly add to the tree, and the internal tree policy reward balances explore-exploit within it.

One such common type of guided tree search is Monte Carlo Tree Search (MCTS), which consists of several steps:
1. **Selection**: A valid action from the robot's current state is selected for exploration based on the tree policy.
2. **Rollout**: Forward simulation is run, where the selected action is simulated and a series of random actions are then run up to a horizon $$h$$. The reward of the simulated trajectory is computed.
3. **Backup**: The value of the root action is updated to be the average reward accumulated by the present simulation and all previous simulations run through that node.
4. **Repeat 1-3** until a termination criteria is met (number of planning iterations, allowable compute time, convergence to a best policy)
5. **Return the next best action**

[Image of Monte Carlo Tree Search]

A common tree policy is the Upper Confidence bound for Tree search (UCT), which selects the action node to explore based on its current cumulative reward tempered with the number of times the node has been expanded. The tree policy thus can select nodes that may be very high valued, or nodes that have been rarely explored, thus balancing explore-exploit within the tree search itself.

While nonmyopic search algorithms are a bit more robust to arbitrarily poor performance than greedy solvers, they do contain significantly more design decisions, all of which can have an impact on the ultimate computational efficiency of the algorithm. 


## Today's So What
Today, we learned about search algorithms, the last puzzle piece for creating an approximate POMDP solver. We saw that there are computationally efficient solvers with potentially arbitrarily poor performance and computationally taxing solvers with further design considerations that must be selected. The search algorithm serves as the optimization scheme for find a policy for the robot to execute at each planning step. 


## Going Further
To learn more about tree searches, you might find the following resources useful:
* The background chapters of [Adaptive sampling of transient environmental phenomena with autonomous mobile platforms](https://dspace.mit.edu/handle/1721.1/124212), Preston, 2019.
* [Conintuous upper confidence trees with polynomial exploration - consistency](https://inria.hal.science/hal-00835352v1/document), Auger et al., 2013.
* This [presentation](https://docs.google.com/presentation/d/1EDupQ8LOZwwKld1OWn6qJa4CjbxE1gJ4LOIvfhtrX-Y/edit?slide=id.p#slide=id.p) providing an overview of Remi Coulom's "Efficient Selectivity and Backup Operators in Monte-Carlo Tree Search," 2006.


## Day Assignment
Today's day assignment will implement the last part that we need to have a closed-loop autonomous adaptive sampling robot in our simulator. It also carves out time for brainstorming about your deep dive projects.

### Problem 1: A Simulated Informative Sampler

### Problem 2: Deep Dive Brainstorming
