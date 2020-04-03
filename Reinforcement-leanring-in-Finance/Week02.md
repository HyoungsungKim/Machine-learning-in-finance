# Week02

## MDP Formulation

### Markov Decision Process Model for Discrete-time BS Model

The problem is then solved by a sequential maximization of "reward" (negatives of hedge portfolio one-step variance times the risk-aversion $$\lambda$$, plus a drift term).

Two cases are possible

- The model is known
  - In this case, We can use methods of dynamic programming(DP) or model based reinforcement learning
- The model is unknown
  - We can use model free reinforcement learning methods that rely on samples of data rather than on the model of the world

Respectively, we will have two ways to solve a Bellman equation for these two cases.

### State Variables

We first define a new variable $$X_t$$ by the following relation:
$$
X_t = -(\mu - \frac{\sigma^2}{2})t + logS_t
$$

- $$S_t$$ : Stock price

This implies
$$
dX_t = -(\mu - \frac{\sigma^2}{2})dt + d\,logS_t = \sigma dW_t
$$

- $$S_t$$ is just a re-scaled standard Brownian motion, scales by volatility $$\sigma$$.

For a given $$X_t$$ in a MC scenario, the correspoding value of $$S_t$$ is
$$
S_t = e^{X_t + (\mu-\frac{sigma^2}{2})t}
$$

- As $$X_t$$ is a martingale, i.e. $$\mathbb{E}[dX_t] = 0$$, on average it should not run too far away from $$X_0$$ during the lifetime of an option. The state variable $$X_t$$ is the time-uniform, unlike the stock price $$S_t$$ that has a drift
- The state variable $$X_t$$ is the time-uniform, unlike the stock price $$S_t$$ that has a drift
  - $$X_t$$ 는 시간에 상관없이 균등(Uniform)함

### Finite-state Approximation

While continuous state formulation is more practically irrelevant, a discrete state formulation is easier to understand.

Simplifications due to discretization:

- Can use simple algorithms for discrete-state MDPs(such as Q-learning)
- Discrete state formulation simplifies calculation of one-step conditional expectations

### Value Function

Now we reformulate our risk minimization procedure in a language of MDP problems.

Actions $$u_t = u_t(S_t)$$
$$
u_t(S_t) = a_t(X_t(S_t)) = a_t(logS_t - (\mu - \frac{\sigma^2}{2})t)
$$

### Bellman Equation

Reward $$R(X_t, a_t, X_{t+1})$$ is a one-step time-dependent random reward
$$
R_t(X_t, a_t, X_{t+1}) = \gamma a_t \Delta S_t(X_t, X_{t+1}) - \lambda Var_t[\Pi_t]
$$

- There are 2 contributions to the instantaneous reward $$R_t$$
  - Proportional to the change of stock price multiplied by the position in the stock
  - Negative risk at timestep T as measured by the variance of the hedge portfolio at time T
- ***In the limit of lambda going to zero, the hedging and pricing problems become decoupled***
  - The link between the price and hedge is lost
- If lambda=0 and delta t = 0, then risk is lost two, nothing to optimize any more. Back to the BS formula

### Quadratic Q-function

Q-function
$$
Q_t(x,a) = \mathbb{E}_t[R_t(X_t, a_t, X_{t+1}) + \gamma Q_{t+1}(X_{t+1}, a_{t+1})|x,a]
$$
Optimal
$$
Q^*_t(X_t, a_t) = \gamma \mathbb{E}_t[Q^*_{t+1}(X_{t+1}, a^*_{t+1}) + a_t \Delta S_t] - \lambda \gamma^2 \mathbb{E}_t [\hat{\Pi}^2_{t+1}-2a_t\hat{\Pi}_{t+1} \hat{S}\Delta_t + a^2_t(\Delta \hat{S}_t)^2]
$$
Optimal action
$$
a^*_t(X_t) = \frac{\mathbb{E}_t[\Delta \hat{S}_t \hat{\Pi}_{t+1} + \frac{1}{2\gamma \lambda}\Delta S_t]}{\mathbb{E}_t[(\Delta \hat{S_t})^2]}
$$
In a discrete-time BSM model, we had pure risk-based hedges:
$$
u^*_t(S_t) = \frac{Cov(\Pi_{t+1},\Delta S_t |\mathcal{F})}{Var(\Delta S_t | \mathcal{F}_t)} = \frac{\mathbb{E}_t[\Delta \hat{S}_t \hat{\Pi}_{t+1}]}{\mathbb{E}_t[(\Delta \hat{S}_t)^2]}
$$

- When lambda goes infinite, then action is same with risk-based hedges

### Optimal Action:Why the Additional Term?

Why the additional second term in the numerator of the optimal hedge formula?

- The quadratic hedging of the ***discrete-time BSM model only looks at risk of hedge portfolio***
- But ***here the expected reward had both a drift and variance parts***, similar to the Markowitz risk-adjusted portfolio return analysis

옵티멀 액션에서 평균이 r 이거나 람다가 무한대로 가면 두개는 같아짐

***The optimal hedge in a MDP model differs from the optimal hedge in the risk minimization approach*** because the objective function for the former is based on a risk-adjusted return, while it is based on risk only for the latter.

## Monte-Carlo Solution

Use all MC paths simultaneously to learn optimal actions. Why? Because learning optimal actions for all states simultaneously means learning a policy, which is exactly our objective.
$$
\begin{align}
a^*_t(X_t) &= \sum^M_n \phi_{nt} \Phi_n(X_t) \\
Q^*_t(X_t, a^*_t) &= \sum^M_n \omega_{nt} \Phi_n(X_t)
\end{align}
$$

- Coefficients are computed recursively backward in time 

$$
\begin{align}
A^{(t)}_{nm} &= \sum^{N_{MC}}_{k=1} \Phi_n(X^k_t) \Phi_m(X^k_t)(\Delta \hat{S}^k_t)^2 \\
B^{(t)}_n &= \sum^{N_{MC}}_{k=1}\Phi_n(X^k_t)[\hat{\Pi}^k_{t+1} \Delta \hat{S}^k_t + \frac{1}{2 \gamma \lambda} \Delta S^k_t]  \\

C^{(t)}_{nm} &= \sum^{N_{MC}}_{k=1 } \Phi_n(X^k_t) \Phi_m (X_t^k) \\
D_n^{(t)} &= \sum^{N_{MC}}_{k=1} \Phi_n(X^k_t)(R_t(X_t, a^*_t, X_{t+1}) + \gamma \max_{a \in \mathcal{A}} Q^*_{t+1}(X_{t+1},a)) \\
\omega^*_t &= C^{-1}_tD_t
\end{align}
$$

- We obtain the vector-valued solution for optimal weights $$\omega_t$$ defining the optimal Q-function at time t

### Summary of the MC Backward Recursion

For t = T - 1, ... , 0:

1. Compute matrix  $$A_t$$ and vector $$B_t$$
2. Compute the optimal action using $$\phi^*_t = A^{-1}_tB_t$$
3. Compute the optimal reward $$R^*_t$$
4. Compute matrix $$C_t$$ and vector $$D_t$$
5. Compute the optimal Q-function using $$\omega^*_t = C^{-1}_tD_t$$

