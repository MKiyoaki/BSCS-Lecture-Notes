## REVISION

>[🏠 MENU - 7CCSMPNN](year3/7ccsmpnn.md)
>
>[⬅️ WEEK X - Clustering](year3/7ccsmpnn/w10.md)
>
>⏹️

##### 1. Pattern Recognition

- Definition
  - Terminologies
    - **Pattern**: Targets including the data between completely deterministic and completely random data.
    - **Pattern Recognition**: Aim is to build algorithms that can <u>recognize useful patterns in data</u>, extract useful information from data, or make decisions based on data.
    - **Generalisation**: How well a model performs on new data.
    - **Exemplar/Datapoint/Sample**: A particular datapoint which is represented by a feature vector.
    - **Feature Space**: The multi-dimensional space defined by the feature vectors in the dataset.
    - **Feature Vector** :The combination of $d$ features is a $d$-dimensional column vector.
    - **Model**: The method used to perform the learning. 
    - **Hyper-Parameter**: a value used by the learning algorithm in its search for the optimal parameters of the classifier. 
      - **Grid search**: A method of trying to find suitable hyper-parameters that searches all possible combinations of values whtin defined ranges. 
      - **Meta-learning**: A method to learn the suitable hyper-parameters. 
    - **Decision Theory**: Methods for making decisions that reduce cost rather than misclassification rate. 
    - **Dichotomizer**: a classifier that places exemplars in one of two classes (also called binary classifier). 
    - **Linear Separable**: Exemplars from two classes in a $D$ dimensional space can be separated by a hyperplane in the dimension of $D-1$ in feature space. 
  - Principles (for classification)
    - Data
      - Types of Data
        - **discrete** (integer/symbolic valued) or 
          **continuous** (real/continuous valued), and
        - **univariate** (containing one variable) or 
          **multivariate** (containing multiple variable).
      - Dataset
        - Train set
          - The collection of feature vectors used by the learning algorithm to tune the parameters of the classifier. 
        - Test set
          - The collection of feature vectors used by to evaluate the performance of the trained classifier. 
        - Validation set
    - Features
      - Feature Extraction and Selection
    - Models
    - Training
      - The procedure used to define the parameters of the classifier is called training, or parameter tuning, or optimisation, or learning. 
      - Types of Machine Learning:
        - Supervised Learning
        - Unsupervised Learning
        - Semi-supervised Learning
          - A method that learns using both labelled and unlabelled training exemplars. 
      - Transfer Learning
        - A method that pre-trains a classifier on another task before training it on the main taask in the hope that the pre-training will help improve performance on the main task. 
    - Evaluation
      - Confusion matrix
        - True or False: whether prediction is same as the actual label.
        - Positive or Negative: whether the prediction is positive or negative. 
      - Measurement Metrices
        - Error-rate
        - Accuracy
        - Precision
          $$
          \text{Precision} = \frac{TP}{TP+FP}
          $$
        - Recall
          $$
          \text{Recall} = \frac{TP}{TP+FN}
          $$
        - F1-Score
          $$
          F_1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
          $$
      - Problems
        - **Overfitting**: making the model so specific to the training data that it fails to generalize to new data. 

---

##### 2. Discriminant Functions

- Discriminant Functions
  - **discriminant function**: a scalar function, whose output is used to make categorization decisions on the class of a given feature vector. 
  - A **linear** discriminant function uses a linear combination of feature values, in the format of follows:
    $$
    g(\textbf{x}) = \textbf{w}^T \textbf{x} + w_0
    $$

    - In binary classification, we usually have the scheme that, 
      - $\textbf{x}_i \in \omega_1$ if $g(\textbf{x}_1) > 0$, otherwise
    
      - $\textbf{x}_i \in \omega_2$ if $g(\textbf{x}_i) \leq 0$. 
    
  - A **quadratic** discriminant function is defined as follows:
    $$
    g(\textbf{x}) = w_0 + \sum_i w_i x_i + \sum_{i, j} w_{ij}x_iy_j
    $$

- Augmented Vectors

  - To simplify maths, augment weight and data vectors. **Augmentation** means to extend a vector (or matrix) by appending elements to it.
  - i.e., Instead of defining $g(\textbf{x}) = \textbf{w}^t\textbf{x} + w_0$, we define $g(\textbf{x}) = a^t y$, where $a=\begin{bmatrix}w_0 &\textbf{w}^T\end{bmatrix}^T, y = \begin{bmatrix}1 &\textbf{x}^T\end{bmatrix}^T$

