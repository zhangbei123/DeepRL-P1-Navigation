#### Udacity Deep Reinforcement Learning Nanodegree
### Project 1: Navigation
# Train an RL Agent to Collect Bananas
:banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana::banana:

##### &nbsp;

<img src="images/banana_agent.gif" width="100%" align="top-left" alt="" title="Banana Agent" />
_Above: animation of my final trained agent_

##### &nbsp;

## Goal
In this project, I build a reinforcement learning (RL) agent that navigates an environment that is similar to [Unity's Banana Collector environment](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#banana-collector).

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana. The goal of our agent is to collect as many yellow bananas as possible while avoiding blue bananas. In order to solve the environment, our agent must achieve an average score of +13 over 100 consecutive episodes.

##### &nbsp;

## Approach
Here are the high-level steps taken in building an agent that solves this environment.

1. Evaluate the state and action space.
1. Establish baseline using a random action policy.
1. Implement learning algorithm.
1. Run experiments to measure agent performance.
1. Select best performing agent and capture video of it navigating the environment.

##### &nbsp;

### 1. Evaluate State & Action Space
The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around the agent's forward direction. Given this information, the agent has to learn how to best select actions. Four discrete actions are available:

- `0` move forward
- `1` move backward
- `2` turn left
- `3` turn right


##### &nbsp;

### 2. Establish Baseline
Before building an agent that learns, I started by testing an agent that selects actions (uniformly) at random at each time step.

```python
env_info = env.reset(train_mode=False)[brain_name] # reset the environment
state = env_info.vector_observations[0]            # get the current state
score = 0                                          # initialize the score
while True:
    action = np.random.randint(action_size)        # select an action
    env_info = env.step(action)[brain_name]        # send the action to the environment
    next_state = env_info.vector_observations[0]   # get the next state
    reward = env_info.rewards[0]                   # get the reward
    done = env_info.local_done[0]                  # see if episode has finished
    score += reward                                # update the score
    state = next_state                             # roll over the state to next time step
    if done:                                       # exit loop if episode finished
        break

print("Score: {}".format(score))
```

Running this agent a few times resulted in scores from -2 to 2. Obviously, if the agent needs to achieve an average score of 13 over 100 consecutive episodes, then choosing actions at random won't work.


##### &nbsp;

### 3. Implement Learning Algorithm
Agents use a policy to decide which actions to take within an environment. The primary objective of the learning algorithm is to find an optimal policy&mdash;i.e., a policy that maximizes the reward for the agent. Since the effects of possible actions aren't known in advance, the optimal policy must be discovered by interacting with the environment and recording observations. Therefore, the agent "learns" the policy through a process of trial-and-error that iteratively maps various environment states to the actions that yield the highest expected reward. This type of algorithm is called Q-Learning.

As for constructing the Q-Learning algorithm, the general approach is to implement a handful of different components, then run a series of tests to determine which combination of components and which hyperparameters yield the best results.

In the following sections, we'll describe each component of the algorithm in detail.

#### Q-Function
To discover an optimal policy, I setup a Q-function. The Q-function calculates the expected reward `R` for all possible actions `A` in all possible states `S`.

<img src="images/Q-function.png" width="19%" align="top-left" alt="" title="Q-function" />

We can then define our optimal policy `π*` as the action that maximizes the Q-function for a given state across all possible states. The optimal Q-function `Q*(s,a)` maximizes the total expected reward for an agent starting in state `s` and choosing action `a`, then following the optimal policy for each subsequent state.

<img src="images/optimal-policy-equation.png" width="47%" align="top-left" alt="" title="Optimal Policy Equation" />

One challenge with this approach is choosing which action to take while the agent is still learning the optimal policy. Should the agent choose an action based on the values of `Q(s,a)` observed thus far? Or, should the agent try a new action in hopes of earning a higher reward? This is known as the **exploration vs. exploitation dilemma**.

To address this, I implemented an 𝛆-greedy algorithm. This algorithm allows the agent to systematically manage the exploration vs. exploitation trade-off. The agent "explores" by picking a random action with some probability epsilon `𝛜`. However, the agent continues to "exploit" its knowledge of the environment by choosing actions in accordance with the current policy with a probability of (1-𝛜).

Furthermore, the value of epsilon is purposely decayed over time, so that the agent favors exploration during its initial interactions with the environment, but increasingly favors exploitation as it gains more experience. 

#### Deep Q-Network





#### Experience Replay

Experience replay allows an RL agent to learn from past experience.

Each experience is stored in a replay buffer as the agent interacts with the environment. The replay buffer contains a collection of experience tuples (S, A, R, S'). The agent then samples from this buffer as part of the learning step. Experiences are sampled randomly, so that the data is uncorrelated. This prevents action values from oscillating or diverging catastrophically, since a naive Q-learning algorithm that learns from each experience tuple in sequential order can become biased by the effects of this correlation.

Also, experience replay improves learning through repetition. By doing multiple passes over the experience data, our agent has multiple opportunities to learn from individual tuples. This is particularly useful for rare interactions within the environment.


#### DDQN


**Dueling**

##### &nbsp;

### 4. Run Experiments
Measure agent performance
- Experiment summary

- Hyperparameter exploration


##### &nbsp;

### 5. Select best performing agent

- Best performing agent + graph

The complete set of results and steps taken can be found in [this notebook](Navigation_final.ipynb).

[Here]() is a video showing the agent's progress as it goes from randomly selecting actions to learning a policy that maximizes rewards.

<img src="images/video_thumbnail.png" width="40%" align="top-left" alt="" title="Banana Agent Video" />


##### &nbsp;

## Future Improvements
- Implement way to enable/disable replay buffer.
- **Prioritized experience replay** &mdash; Prioritized experience replay increases the probability that rare or important experience vectors will be sampled. Rather than selecting experiences randomly, they are selected based on a priority value that is correlated with the magnitude of error.


##### &nbsp;
##### &nbsp;

---

# Project Starter Code
The project starter code can be found below, in case you want to run this project yourself.

Also, the original Udacity repo for this project can be found [here](https://github.com/udacity/deep-reinforcement-learning/tree/master/p1_navigation).


### Getting Started

1. Download the environment from one of the links below.  You need only select the environment that matches your operating system:
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Windows_x86_64.zip)

    (_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

    (_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P1/Banana/Banana_Linux_NoVis.zip) to obtain the environment.

2. Place the file in the DRLND GitHub repository, in the `p1_navigation/` folder, and unzip (or decompress) the file.

### Instructions

Follow the instructions in `Navigation.ipynb` to get started with training your own agent!  
