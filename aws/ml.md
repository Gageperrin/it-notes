# AWS Certified Machine Learning Specialty

These notes are taken from conversations from ChatGPT. By posing a series of carefully formulated questions, as if I were talking to a Machine Learning expert, I learned all of the following about the concepts and practices of machine learning.

Machine learning falls into three categories:
1. Supervised Learning
2. Unsupervised Learning
3. Reinforcement Learning

### Supervised Learning

Training a model on a labeled dataset. Each provided example has a target value. Eventually, the model should predict target values for new examples. Most used for classification and regression.

Common techniques:
* Linear regression
* Logistic regression
* Decision trees
* Random forests
* Support vector machines (SVMs)
* Naive Bayes
* k-Nearest Neighbors (k-NN)
* Neural networks

### Unsupservised Learning

Training a model on an unlabele dataset. There are no target values assigned. The model must discover patterns or structure without prior knowledge. Used for clustering, dimensionality reduction, and anomaly detection.

Common techniques:
* K-means clustering
* Hierarchical clustering
* Principal Component Analysis (PCA)
* Independent Component Analysis (ICA)
* t-SNE
* Autoencoders

### Reinforcement Learning

Training a model to make decisions based on environment feedback. Maximize reward signal over time. Used for game simulations, robotics, and control systems.

* Q-learning
* SARSA
* Deep reinforcement learning

#### Linear Regression

For predicting continuous values. Use a dataset with linked dependent and independent variables. Find the coefficients of independent variables to minimize the difference between teh predicted and actual value for dependent variables.

This is a baseline method for comparing against more complex ML algorithms.

#### Logistic Regression

Predicts probability. Use for classification tasks in which the variables are binary or categorical. 

The goal is to find the coefficients that maximize the likelihood of the observed data given the model, using maximum likelihood estimation. Most useful for non-linear relationships. 

Robust against outliers and high-dimensional data.

Weak when dealing with non-linear relationships, imbalanced classes or when the decision boundary is itself nonlinear. Can also cause overfitting.

#### Decision Trees

For regression and classification tasks. Each node in the tree represents a decision based on a feature or attribute. Each leaf represents a predicted value or class label. The goal is to maximize the homogeneity of each subset. Partitioning continues until it reaches a maximum depth.

Strengths: human readable, handling nonlinear relationships

Weaknesses: Overfitting (especially when tree has large depth) and unstable.

Overfitting can be offset by using regularization techniques such as pruning (setting a minimum number of samples per leaf) or setting a maximum depth.

Instability can be offset by ensemble methods such as random forests or gradient boosting.

#### Random Forests
Ensemble learning method based on decision trees. Multiple decisions trees are trained on different subsets of the training data and then combining the predictions of all trees. Final prediction is a majority vote.

Strengths: Less prone to overfitting, more accurate in high-dimensional spaces. Handles missing values or noisy data. Easy to use and minimal tuning required.

Weaknesses: Computationally expensive, especially with large datasets. Difficult to interpret or visualize. Not as high performant as gradient boosting in some scenarios.

#### Support Vector Machines (SVM)

Used for classification and regression tasks. Useful when data is nonlinearly separable or there are complex decision boundaries.

Find a hyperplane to discriminate data into separate classes while maximizing the margin between the hyperplane and the closest points in each class (i.e. the support vectors). By maximizing the margin, SVMs aim to find the decision boundary that is least likely to generalize poorly for new data.

In linearly separable cases, the hyperplane is a line that separates two classes. In non-linear cases, SVMs use kernel functions to transform data into a higher-dimensional space. Most common SVM kernel functions include radial basis function (RBF) and the polynomial kernel.

Strengths: Handle high-dimensional data, effective in small sample sizes, and strong theoretical guarantees for generalization. Useful for multi-class classification and can handle unbalanced datasets.

Weaknesses: Sensitive to the choice of kernel and kernel parameters which are difficult to tune. Computationally expensive. Not easily interpretable.

#### Naive Bayes Classifiers

Bayes' Theorem describes the probability of a hypothesis given evidence. Naive Bayes is used for NLP and text classification.