- Perceptron Learning

  - Criterion Function
    - **Sequential** Perceptron Learning Algorithm
      $$
      J_p(a) = \sum_{j \in \mathcal{X}} (-a^T \textbf{y}) \\
      a \leftarrow a - \eta ∇ J(a)
      $$
      where $\mathcal{X}$ is the set of samples <u>misclassified</u> by $a$. 
    - **Batch** Perceptron Learning Algorithm
      $$
      J_p (a) = \sum_{j \in \mathcal{X}} (-\textbf{y}) \\
      a \leftarrow a + \eta \sum_{j \in \mathcal{X}} \textbf{y}
      $$

  - Sample Normalization
    - In binary classification, we can replace all samples from class $\omega_2$ by their negatives. Then, the misclassified samples are the samples that $g(\textbf{x}_i) \leq 0$. 

  - MSE Procedure

    - Motivation

      - Solves linear equations $a^t\textbf{y}_k = b_k \ \forall k$. 

    - Pseudoinverse

    - Gradient Descent
      - Widrow-Hoff (or LMS) Learning Algorithm

        - Introduces an arbitrary margin $b$. 

        - The gradient of the MSE cost function is:
          $$
          ∇ J_s (a) = \textbf{Y}^T (\textbf{Y} a - b) \\
          a \gets a - \eta ∇ J_s (a)
          $$

        - Hence, the batch update rule is:
          $$
          a \leftarrow a + \eta  (b - \textbf{Y} a) \textbf{Y}^T
          $$

        - The sequential update rule is:
          $$
          a \leftarrow a + \eta (b_k - a^T \textbf{y}_k) \textbf{y}_k
          $$

- kNN Classification

  - Provides a way to <u>partition feature-space without learning</u>. 
  - Requires:
    - a metric to measure distance
    - a value for hyper-parameter $k$

---

##### 3. Introduction to Neural Networks

- ANN

  - Components
    - Layers
      - **visible**: receive inputs from, or send outputs to, the external environment
        - **input** layers
        - **output** layers

      - **hidden**: only receive inputs and send output to other processing units

    - Functions
      - This **response function** often split into two component parts:
        - A **transfer function** that determines how the inputs are integrated.
        - An **activation function** that determines the output the neuron produces. 

    - Weights
      - Setting weights explicitly using prior knowledge.
      - Optimising connectivity to achieve some objective 

    - Units (Neurons)

- Perceptron

  - Equivalence to Linear Discriminant Function

  - Update Rule

    - Delta Learning Algorithm

      - Learning

        - **Sequential** (online) update:
          $$
          \textbf{w} \leftarrow \textbf{w} + \eta (t-y) \textbf{x}^T
          $$
        - **Batch** update:
          $$
          \textbf{w} \leftarrow \textbf{w} + \eta \sum_p (t_p-y_p) \textbf{x}^T_p
          $$

      - Properties

        - Supervised.
        - Adjust weights in proportion to the difference between the desired output $t$, and the actual output $y$.

      - Two types of sequential updates

        - False negative ($t = 1, y=0$)
          $$
          \textbf{w} \leftarrow \textbf{w} + \eta \textbf{x}^T
          $$

          - Make $\textbf{w}$ more like $x$. 

        - False positive ($t = 0, y = 1$)
          $$
          \textbf{w} \leftarrow \textbf{w} - \eta \textbf{x}^T
          $$

          - Make $\textbf{w}$ less like $x$. 

    - Hebbian Learning Rule

      - Learning

        - Sequential
          $$
          \textbf{w} \leftarrow \textbf{w} + \eta y \textbf{x}^T
          $$

      - Properties

        - Unsupervised.

        - Strengthen the connection between an active neuron and any active inputs.

- Competitive Learning Networks

  - Definition
    - Neurons compete with each other to get the right to respond the input.

    - Can be used for multi-class classification

    - Following Hebbian Learning Rule. 

  - Activation Function (Selection)
    - **Winner–Takes–All (WTA)**: Select the prediction where the output is the highest. 
    - **k–Winners–Takes–All (kWTA)**: Select $k$ predictions where the output is the highest. 
    - **Softmax**

