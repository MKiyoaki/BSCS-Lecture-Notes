## REVISION

>[🏠 MENU - 6CCS3ML1](year3/6ccs3ml1.md)
>
>[⬅️ WEEK X - Learning from Demonstration](year3/6ccs3ml1/w10.md)
>
>⏹️

### 11.1. Definitions

##### 11.1.1. Machine Learning

- Taxonomy
  - Supervised Learning: There is a desired output defined for each feature vector in the training set. 
    - Classification
    - Regression
  - Unsupervised Learning: The desired output is undefined for each feature vector in the training set.
    - Clustering
  - Reinforcement Learning: learning what to do (i.e. how to map situations to actions) to maximise a numerical reward signal.
- Dataset
  - Train set
  - Test set
  - Validate set
- K-Fold Cross Validation



##### 11.1.2. Naive Bayes Classifier

- Formula
  $$
  p(h|\mathcal{D}) = \frac{p(\mathcal{D} | h) p(h)}{p(\mathcal{D})}
  $$
  

  - $p(h|D)$: postprior probabiltiy
  - $p(D|h)$: likelihood
  - $p(h)$: prior probabiltiy
  - $p(D)$: evidence



##### 11.1.3. Hidden Markov Model (HMM)

- Components
  
  - Probability Transition Matrix
    $$
    A =
    \begin{bmatrix}
    - & a_{01} & a_{02} & ... & a_{0N} & - \\
    
    
    \end{bmatrix}
    $$
    where $a_{ij} = P(X_t = x_j|X_{t-1} = x_i)$, and $\forall i \sum_{j=1}^{N+1}a_{ij} = 1$
    
  - Emission Matrix
  
- First order HMM assupmtions

  - Markov Assuption: 
  - Output Independence

##### 11.1.4. Neural Network

- Perceptron
  - Approximate to linear functions. 
  - `Input` =`weight`=> `output`
- Multilayer Perceptron
  - Approximate to any functions (Universal Approximator). 
  - `Input` =`weight`=> `hidden` =`weight`=> `output`

##### 11.1.5. Reinforcement Learning

- Model-based: Algorithms that require transition model.
  - Adaptive Dynamic Programming (ADP)
- Model-free: Algoriths that NOT require transition model.
  - Direct utility estimation
  - Temporal Difference Learning (TD)
- Function approximation
  - Example: Deep Reinforcement Learning

##### 11.1.6. Evolutionary Algorithm

- Evolutionary Algorithm
  - Steps:
    1. Initialize population with random values.
    2. Perform tasks to calculate the fitness of population.
    3. Select the most fitness populations as the next generation.
    4. Reproduce new population. 
    5. Repeat with 2. 3. 4. steps. 
- Genetic Algorithm
  - Use binary bit string as representations.
    - Through encoding methods. 
    - Invalid, multiple, uncertain: populations that not satisfy the constrains of the problem. 
  - Crossover
    - Use a mask to control.
    - Swap the bit with mask of zero places.
  - Mutation
    - Rate of mutation: rate of populations to execute mutation.
    - n-point mutation: randomly select $n$ bits to filp the value. 
- Genetic Programming
  - Use trees as representations.
  - Crossover
    - Swap a part of the sub-trees or branches.
  - Mutation
    - Randomly change the value of some nodes. 

### 11.2. Formulas

##### 11.2.1. Measurement Metrices

- Confusion Matrix	

  | Predicted / Actual | **T**          | **F**          |
  | ------------------ | -------------- | -------------- |
  | **T**              | True Positive  | False Positive |
  | **F**              | False Negative | True Negative  |
  
  - True or False: Whether Prediction is same as actual label. 
  - Postive or Negative: Whether predicted output is positive or negative. 
  
- Precision
  $$
  \text{Precision} = \frac{TP}{TP + FP}
  $$
  
- Recall
  $$
  \text{Recall} = \frac{TP}{TP + FN}
  $$
  
- F-beta Score

  - F1 Score
    $$
    F_1 = 2 \left( \frac{\text{precision × recall}}{\text{precision + recall}} \right)
    $$
    
  - F-beta Score
    $$
    F_\beta = (1 + \beta^2) \left( \frac{\text{precision × recall}}{\beta^2 \text{precision + recall}} \right)
    $$


##### 11.2.2. Decision Trees

- Entropy
  $$
  H(S) = H(\langle \frac{p}{p+n}, \frac{n}{p+n} \rangle) = \sum^c_{i=1} - p_i \log_2 p_i
  $$

- Information Gain
  $$
  \text{Gain}(S, A) = H(S) - \sum_i \frac{S_i}{|S|} H(S_i)
  $$

- Gain Ratio
  $$
  \text{SplitInfo}(S, A) = - \sum_i \frac{|S_i|}{|S|} \log_2 \frac{|S_i|}{|S|}\\
  \text{GainRatio}(S, A) = \frac{\text{Gain}(S, A)}{\text{SplitInfo}(S, A)}
  $$
  
- Gini Impurity
  $$
  G(S) = 1 - \sum^c_{i=1} p_i^2 \\
  \text{GiniImpurity}(S, A) = \sum_i \frac{|S_i|}{|S|} G(S_i)
  $$

##### 11.2.3. Kernel Machine