It is "naive" because it assumes that features are conditionally independent given a class label. This simplifies the calculation of probabilities and makes the algorithm efficient and easy to implement.

It first calculcates the posterior probability of each class given the input features, then it selects the class with the highest probability.

Three types of Naive Bayes classifiers:
* Gaussian Naive Bayes - Continuous input features with normal distribution.
* Multinomial Naive Bayes - Discrete input features
* Bernoulli Naive Bayes - Discrete input features

Strengths: Computationally efficient and only a small amount of training data needed. Handles high-dimensional data and is robust to irrelevant input features.

Weaknesses: The independence assumption can hamper its accuracy. Sensitive to the presence of irrelevant features and can lead to overfitting. Not well-suited for non-linear relationships between input features and output classes

#### k-Nearest Neighbors (k-NN)

k-NN is used for classification and regression tasks. It is a type of instance based learning. It does not map inputs to outputs but memorizes the training data to make predictions for new inputs.

It finds the training examples closest to the input, uses a distance metric to identify the gap, and then takes a majority vote among the k-nearest neighbors for the  prediction.

Strengths: Non-parametric and therefore no particular functional form between input and output variables. Very flexible and fits complex decision boundaries.

Weaknesses: Computationally expensive, careful choice of distance metric and k-value to avoid under or overfitting.

#### Neural Networks

Used for complex problems with high-dimensional input such as image or speech. An artificial neuron takes in a set of inputs, performs a weighted sum, then applies an activation function to produce an output. In a neural network, layers of neurons are built on top of each other. The first layer of the network is the input layer where the input variables are inserted. The output of the first layer is inserted into the next layer, and son on. The final layer corresponds to the output variables.

During training, neural networks learn the weight of connections between neurons by minimizing a loss function which measures the difference between predicted and true output. 

Strengths: Handles complex, non-linear, high-dimensional data.

Weaknesses: Computationally expensive, careful selection of hyperparameters, and prone to overfitting if not properly regularized.


### Transformations
* Logarithmic transformations conform positively skewed data to normally distributed data.
* Laplace transformations simplify complex differential equations into algebraic equations. Useful for digital signal processing.
* Exponential transformations compress a distribution toward zero. This reduces the influence of extreme values. It can also make variables more human-readable.
* Third-order polynomial transformations are preferable in fixing negatively skewed data or when capturing more complex patterns in data (that linear or quadratic transformations do not display). Third-order polynomial transformations can lead to overfitting.


### Unsupervised Learning

#### K-means clustering

Used for image segmentation, document clustering, and market segmentation.

Partitions a set of data points into k clusters, based on similarity. It iteratively updates the positions of k centroids until convergence is reached.

This algorithm is sensitive to the initial choice of centroids, which are often chosen randomly. The algorithm should be run multiple times with varying initializations to choose the solution with the lowest total within-cluster sum of squares (WCSS).

Strengths: Structure in unlabeled data

Weaknesses: Sensitive to outliers and assumes clusters are spherical with equal variance.

#### Hierarchical clustering

Used for image segmentation, document clustering, gene analysis, and market segmentation.

Unlike K-means clustering it does not require a user-specified number of clusters to generated. It builds a hierarchy of clusters visualized as a dendrogram.

Hierarchical clustering can be agglomerative or divisive. Agglomerative starts with each data point as its own cluster then merges the two closest clusters at each iteration until there is a single encompassing cluster. Divisive clustering starts with a single cluster and then splits the cluster into two iteratively until each data point has its own cluster.

The distance between clusters can be measured using a variety of distance metrics such as Euclidean distance, Manhattan distance, or correlation distnace. The linkage criterion (single, complete, or average) determines hwo the distance between clusters is calculated.

The user can choose what layer to cut the dendrogram at in order to obtain a specific number of clusters.

Strengths: Can handle any number of clusters and visually represent it.

Weaknesses: Computationally expensive and can suffer from chaining (clusters merge on a single outlier)

#### Principal Component Analysis (PCA)

Used for data visualization, feature extraction, and noise reduction.