- Negative Feedback Networks

  - Definition

    - Unsupervised.
    - Use **inhibitory feedback** connections between the output neurons and the input neurons.

  - Activation

    - Initialise $\textbf{y}$ to zero.
    - perform several iterations of:
      - Update input units: $\textbf{e} = \textbf{x} - \textbf{w}^T \textbf{y}$
      - Update output units: $\textbf{y} \leftarrow \textbf{y} + \alpha \textbf{w} \cdot \textbf{e}$
    - Tries to minimise response from input units $\textbf{e}$ (i.e. minimise reconstruction error).
    - Tries to find $\textbf{y}$ values that accurately reconstruct the input (in terms of minimising the reconstruction error).

  - Training
    $$
    \textbf{w} \leftarrow \textbf{w} + \beta \textbf{y} \cdot \textbf{e}^T
    $$
    where $\alpha, \beta$ are non-negative hyperparameters. 

    - Adjust weights to accurately reconstruct the input (in terms of minimising the reconstruction error)
    - i.e., 

      - $\textbf{w}^T \textbf{y}$ is a *reconstruction of the input*,
      - $\textbf{e}$ is the *error* between the input and the reconstruction.

- Autoencoder Networks

  - Definition
    - By representing the reconstruction using a separate neural population $\textbf{r}$, and remove the inhibitory feedback connections. Neural responses no longer to be calculated iteratively. The is called an **autoencoder**.

    - A three (two) layer neural network. Aim is for the output to reconstruct the input.
      - Hidden units impose an *information bottleneck*. 
      - Hidden units learn a useful representation of the data.
  - Learning
    - Trained using **Backpropagation** of error.
    - To avoid overfitting, *de-noising* autoencoders, add noise to inputs used for learning.
    - *Stacked autoencoders* form the basis for some Deep Learning Architectures


---

##### 4. Multilayer Perceptrons & Backpropagation

- Deep Neural Networks
  
  - Network Architectures
  
    - **Feedforward networks:** no loop exists (static system)
    - **Feedback/Recurrent networks:** loop exists (dynamic system)
  - Learning Paradigms
  
    - Supervised learning: Target is known for every input pattern. 
    - Unsupervised learning: Target is not known. 
    - Hybird learning: It combines the supervised learning and unsupervised learning. 
  - Number of weights: $w(h,h+1)=(n_h +1)×n_{h+1}$
  
