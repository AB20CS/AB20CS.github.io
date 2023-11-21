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

Let $$e_i$$ be the vertical distance from the line to the training point.

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

# Clustering and the K-Means Algorithm

Clustering is an **unsupervised** learning problem in which we seek to group our data into clusters based on similarity.

## K-Means Algorithm
*Adapted from https://towardsdatascience.com/k-means-clustering-introduction-to-machine-learning-algorithms-c96bf0d5d57a*

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