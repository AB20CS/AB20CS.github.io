---
layout: page
permalink: /intro-ml-tutorials/
title: Introduction to Machine Learning Tutorials
description: 
nav: false
nav_order:
toc:
  sidebar: left
---
On this page, you can find primers on introductory machine learning algorithms.

# Linear Regression

In linear regression, our goal is to simplify our data into a line that can capture the overall trend.

In other words, we would like to fit the following model onto our data:

$$y = wx + b$$

where $$w$$ is the weight and $$b$$ is the bias.

_To Determine:_ Estimate $$w$$ and $$b$$ from $$N$$ training pairs $$\{(x_i, y_i\}_{i=1}^N$$.

_Goal:_ With determined $$w$$ and $$b$$, we would like to calculate $$y$$ for some new inputs $$x$$.

## Derivation of Optimal Parameters

Let $$e_i$$ be the vertical distance from the line to the $$i^{\text{th}}$$ training point.

$$
\begin{equation}
e_i = y_i - (wx_i + b)
\end{equation}
$$


**Objective Function:**
We define our objective function $$E(w,b)$$ as follows:
$$
\begin{equation}
\begin{split}
    E(w,b) &= \sum_{i=1}^N e_i^2 \\
    &= \sum_{i=1}^N y_i - (wx_i + b)^2
\end{split}
\end{equation}
$$

We would like to minimize $$E(w,b)$$ in linear regression (i.e., we want the parameters when $$\nabla E(w,b)=0$$).

This means that $$\frac{\partial E}{\partial w} = 0$$ and $$\frac{\partial E}{\partial b} = 0$$.

Let us find $$b^*$$ and $$w^*$$ - the values of $$b$$ and $$w$$, respectively, that minimize $$E(w,b)$$.

**Find $$b^*$$:**

Let us first find an expression for $$\frac{\partial E}{\partial b}$$.

$$
\begin{equation}
\begin{split}
    \frac{\partial E}{\partial b} &= -2 \left( \sum_{i=1}^N y_i - (wx_i + b) \right) \\
    &= -2 \left( \sum_{i=1}^N y_i - wx_i - b \right) \\
    &= -2 \left( \sum_{i=1}^N y_i - \sum_{i=1}^N wx_i - \sum_{i=1}^N b \right) \\
    &= -2 \left( \sum_{i=1}^N y_i - w \sum_{i=1}^N x_i - \sum_{i=1}^N b \right) \\
    &= -2 \left( \sum_{i=1}^N y_i - w \sum_{i=1}^N x_i - Nb \right) \\
\end{split}
\end{equation}
$$

Setting $$\frac{\partial E}{\partial b} = 0$$, we get 
$$
\begin{equation}
\begin{split}
    b^* &= \frac{\sum_{i=1}^N y_i}{N} - \frac{w \sum_{i=1}^N x_i}{N} \\
    &= \hat{y} - w \hat{x}
\end{split}
\end{equation}
$$

where $$\hat{x}$$ and $$\hat{y}$$ are the averages of all $$x$$ and $$y$$ values, respectively.

**Find $$w^*$$:**

Now that we have an expression for $$b^*$$, we can re-write $$E(w,b)$$ as 
$$
\begin{equation}
\begin{split}
    E(w,b) = \sum_{i=1}^N ((y_i - \hat{y}) - w(x_i - \hat{x}))^2
\end{split}
\end{equation}
$$

Solving for $$\frac{\partial E}{\partial w}$$, we obtain the following

$$
\begin{equation}
\begin{split}
    \frac{\partial E}{\partial w} &= -2 \left( \sum_{i=1}^N ((y_i - \hat{y}) - w(x_i - \hat{x})) \right) \\
    &= -2 \left( \sum_{i=1}^N ((y_i - \hat{y})(x_i - \hat{x})) - w \sum_{i=1}^N (x_i - \hat{x})^2 \right) \\
\end{split}
\end{equation}
$$

Setting $$\frac{\partial E}{\partial w}=0$$, we get
$$
\begin{equation}
\begin{split}
    w^* &= \frac{\sum_{i=1}^N ((y_i - \hat{y})(x_i - \hat{x}))}{\sum_{i=1}^N (x_i - \hat{x})^2}
\end{split}
\end{equation}
$$

In this way, we can derive the optimal parameters for linear regression using least-squares and fit a line on our dataset.

# Non-Linear Regression
When the relation betewen inputs and outputs is no longer linear, linear regression is no longer appropriate.
We are still interested in a parameterized funtion in the form $$y=f(x)$$, except $$f(x)$$ is non-linear (in linear regression, $$f(x)=wx+b$$).

## Basis Function Regression
**1D Case:**
$$
\begin{equation}
\begin{split}
    y=f(x) = \sum_k w_k b_k(x)
\end{split}
\end{equation}
$$

where $$b_k(x)$$ are basis functions.

**Multi-Dimensional Case:**
$$
\begin{equation}
\begin{split}
    y=f(x) = \boldsymbol{b}^T \boldsymbol{w}
\end{split}
\end{equation}
$$

where $$\boldsymbol{b}(x) = [b_1(x), b_2(x), \dots, b_M(x)]^T$$ and $$\boldsymbol{w} = [w_1, w_2, \dots, w_M]^T$$.

**Choices of Basis Functions:**
- _Polynomials:_ common basis for polynomials is monomials

  $$b_0(x)=1, b_1(x)=x, b_2(x)=x^2, b_3(x)=x^3, \dots$$

  $$f(x) = \sum_k w_k x^k$$

- _Radial Basis Functions (RBF):_

  $$b_k(x) = \exp \left( -\frac{(x-c_k)^2}{2\sigma^2} \right)$$, where $$c_k$$ is the centre of the basis function $$b_k$$ and $$\sigma^2$$ determines its width.

  $$f(x) = \sum_k w_k b_k(x) = \sum_k w_k \exp \left( -\frac{(x-c_k)^2}{2\sigma^2} \right)$$

**Fitting the Model:**

To fit the model, we use least squares regression, minimizing the sum of squared residual error between model predictions and given training points.

$$
\begin{equation}
\begin{split}
    E(\boldsymbol{w}) = \sum_i (y_i-f(x_i))^2 = \sum_i \left( y_i - \sum_k w_k b_k(x) \right)^2
\end{split}
\end{equation}
$$

We can re-write this in matrix-vector form as follows:

$$
\begin{equation}
\begin{split}
    E(\boldsymbol{w}) = \lVert \boldsymbol{y} - \boldsymbol{B}\boldsymbol{w} \rVert
\end{split}
\end{equation}
$$

where $$\boldsymbol{y} = [y_1, y_2, \dots, y_M]^T$$, $$\boldsymbol{B}_{ij} = b_j(x_i)$$, and $$\boldsymbol{w} = [w_1, w_2, \dots, w_M]^T$$.  

**Determining Parameters:**

*Picking basis centres $$c_k$$:*
- pick uniformly spaced centres across region containing data
  - Advantage: simple to implement
  - Disadvantage: can lead to empty regions with basis functions, impractical number of data points in higher dimensional input spaces
- pick on centre per data point
  - Advantage: limits number of centres needed
  - Disadvantage: can be expensive if number of data points is larger
- cluster the data and use one centre for each cluster

*Picking the width $$\sigma^2$$*:
- trial-and-error
- use average squared distances (or median distances) to neighbouring centres, scaled by a constant

## Overfitting and Regularization

**Overfitting** is the phenomenon in which we fit the training data extremely well yet obtain a model that does not perform well on data that differs form its training inputs. To avoid overfitting, we can add an extra term to the learning objective function. This is called **regularization**.

$$
\begin{equation}
\begin{split}
    E(\boldsymbol{w}) = \lVert \boldsymbol{y} - \boldsymbol{B}\boldsymbol{w} \rVert + \lambda \lVert \boldsymbol{w} \rVert^2
\end{split}
\end{equation}
$$

In the sum above, $$\lVert \boldsymbol{y} - \boldsymbol{B}\boldsymbol{w} \rVert$$ is the "data term" which measures how well the model fitst the training data. $$\lambda \lVert \boldsymbol{w} \rVert^2$$ is the "smoothness term" whcih penalizes non-smoothness, preventing overfitting (note: $$\lambda$$ is a constant).

**Finding the Optimal Weight $$\boldsymbol{w^*}$$:**
$$
\begin{equation}
\begin{split}
    E(\boldsymbol{w}) &= \lVert \boldsymbol{y} - \boldsymbol{B}\boldsymbol{w} \rVert + \lambda \lVert \boldsymbol{w} \rVert^2 \\
    &= (\boldsymbol{y} - \boldsymbol{B}\boldsymbol{w})^T(\boldsymbol{y} - \boldsymbol{B}\boldsymbol{w}) + \lambda \boldsymbol{w}^T \boldsymbol{w} \\
    &= \boldsymbol{w}^T\boldsymbol{B}^T\boldsymbol{B}\boldsymbol{w} - 2\boldsymbol{w}^T\boldsymbol{B}^T\boldsymbol{y} + \boldsymbol{y}^T\boldsymbol{y} + \lambda \boldsymbol{w}^T\boldsymbol{w} \\
    &= \boldsymbol{w}^T(\boldsymbol{B}^T\boldsymbol{B} + \lambda I)\boldsymbol{w} - 2\boldsymbol{w}^T\boldsymbol{B}^T\boldsymbol{y} + \boldsymbol{y}^T\boldsymbol{y}
\end{split}
\end{equation}
$$

Setting $$\nabla E(\boldsymbol{w}=0)$$, we get
$$
\begin{equation}
\begin{split}
    w^* = (\boldsymbol{B}^T\boldsymbol{B} + \lambda I)^{-1}\boldsymbol{B}^T\boldsymbol{y}
\end{split}
\end{equation}
$$

In this way, we can modify our expression for $$\boldsymbol{w^*}$$ to avoid overfitting.

# Class Conditionals

Class conditionals are a type of generative models used for classification. We will focus on the binary classification task, but it can be applied to any number of classes.

With binary classification, suppose that we have 2 mutually exclusive classes $$c_1$$ and $$c_2$$.

_Some important terms:_
- *Prior* probability of a data vector coming from class $$c_1$$: $$P(c_1) \equiv P(y=c_1)$$.
- *Prior* probability of a data vector coming from class $$c_2$$: $$P(c_2) \equiv P(y=c_1) = 1-P(c_1)$$.
- *Data likelihood distributions*: $$p(x \vert c_1)$$ and $$p(x \vert c_2)$$
- *Posterior distributions*:  $$P(c_1 \vert x)$$ and $$P(c_2 \vert x)$$

The probability of a data point $$x$$ is given as 
$$
\begin{equation}
\begin{split}
    p(x) &= p(x,c_1) + p(x,c_2)\\
    &= p(x \vert c_1)P(c_1) + p(x \vert c_2)P(c_2) 
\end{split}
\end{equation}
$$

**Learning Goal:**
1. estimate likelihood distributions  $$p(x \vert c_1)$$ and $$p(x \vert c_2)$$
2. estimate $$P(c_1)$$ by computing the ratio of number of elements of class 1 to the total number of elements

We can perform classifications by comparing the posterior class (i.e., check whether $$P(c_1 \vert x) > P(c_2 \vert x)$$). Equivalently, if $$\frac{P(c_1 \vert x)}{P(c_2 \vert x)}>1$$, then $$x$$ belongs to class 1. Otherwise $$x$$ belongs to class 2.

The posteriors can be computed using Bayes' Rule as: $$P(c_i \vert x) = \frac{p(x \vert c_i)P(c_i)}{p(x)}$$.

Hence, the ratio above (that is compared to 1) is equivalent to $$\frac{p(x \vert c_1)P(c_1)}{p(x \vert c_2)P(c_2)}>1$$.

These computations are typically done in log scale as it is faster and more numerically stable. Thus, instead of comparing the ratio to 1, we can compare the $$a(x)$$ to 0, where
$$
\begin{equation}
  a(x) = \log \left( \frac{p(x \vert c_1)P(c_1)}{p(x \vert c_2)P(c_2)} \right)
\end{equation}
$$
If $$a(x)>0$$, then $$x$$ belongs to class 1. Otherwise, $$x$$ belongs to class 2.

## Gaussian Class Conditionals (GCC)

With GCC, the likelihood distributions are modeled with multi-dimensional Gaussian distributions. In other words,

$$p(x \vert c_i) = \mathcal{N}(x; \mu_i, \Sigma_i)$$

We assume that the prior class probabilities are equal ($$P(c_1) = P(c_2) = \frac{1}{2}$$). The values of $$\mu_i$$ and $$\Sigma_i$$ can be estimated by maximum likelihood estimation (MLE) on the individual classes in the training data.

With this set-up, we define our decision boundary as 
$$
\begin{equation}
  a(x) = -\frac{1}{2}(x-\mu_1)^T\Sigma_1^{-1}(x-\mu_1) - \frac{1}{2}\ln \vert \Sigma_1 \vert + \frac{1}{2}(x-\mu_2)^T\Sigma_2^{-1}(x-\mu_2) + \frac{1}{2}\ln \vert \Sigma_2 \vert
\end{equation}
$$

*Interesting property:* The decision boundary $$a(x)$$ is linear when $$\Sigma_1 = \Sigma_2$$.

# Clustering and the K-Means Algorithm

Clustering is an **unsupervised** learning problem in which we seek to group our data into clusters based on similarity.

## K-Means Algorithm
*Adapted from [Rohith Gandhi's notes](https://towardsdatascience.com/k-means-clustering-introduction-to-machine-learning-algorithms-c96bf0d5d57a)*

1. Choose the number of clusters, $$K$$, and obtain the data points.
2. Initialize the centroids $$c_1, c_2, ..., c_K$$. This can be done in various ways (e.g., random, $$K$$-medoids, multiple restarts).
3. Repeat until convergence or fixed number of iterations:
    - for each data point $$x_i$$: `# update cluster labels`
        - find the nearest centroid $$c_j$$ to $$x_i$$
        - assign point $$x_i$$ to the cluster represented by the centroid $$c_j$$
    - for each cluster $$j = 1, \dots, K$$:`# update centroid locations`
        - $$c_j$$ = mean of all points assigned to cluster $$j$$

## "Elbow" Method
**Problem:** What if the number of clusters was not obvious? How would we initialize the number of centroids?
**Solution:** We can calculate the **Within-Cluster Sum of Squares (WCSS)** for every number of clusters we would like to examine. Then, we could plot these WCSS values. The number of clusters at which the plot starts to plateau (looks like an "elbow") is the optimal number of clusters according to this metric. We define WCSS as follows:

$$
\begin{equation}
  WCSS = \sum_{C_k}^{C_n} \sum_{d_i \text{ in } C_k}^{d_m} \text{distance}(d_i, C_k)^2
\end{equation}
$$

where $$C_k$$ is the $$i^{\text{th}}$$ centroid and $$d_i$$ is $$i^{\text{th}}$$ data point in the centroid $$C_k$$.

In the `KMeans` class within Scikit-Learn, the attribute `inertia_` stores the WCSS.

# Cross-Validation
Cross-validation is an empirical approach for evaluating a model's performance by dividing the dataset into several parts, using different parts for testing and training in successive iterations.

**Cross-validation can be used for:**
- Mitigating the risk of overfitting
- Comparing different models and selecting the one performing best on average
- Determining appropriate hyperparameter values

**Disadvantages:**
- Can be computationally expensive
- Might not be appropriate for certain data sets

**$$N$$-Fold Cross-Validation Algorithm**
1. Split dataset into $$N$$ equal partitions
2. Use one fold for testing and the other $$N-1$$ folds for training
3. Calculate performance on the test set
4. Repeat Steps 2 and 3 $$N$$ times
5. Use average accuracy as an estimate of model's performance

When $$N=M-1$$, where $$M$$ is the number of data points, this approach is called **leave-one-out cross-validation**.