Reduces the dimensionality of a dataset by identifying correlations between variables. It transforms the data into a new coordinate system so that the first axis or principal component captures the most significant variation in the data.

Strengths: Dimensionality reduction, data compression, and feature extraction
Weaknesses: Does not guarantee interpretability, assumes linearity, sensitive to outliers, and sensitive to scaling.

#### Independent Component Analysis (ICA)

Used for signal or image processing.

Separates a multivariate signal into underlying independent components. It identifies a set of independent components that can be combined linearly to produce the observed data. While PCA finds an orthogonal set of components to capture maximum variance, ICA finds statistically independent and non-Gaussian components.

Strengths: Separate sources, no assumption of linearity, and robustness to outliers
Weaknesses: Requires assumptions for identifiability, Computationally expensive, and no guarantee of optimality.

#### t-Distributed Stochastic Neighbor Embedding (t-SNE)

t-SNE is a nonlinear dimensionality reduction technique for visualizing high-dimensional data in two or three dimensions. It minimizes the divergence between a Gaussian distribution and t-distribution. The Gaussian distribution measures the similarity between high-dimensional data points while t-distribution does the same for low-dimensional data points.

Strengths: Visualization of high-dimensional data, preserves local structure, robust for different data types (e.g. continuous, discrete, categorical)
Weaknesses: Computationally expensive, no guarantee of global structure preservation, sensitive to hyperparameters

#### Autoencoders

Used for dimensionality reduction, feature learning, data compression, and data denoising.

It learns a compressed representation of the input data and then reconstructs the original input with minimal loss. Autoencoders consist of an encoder network that maps the input data to a lower-dimensional latent representation, and a decoder network that maps the latent representation back to the original input space.

Strengths: does not require labeled data, nonlinear mapping, regularization to prevent overfitting
Weaknesses: difficult to interpret, sensitivity to initialization, and computational complexity



### Reinforcement Learning

#### Q-learning

Used for robotics, game playing, and control systems.

Learns an optimal policy for an agent in an unknown dynamic environment.

The agent iteratively updates its estimates of the action-value function using the Bellman equation which expresses the expected reward for taking an action in a state as the sum of the immediate reward and the discounted expected reward for the next state.

Strengths: Model-free learning, handles continuous and discrete state and action spaces
Weaknesses: Slow convergence (computationally expensive), sensitive to hyperparameters, and limited to single-agent environments

#### State-Action-Reward-State-Action (SARSA)

Similar to Q-learning but instead of following the optimal policy, it estimates the expected reward for a particular action, and follows the particular policy thereafter. It iteratively updates its estimates using the SARSA update rule. The agent selects actions according to an epsilon-greedy policy.

Strengths: Converges under certain conditions, handles continuous and discrete state and action spaces, more conservative than Q-learning
Weaknesses: Can converge slowly, limited to single-agent environments, and sensitive to hyperparameters

#### Deep reinforcement learning

Used for raw sensory input such as images or audio to map it to actions in an environment. Robotics, games, and NLP.

Strengths: Learn complex and abstract representations of an environment
Limitations: Computationally expensive, unstable training process, and difficult to interpret policies. Overfitting and sample inefficiency are also common problems.

* Q-learning
* SARSA
* Deep reinforcement learning


## Feature Engineering

* Standard scaling transforms continuous features by centering their mean around zero and scaling them toward a unit variance. This helps ensure that no single feature dominates the others due to scale differences.
* One-hot encoding turns a categorical feature into a binary feature. This helps logistic regression algorithms to use continuous features on categorical features.
* Row normalization scales the values of each row to have a unit length. Commonly used for text analysis applications, not regression.
* Ordinal encoding is used to create an order for categorical features.
* Max absolute scaler scales data based on the maximum absolute value.

## SageMaker Linear Learner Algorithm

Three data channels: `train`, `validation`, and `test`.

Regression: set `predictor_type` to `regression` where `score` is the produced prediction.
Classification: `binary_classifier` or `multiclass_classifier` which returns a `score` or a `predicted_label`.