- Feedforward Networks

  - (Three layer fully connected) MLPs

    - Forward Propagation
      - The feed-forward operations consists of presenting a pattern to the input units and passing (or feeding) the signals through the network in order to get outputs units (no cycles).
        $$
        net_k = \sum_{j=1}^{n_H} y_j w_{kj} + w_{k0} = \sum_{j=0}^{n_H} y_j w_{kj} = \textbf{w}^T_k \textbf{y} \\
        z_k = f(net_k)
        $$
        where the subscript $k$ indexes <u>units in the output layer</u> and $n_H$ denotes the number of hidden units, $y_0=1$. 
        
      - General feed forward operation:
        $$
        g_k(\textbf{x}) = z_k = f \left( \sum^{n_H}_{i=1} f \left( \sum^d_{i=1} x_iw_{ji} + w_{j0} \right) w_{kj} + w_{k0} \right), k \in \{1, ..., c\}
        $$
      
    - Backpropagation
      - The supervised learning consists of presenting an input pattern and modifying the network parameters (weights) to reduce distances between the computed output and the desired/teaching/target output pattern.
      - **Error on the hidden-to-output weights** (at the $m$-th iteration):
        $$
        \frac{\partial J}{\partial w_{kj}} = \frac{\partial J}{\partial net_k} \frac{\partial net_k}{\partial w_{kj}} = -\delta_k \frac{\partial net_k}{\partial w_{kj}}
        $$
        where the sensitivity of unit $k$ is defined as:
        $$
        \delta_k = - \frac{\partial J}{\partial net_k} = ... = (t_k - z_k)f'(net_k)
        $$

        - The <u>weight update (learning rule)</u> for the hidden-to-output weights (i.e., $w_{kj}$) is then:
          $$
          \Delta w_{kj} = - \alpha \frac{\partial J}{\partial w_{kj}} = \alpha \delta_k y_j = \alpha (t_k - z_k) f'(net_k)y_j \\
          w_{kj, (m+1)} \leftarrow w_{kj, (m)} + \Delta w_{kj, (m)}
          $$

      - **Error on the input-to-hidden units** (at the $m$-th iteration):
        $$
        \frac{\partial J}{\partial w_{ji}} = \frac{\partial J}{\partial y_j} \frac{\partial y_j}{\delta net_j} \frac{\partial net_j}{\partial w_{ji}} = - \underbrace{\sum^c_{k=1} \delta_k w_{kj} f'(net_j)}_{\delta_j}x_i
        $$

        - The <u>weight update (learning rule)</u> for the hidden-to-output weights (i.e., $w_{ji}$) is then:
          $$
          \Delta w_{ji} = -\alpha \frac{\partial J}{\partial w_{ji}} = \alpha \delta_j x_i = \alpha f'(net_j) \left[ \sum^c_{k=1} w_{kj}\delta_k \right] x_i \\
          w_{ji, (m+1)} \leftarrow w_{ji, (m)} + \Delta w_{ji, (m)}
          $$

      - Stochastic and Batch Backpropagation

  - Commonly Used Activation/Transfer Funcitons

    - Symmetric hard limit (sign) transfer function
    - Linear transfer function
    - Symmetric *sigmoid* transfer function
    - Logarithmic *sigmoid* transfer function
    - *Radial basis transfer* function

- RBF NNs

  - Definition
  
    - An RBF network network is a three-layer network.
      - Input unit: Linear transfer function. (No bias unit)
      - Hidden unit: Radial basis function, $h(\cdot)$. 
      
        >$y_j(\textbf{x}_p)= \exp (-\frac{\parallel \textbf{x}_p - \textbf{c}_j \parallel^2}{2 \sigma^2_j}) = e^{-\frac{\parallel \textbf{x}_p - \textbf{c}_j \parallel^2}{2 \sigma^2_j}}$
      - Output unit: Any activation function, $f(\cdot)$. (Contains a bias unit)
    - The objective is to define multiple Gaussian distributions in the hidden layer, using k-Means Clustering to determine the centres of clusters basing on the feeded exemplars, then execute linear regression to determine the output weights of different distributions. 
    - Can be used for regression or classification tasks. 
  
  - Output
    $$
    z_k(\textbf{x}) = f(\sum^{n_H}_{j=1} w_{kj}y_j(\textbf{x}) + w_{k_0}) = f(\sum^{n_H}_{j=1} w_{kj}h(\parallel \textbf{x} - \textbf{c}_j \parallel) + w_{k_0})
    $$
  
  - Learning
  
    - (*Unsupervised learning*) **Determine the centres** $\textbf{c}_j$ (and function parameters).
      - Fixed centres selected at random.
      - Clustering based approaches (k-Means Clustering).
    - (*Supervised learning*) **Determine the output weights** $w_{kj}$
      - Computing the output weights (*when linear output function is employed*) using *least squares method*.
      - *Gradient descent* (backpropagation) approaches.
    
  - Procedure:
  
    1. Choose number of hidden units, $n_H$ .
    2. Pick randomly $n_H$ input pattern from the data set (with $n$ input patterns) as the centres $\textbf{c}_j$, where $j \in \{1, ..., n_H\}$. 
    3. When Gaussian function is employed, define $\sigma_j = \frac{\rho_\max}{\sqrt{2n_H}}$ or $\sigma_j = 2\rho_\text{avg}$ for all $j$, where
       - $\rho_\max$ is the maximum distance between the chosen centres, and
       - $\rho_\text{avg}$ is the average distance between the chosen centres.
    4. Other parameters are chosen randomly.
  


---

##### 5. Deep Discriminative Neural Networks

- CNNs
  - Definitions
    - Convolutional Layers
      
      - Extract the strongest value where the location in the input matches the weights in the kernel. 
      
        - A kernel in the size of $(w, w, c)$ has the weights of $w \cdot w \cdot c + 1$ (plus with the bias).
      
      - The mask defines the weights of a neuron. A neuron only receives input from a small region of the input array, called **receptive field (RF)**. 
      
      - A neuron can have **multi-channel** input (e.g., from different colour channels of an image). In this case the mask has multiple channels. 
      
        - i.e., A convolutional layer may have multiple kernels (masks) to handle different features. 
        - The output dimension of $D$ masks is $D$.
      
      - Components
      
        - Kernel
        - Padding
        - Stride
        - Dilation
      
      - Computation on the output size:
      
        - The output size for an input with the size $W \times W$ is as follows:
          $$
          W_{out} = \frac{W + 2P - K}{S} + 1
          $$
      
    - Pooling Layers
    
      - Pooling also called **sub-sampling** or **down-sampling**. 
      - CNNs contain pooling layers to increase tolerance to:
        - the location of patterns,
        - the configuration of sub-patterns.
      - Types
        - Max Pooling
        - Average Pooling
    
    - Fully Connected (FC) Layers
    
      - Typically, particularly when applied to classification tasks, the final layers of a CNN are **fully connected (FC) layers**. They work exactly the same as a traditional, feedforward, neural network. 
        
      - Flatten the input as 1 dimension.
        $$
        (W, W, C) \to (W \cdot W \cdot C, 1)
        $$
  
- Pratical Issues
  - Volume of Training Data
    - Data augmentation
    - Transfer Learning
  - Exploding & Vanishing Gradient
    - Use activation function: ReLU, LReLU, PReLU. 
    - **Gradient Clipping**: Set a threshold to prevent gradient exploding, clip the gradient if it is beyond the threshold. 
  - Instable BP
  
    - Better ways to initialize weights
  
    - Batch normalization
      $$
      BN(\textbf{x}) = \beta+\gamma \frac{\textbf{x} - E(\textbf{x})}{\sqrt{\text{Var}(\textbf{x}) + \epsilon}}
      $$
  
      - Batch normalization attempts to solve both these issues by scaling the output of each individual neuron so that it has a mean close to 0, and a standard deviation close to 1.
  
    - Adaptive Versions of Backpropagation
  
      - Momentum
      - Adaptive learning rate
  
        > e.g. 
        >
        > AdaGrad
        >
        > RMSprop
        >
        > ADAM
        >
        > ...
  
    - Skip connections (ResNet)
  
  - Fail to Generalization
    - Overfitting
      - Regularization 
        - L1 and L2 norm
        - Dropout
      - Early Stopping


---

##### 6. Deep Generative Neural Networks

- GANs

  - Components

    - **Generator**
      - Maps random noise $z$ to produce a meaningful sample.
      - If not well trained, the produced sample is a noise.
    - **Discriminator**
      - To decide if the sample $x$ is real or produced from the Generator (fake). 
      - The objective is to train the generator until the generated samples cannot be identified by the discriminator. 

  - Loss function: Cross Entropy
    $$
    V(D, G) = \mathbb{E}_{\textbf{x} \sim p_\text{data} (\textbf{x})} \underbrace{[ \log D(\textbf{x})]}_\text{for real samples} + \mathbb{E}_{\textbf{z} \sim p_\textbf{z}(\textbf{z})} \underbrace{[\log (1- D(G(\textbf{z}))]}_\text{ for fake samples } \implies \\
    V(D, G) = \sum_{\textbf{x} \in \mathcal{X}} [p_\text{data}(\textbf{x}) \log (D(\textbf{x})) + p_\text{gen}(\textbf{x}) \log(1 - D(\textbf{x})) ]
    $$
    where $\mathbb{E}(\cdot)$ denotes the expectation operator.
    
    > Note. Logrithm in this section is defined as $\log_e$. 
    
  - Training Process
  
    - Maximise $V(D, G)$, i.e., $\max_D V(D, G)$ by adjusting the parameter of $D$, $\theta_d$.
    - Minimize $V(D, G)$, i.e., $\min_G V(D, G)$ by adjusting the parameter of $G$, $\theta_g$. 
  
  - Optimality
  
    - Optimal discriminator is given by $D^*(\textbf{x}) = \frac{p_\text{data}(\textbf{x})}{p_\text{data}(\textbf{x}) + p_\text{gen}(\textbf{x})}$
    - If optimal generator is achieved, discriminator is given by $D(\textbf{x}) = \frac{p_\text{data}(\textbf{x})}{2 \cdot p_\text{data}(\textbf{x})} = \frac{1}{2}$ (i.e., $p^*_\text{gen}(\textbf{x}) = p_\text{data}(\textbf{x})$)
    - When both discriminator and generator achieves the optimal situation, we have the loss value $V(D^*, G^*) = -2\log2$
  
  - Problems
  
    - Non-convergence
    - Mode collapse
    - Diminished gradient (gradient saturation)
    - Unbalance between the generator and discriminator
    - Highly sensitive to hyperparameters
  
  - Solutions
  
    - Network Design: DCGAN (Deep convolutional generative adversarial networks)
    - Cost Function
      - One-sided label smoothing
      - Feature Matching
  
    - Optimization
      - Experience replay
      - Training with Labels
      - Adding noise (when training the Discriminator)
      - Unrolling GAN


---

##### 7. Feature Extraction

- Feature Engineering
  - Modify some measurements to make them more useful for classification.
  - Feature engineering can result in dimensionality **increase**: length($x$) ≥ length($m$)
- Feature Selection
  - Choose the most discriminative subset of data on which to perform the classification process.
  - Feature selection results in dimensionality **reduction**: length($m$) ≥ length($x$). 
  - Benefits
    - Reduces risk of overfitting.
    - Reduces training time (by reducing complexity of classifier).
- Feature Extraction Methods
  - **PCA**
    - *Reduce* the dimension. 
    - <u>orthogonal</u> directions in which the data <u>have the most variance</u>.
  - **Whitening**
    - *Reduce* the dimension. 
    - <u>orthogonal</u> directions with <u>equal</u> variance.
  - **Linear Discriminant Analysis (LDA)**
    - *Reduce* the dimension. 
    - directions in which the data from different classes is <u>most distinct</u>.
  - **Independent Component Analysis (ICA)**
    - *Reduce* the dimension. 
    - <u>statistically independent</u> directions.
  - **Random Projections**
    - a *high*-dimensional space with <u>random</u> directions.
  - **Sparse Coding**
    - a few, <u>non-random</u> directions in a *high*-dimensional space.

---

##### 8. Support Vector Machine

- Linear SVM
  - Support vector: exemplars that are the closest to the hyperplane. 
  - **Margin** between support and hyperplane: $\frac{1}{\parallel \textbf{w} \parallel}$
  - Output Categories
    
    - Samples are correctly classified: $\textbf{w}^T\textbf{x}_i + w_0 \geq 1$
    - Samples are misclassified:  $\textbf{w}^T\textbf{x}_i + w_0 \leq -1$
  - Objective: 
    $$
    \min_\textbf{w} J(\textbf{w}) = \frac{1}{2} \parallel \textbf{w} \parallel^2
    $$
    subject to $y_i(\textbf{w}^T \textbf{x}_i + w_0) \geq 1, i \in \{1, 2, ..., N\}$
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
    \lambda_i(y_i(\textbf{w}^T\textbf{x}_i + w_0) -1) = 0, i \in \{1, 2, ..., N \}
    $$
  
    - Support vectors: $\lambda_i > 0$
  
  - Hard classifier:
    $$
    f(\textbf{x}) = \text{sgn}(\textbf{w}^T\textbf{x} + w_0)
    $$
  
  - Soft classifier
    $$
    f(\textbf{x}) = h(\textbf{w}^T\textbf{x} + w_0)
    $$
  
- Soft-Margin SVM
  - Output Categories
    $$
    y_i(\textbf{w}^T\textbf{x}_i + w_0) \geq 1 - \xi_i
    $$
    where $\xi_i$ is known as *slack variable*. 
  
    - Samples fall outside the band and are correctly classified: $ξ_i = 0, y_i(\textbf{w}^T\textbf{x}_i + w_0) \geq 1$; 
    - Samples fall inside the band and are correctly classified: $0 < \xi_i \leq 1, 0 \leq y_i(\textbf{w}^T\textbf{x}_i + w_0) < 1$; 
    - Samples are misclassified: $\xi_i > 1, y_i(\textbf{w}^T\textbf{x}_i + w_0) < 0$.
    
  - Objective:
    $$
    \min_{\textbf{w}, w_0, \xi} J(\textbf{w}, \xi) = \frac{1}{2} \parallel \textbf{w} \parallel^2 + C \sum^N_{i=1} \xi_i
    $$
    subject to $y_i(\textbf{w}^T\textbf{x}_i + w_0) \geq 1 - \xi_i, \xi_i \geq 0$ and $i\in \{1, 2, ..., N\}$.
  
    - $\xi = \begin{bmatrix} \xi_1 &\xi_2 &... &\xi_N\end{bmatrix}$
    - $0 \leq C \leq + \infin$ is a pre-set constant scalar, which controls the influence of the two computing terms.
  
  - The linear SVM classifier (non-separable case) can be found by solving the solution $(\textbf{w}, w_0, ξ_i, λ_i, μ_i)$ to the following conditions:
    $$
    \begin{array}{lcl}
    \frac{\partial \mathcal{L}(\textbf{w}, w_0, \xi, \lambda, \mu)}{\partial \textbf{w}} = \textbf{0} &\implies & \textbf{w} = \sum^N_{i=1} \lambda_i y_i \textbf{x}_i\\
    \frac{\partial \mathcal{L}(\textbf{w}, w_0, \xi, \lambda, \mu)}{\partial w_0} = 0 &\implies &\sum^N_{i=1} \lambda_i y_i = 0 \\
    \frac{\partial \mathcal{L}(\textbf{w}, w_0, \xi, \lambda, \mu)}{\partial \xi_i} = 0 &\implies & C - \mu_i - \lambda_i = 0
    \end{array}
    $$
    where
    $$
    \mu_i, \lambda_i \geq 0, \\
    \mu_i\xi_i = 0, \\
    \lambda_i(y_i(\textbf{w}^T\textbf{x}_i + w_0) - 1 + \xi_i) = 0, i \in \{1, 2, ..., N\}
    $$
  
    - Hard classifier:
      $$
      f(\textbf{x}) = \text{sgn}(\textbf{w}^T\textbf{x} + w_0)
      $$
  
    - Soft classifier
      $$
      f(\textbf{x}) = h(\textbf{w}^T\textbf{x} + w_0)
      $$
  
- Nonlinear SVM
  - Kernel Trick
    - Simplify the computation. 
  
- Multiclass SVMS

  | Methods (for $R$-class classification) | Description                                                  | Train              | Inference          |
  | -------------------------------------- | ------------------------------------------------------------ | ------------------ | ------------------ |
  | One against one                        | Train binary classifier for each pair (Cls 1 vs. Cls 2, ...) | $\frac{R(R-1)}{2}$ | $\frac{R(R-1)}{2}$ |
  | One against all                        | Train binary classifier for each class (Cls vs. Non-Cls)     | $R$                | $R$                |
  | Decision Tree                          | Train binary classifier as nodes of decision tree            | $R-1$              | $⌈\log_2 R⌉$       |
  | Binary Encoded                         | Encode the classes, train binary classifier for each binary bit | $⌈\log_2 R⌉$       | $⌈\log_2 R⌉$       |
  

---

##### 9. Ensemble Methods

- Bagging
  - Definition
    - Construct multiple boostrap subset from the original dataset. Train multiple weak classifiers respectively. Taking the vote (for classification) or average (for regression) as the final output.
    - **Parallel** structure. 
  - Random Forest
    - A random forest is <u>an ensemble of bagged decision trees</u> with randomised feature selection.
      - Bagging: randomly drawn subset of the training data
      - Randomised feature selection: randomly drawn subset of features
- Boosting
  - Definition
    - Train a weak classifier first. Next, tweak the weights of samples basing on the error rate, giving a higher weights to the challenging samples. Accumulate multiple weak classifer until getting a strong classifer. 
    - **Serial** structure. 
    
  - AdaBoosting
    - Allows adding weak classifiers to the final classifier until a desirable classification accuracy is achieved.
    - In the sequential structure, while getting the classification results from the previous classifier, set a higher weight to the misclassified samples so the next classifer will pay more attention with them. 
- Stacked Generalization
  - In stacked generalisation, the overall classification is done in two levels. 
  - The input will be processed by a group of classifiers in the first level. The output from the first layer will go though the classifier in the second level to make the final decision.

---

##### 10. Clustering

- Algorithms
  - Iteratively Optimization
    - K-Means Clustering
      - Initialize $k$ clusters, put the datapoints into those clusters.
      - Each datapoint belongs to exactly one cluster.
    - Fuzzy K-Means Clustering
      - Compared to K-Means Clustering, each data point has a continuous belonging value regarding to each cluster. 
  - Hierarchical Clustering
    - Initialize every datapoint as a cluster, merge the clusters in each iteration. 
    - Metrices
      - **Single-link clustering**: distance between clusters is the **minimum distance** between constituent samples. 
      - **Complete-link clustering**: distance between clusters is the **maximum distance** between constituent samples.
      - **Group-average clustering**: distance between clusters is **average distance** between all pairs of samples.
      - Centroid clustering: distance between clusters is the **distance between the average of the feature vectors** in each cluster. 
