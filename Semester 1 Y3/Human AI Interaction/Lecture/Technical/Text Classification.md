Documents in our corpus are represented by the TF-IDF weighting of their positive and negative words yielding a representation in 2-dimensional vector space, after preprocessing and representation. The sentiment classes are positive, and negative. 
$$
\begin{array}{c}
P_{1}[3,6] \to positive  \\
P_{2}[1.5, 6] \to positive\\
P_{3}[2,4] \to positive\\
P_{4}[4.5,2] \to negative \\
P_{5}[5.5, 3] \to negative \\
P_{6}[6.5, 4.5] \to negative
\end{array}
$$
![[Pasted image 20251105094517.png]]
How can we separate these two classes, and classify documents positive or negative based on their features?

## Classifying Text
The goal is to find a rule or model which can separate classes and generalise. 

Several classifier types exist.

### Memorisation
Stores exact examples, perfect on training data and is fast, but obviously has the inability to generalise to unseen instances. 
### OneR
One derives a simple rule, e.g., if $x \geq 3$ then negative, otherwise positive. This is easily interpretable and can generalise(although very poorly.)
### Linear Classifier
One draws a line to separate classes e.g., $y = 1.5 \cdot x$. This is very fast and generalises well, though one is limited to linear separability. 
This can be represented in $n$ dimensions via $$y = \theta_{0} + \sum_{i=1}^n\theta_{i}x_{i}$$
This states that the output y is a weighted sum of the input features plus a bias term $\theta_{0}$.

Yields a plane or hyperplane in higher dimensions. 
### Nearest Neighbour Classifier
Classify based on neighbourhood. New data is classified based on closest neighbouring points. Low level of generalisation error, and speed depends on the model. 
![[Pasted image 20251105100752.png]]

### Decision Tree classifier
Splits data by thresholds, and can be efficient, however can overfit![[Pasted image 20251105100837.png]]

### Classification as generalisation
Core assumption is that our data is representative of the real world. Generalisation challenges include things such as vocabulary drift, temporal shifts in data, and geographical, linguistic, or cultural differences. We never observe the real world - only a sample of it. One must ensure that model assumptions match reality.

## Machine learning
About learning from examples - automatically discovering patterns in data that can therefore generalise to new, unseen data. In text classification, ML models learn decision boundaries that separate categories such as:
- *positive vs negative sentiment*
- *spam vs non-spam*
- *intent type*
Replaces hand-crafted rules with data-driven learning. 

### Training, testing, and generalisation
Three types of error are introduced
1. Training error - how well the model fits the training data
2. Testing error - how well the model performs on unseen testing data
3. Generalisation error - how well the model performs in the real world
The goal is a low generalisation error, and avoiding overfitting. We cannot really measure the generalisation error, but we use the training and testing error and some clever experiment design to estimate it.
- Train/test split - reserve part of the data for testing only
- Cross-Validation - repeatedly train/test on different partitions to get a stable estimate
- Bootstrapping - sample data with replacement to create multiple training/test sets
### Types of ML models for text classifications
#### Traditional ML models
These require manual feature extraction e.g., Bag of Words, TF-IDF
- **Linear models** - Logistic regression
- **Tree models** - Decision Trees
- **Probabilistic models** - Naive bayes, Bayesian Networks
- **K-Nearest Neighbour** - K-Nearest Neighbour
Representation is explicitly defined by user and fed to the model, and thus reliant on good text representation.

#### Deep Learning Models
These learn representations automatically through multiple layers:
- **Perceptron/Multi-Layer Perceptron**
- **Convolutional Neural Networks**
- **Recurrent Neural Networks**
- **Transformers**

### ML approaches
- **Supervised** - there is known ground truth about the training data i.e., each data point has a label associated
- **Unsupervised** - there is no known ground truth about the data i.e., no labels associated to data points
- **Semi-Supervised** - there is ground truth about some of the data i.e., some data points have labels, others do not.
- **Reinforcement Learning** - 

### Learning and Optimisation