- Maximum Margin
  $$
  \begin{array}{l}
  \min_{x_i; y_i=-1} \frac{|f(x_i)|}{\parallel \textbf{w} \parallel} + \min_{x_i; y_i=+1} \frac{|f(x_i)|}{\parallel \textbf{w} \parallel} \\
  = \frac{1}{\parallel \textbf{w} \parallel} \left( \min_{x_i; y_i=-1} |f(\textbf{x}_i)| + \min_{x_i; y_i=+1} |f(\textbf{x}_i)| \right) \\
  = \frac{2}{\parallel \textbf{w} \parallel}
  \end{array}
  $$
  where $\parallel \cdot \parallel$ denotes euclidean distance. 
  
- Linear SVM

  - The linear SVM classifier (linearly separable case) can be found by solving the solution $(\textbf{w}, w_0, λ_i)$ to the following conditions:
    $$
    \begin{array}{lcl}
    \frac{\partial \mathcal{L}(\textbf{w}, w_0, \lambda)}{\partial \textbf{w}} = \textbf{0} &\implies
    &\textbf{w} = \sum^N_{i=1} \lambda_i y_i \textbf{x}_i \\
    \frac{\partial \mathcal{L}(\textbf{w}, w_0, \lambda)}{\partial w_0} = 0 &\implies 
    &\sum^N_{i=1} \lambda_iy_i=0 
    \end{array}
    $$
    where
    $$
    \lambda_i \geq 0 \\
    \lambda_i(y_i(\textbf{w}^T\textbf{w}_i + w_0) -1) = 0, i \in \{1, 2, ..., N \}
    $$

    - Support Vector: sample $i$ where $\lambda_i > 0$

  - Hard classifier
    $$
    f(\textbf{x}) = \text{sgn}(\textbf{w}^T \textbf{x} + w_0)
    $$
    where $\text{sgn}(z) = \begin{cases}-1 &\text{if }z<0 \\ +1 &\text{if }z \geq 0\end{cases}$

  - Soft classifier
    $$
    f(\textbf{x}) = h(\textbf{w}^T \textbf{x} + w_0)
    $$
    where $h(z) = \begin{cases} -1 &\text{if } z<-1 \\ z &\text{if } -1 \leq z \leq1\\ +1 &\text{if } z > 1\end{cases}$

- Nonlinear SVM

  - Kernel Tricks
    
    - Define a kernel function $K(\cdot)$ to replace the dimensional promotion funciton. Reduce the computation complexity. 
    
    $$
    \arg\max_\alpha \sum_i \alpha_i - \frac{1}{2} \sum_{i, j}\alpha_i\alpha_jy_iy_j F(\textbf{x}_i) \cdot F(\textbf{x}_j)
    $$
    
    $$
    \arg\max_\alpha \sum_i \alpha_i - \frac{1}{2} \sum_{i, j}\alpha_i\alpha_jy_iy_j K(\textbf{x}_i \cdot \textbf{x}_j)
    $$
    

##### 11.2.4. Neural Networks

- Perceptron
  - Error Correction Rule
    $$
    w_i \leftarrow w_i + \Delta w_i \\
    \Delta w_i = \alpha (t - g(s))x_i
    $$
  
  - Delta Learning Rule
    $$
    w_i \leftarrow w_i + \Delta w_i \\
    \Delta w_i = \alpha (t - s)x_i
    $$
  
  - Backpropagation Rule (Generalized Delta Learning Rule)
    $$
    w_i \leftarrow w_i + \Delta w_i \\
    \Delta w_i = \alpha (t - g(s))g'(s)x_i
    $$
- Multilayer Perceptron (MLP)

##### 11.2.5. Reinforcement Learning

- Action-value Methods
  - Action-value Estimation
    $$
    Q_t(a) = \frac{r_1 + r_2 + ... + r_{k_a}}{k_a}
    $$
    
    $$
    \begin{array}{lcl}
    Q_{k+1} &= &\frac{1}{k+1} \sum^{k+1}_{i=1} r_i \\
    ~ &= & Q_k + \frac{1}{k+1} (r_{k+1} - Q_k)
    \end{array}
    $$
    
  - Epsilon-Greedy
    $$
    a_t =
    \begin{cases}
    a^*_t & \text{with the probability } \Pr(1 - \epsilon) \\
    \text{random action} & \text{with the probability } \Pr(\epsilon) \\
    \end{cases}
    $$

- Direct Utility Estimation
  - Calculate the average reward (from this state until the end state):
    $$
    U(s) = \frac{1}{N} \sum^N_{i=1} G_i(s)
    $$
    where $G_i(s)$ is the $i$-th accumulative rewards start from the state $s$, defined as:
    $$
    G_t = \sum^\infin_{k=0} \gamma^k r_{t+k}
    $$
- Adaptive Dynamic Programming (ADP)
  
  - Model-based
  - Bellman Equation
    $$
    U^\pi(s) = R(s) + \gamma \sum_{s'} P(s'|s, \pi(s)) U^\pi(s')
    $$
  - Value Iteration
  - Policy Iteration
- Temporal Difference Learning (TD)

  - Model-free

  - TD Update Rule
    $$
    U^\pi(s) \leftarrow U^\pi(s) + \alpha \left( \underbrace{R(s) + \gamma U^\pi(s')}_\text{actual utiltiy} - \underbrace{U^\pi(s) }_\text{esitmate utiltiy}\right)
    $$

- Q-Learning

  - Off-policy
    
  - Q-Learning Update Rule
    $$
    Q(s, a) \leftarrow Q(s,a) + \alpha \left( R(s) + \gamma \max_{a'} Q(s', a') - Q(s, a) \right)
    $$
    

- SARSA

  - On-policy
  - SARSA Update Rule
    $$
    Q(s, a) \leftarrow Q(s, a) + \alpha \left( R(s) + \gamma Q(s', a') - Q(s, a) \right)
    $$
