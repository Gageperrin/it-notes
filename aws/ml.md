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

k-NN is used for classification and regression tasks.


### Transformations
* Logarithmic transformations conform positively skewed data to normally distributed data.
* Laplace transformations simplify complex differential equations into algebraic equations. Useful for digital signal processing.
* Exponential transformations compress a distribution toward zero. This reduces the influence of extreme values. It can also make variables more human-readable.
* Third-order polynomial transformations are preferable in fixing negatively skewed data or when capturing more complex patterns in data (that linear or quadratic transformations do not display). Third-order polynomial transformations can lead to overfitting.