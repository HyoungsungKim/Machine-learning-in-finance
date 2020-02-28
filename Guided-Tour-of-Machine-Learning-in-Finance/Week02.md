# Week02

## Generalization and a Bias-Variance Tradeoff

### Generalization and a Bias-Variance Tradeoff

Generalization

- The algorithm learns useful information from available data in order to perform well on the new unseen data.
- This ability to perform well on a new data is called ***generalization.***
  - This ability is an ultimate goal of learning in the context of machine learning

### Bias-variance decomposition

$$
\begin{align}
\mathbb{E}[\left ( y-\hat{f}(x) \right )^2]  & = \mathbb{E}[y^2] + \mathbb{E}[\hat{f}^2] - 2\mathbb{E}[y \hat{f}] \\
& = Var(y) + \mathbb{E}[y]^2 + Var(\hat{f}) + \mathbb{E}[\hat{f}]^2 - 2 \mathbb{E}[y \hat{f}] \\
& = \mathbb{E}[f - \hat{f}]^2 + Var(\hat{f}) + Var(y) \\
& = (bias)^2 + variance + noise \\
\end{align}
$$

- Loss가 고정되어 있을 때 bias variance를 동시에 낮추거나 올릴수 없음
- 둘다 낮추려면 loss를 낮춰야 함
- $$y = f(x) + \epsilon$$
- $$\mathbb{E}[\epsilon] = 0, \mathbb{E}[\epsilon ^2] = \sigma ^ 2$$
- $$(bias)^2 = \mathbb{E}[f - \hat{f}]^2$$
  - Bias : The (square of) expected difference of approximate predictor $$\hat{f}(x)$$ from the "true" predictor $f(x)$ 
  - 정답과 멀리 떨어져 있는 경우 bias가 크다고 함
- $$variance = Var(\hat{f})$$
  - Variance : Sensitivity of $$\hat{f}(x)$$ to the choice of data set
  - 예측 값들의 집합이 한 영역에 몰려있으면 variance가 작다고 함
- $$noise = Var(y) = \sigma ^2$$
  - Noise : A property of data; beyond our control
- The relation is mostly of a theoretical value : we do not know how to compute the bias and variance
- But it shows a general decomposition of expected loss of a generic regression model
- There is a general trend for machine learning algorithms to have a kind of strong negative correlation between the bias and the variance.
  - This is called the Bias-Variance, the Bias-Variance trade-off
  - To reduce bias, you might be willing to consider more complex models that incorporate more features
  - Complex models (more features) tend to have low bias and high variance
  - Simple models (less features) tend to have high bias and low variance
- Important to control model complexity to have an optimal tradeoff

### Overfitting and Model Capacity

Key assumption of data set

- All pairs of rows and Y are i.i.d
- I.i.d. sampled from a data generating distribution data

### Regularization, Validation Set, and Hyper-parameters

#### Regularization

The main idea is to modify the objective function by adding to the function of model parameters in the hope that a new function will produce a model with smaller variance.

- The idea of regularization is to modify the objective function of minimization of MSE so that MSE has a smaller variance

$$
J(W) = MSE_{train}(W)  + \lambda\Omega(W)
$$

- $$J(W)$$ : Regularized loss function
- $$MSE_{train}(W)$$ : MSE train loss
- $$\lambda $$ : Regularization parameter
  - IF lambda approach zero, we are back to the initial case without any regularization
- $$\Omega(W)$$ : Regularizer

popular choices for the regularizer:

- $$L_2$$ regularization $$\Omega(W) = W^TW = ||W||_2$$
  - It penalizes large weights
- $$L_1$$ regularization(It is called as LASSO regularization) $$\Sigma(W) = \sum_i |W_i| = ||W||_1$$
  - Such regularization enforces sparsity of the solution
- Entropy regularization : $$\Sigma(W) = \sum_i W_i logW_i $$
  - $$(W_i \ge 0, \sum_i W_i = 1)$$

#### Hyperparameters and Validation Set

- Hyperparameters : Hyperparameters are any quantitative features of ML models that are not directly optimized by minimizing in-sample loss such MSE
  - Backpropagation으로 최적화 되지 않는 변수들
  - Hyperparameters control model capacity(Model complexity)
  - Examples of hyperparameters in linear regression
    - Degree of a polynomial regression(linear, quadratic, cubic, etc.)
    - Regularization parameter $$\lambda$$
    - Number of levels in a decision tree
    - Number of layers and nodes per layer in neural networks
    - Learning rate
    - etc...
  - How to choose hyperparameters:
    - Split a training set into training and validation sets(e.g. as 80:20)
    - Use cross-validation

#### Cross-Validation

- Assume we are given N samples, but N is small, so setting aside a fixed test set is problematic
- We want to use all samples for training
- This is achieved using cross-validation

