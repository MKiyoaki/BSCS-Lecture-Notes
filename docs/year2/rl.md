# Reinforcement Learning Notes



---

### Algorithms

> Basing on decision problems of MDPs, there are the following algoritms: 
>
> >Model-Based
> >
> >>- **Value Iteration**
> >>- **Policy Iteration**
> >
> >Model-Free
> >
> >>Value-Based
> >>
> >>- **Sarsa** - *On-policy*
> >>
> >>- **Q-Learning** - *Off-policy
> >>
> >>- **DQN** - *Based on neural networks*
> >
> >>Policy-Based
> >>
> >>- **Policy Gradient**
> >

#### 1. Q-Learning

***Theorem 1.1.*** $Q(s,a)=R(s,a)+\gamma\cdot\underset{\tilde{a}}{max}\{Q(\tilde{s},\tilde{a})\}$

Specifically, $s, a$ implies the current state and action, $\tilde{s},\tilde{a}$ implies the next state and action of the current state $s$, learning rate $\gamma$ is a constant number that statisfy $0\leq\gamma<1$. 

> Q-Learning steps
>
> *Step 1.* Initializing learning rate $\gamma$ and the reward matrix $R$. 
>
> *Step 2.* Set $Q:=0$. 
>
> *Step 3.* For each episode: 
>
> >*3.1.* Randomly choose an initial state $s$
> >
> >*3.2.* If the agent does not get to the goal state, executing the following steps: 
> >
> >>a. Choosing an action $a$ among all the possible available actions under the current state $s$. 
> >>
> >>b. Get to the next state $\tilde{s}$ by the choosen action $a$. 
> >>
> >>c. Calculate $Q(s, a)$ 
> >>
> >>d. Set $s:=\tilde{s}$. 
> >
>
> *Step 4.* Continue execute *Step 3.* until the value table $Q$ convergent to a stable table. Normalization by divding every number in the matrix with the maximum value. 

#### 2. Sarsa

#### 3. Deep Q Network (DQN)

***Theorem 3.1.*** Idea of DQN

Instead using a Q-table to store the reward information of the environment, we can train a neural network to calculate the value of $Q(s,a)$ for a particular action $a$ under the state $s$. 

***Theorem 3.2.*** Lost Function: $L_{loss}=(R_{t+1}+\gamma_{t+1}\underset{q_\theta}{max}(S_{t+1}+a')-q_\theta(S_t,A_t))^2$
