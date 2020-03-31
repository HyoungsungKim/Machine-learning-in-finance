# Week 01

## Options and Option pricing

### Corporate Defaults: The Merton Model

The ***Merton model*** of corporate defaults(1974-present) is the most popular modeling framework, used as a benchmark for many studies.

- The firm is run by equity holders.
  - At time `T`, they pay the face value of the debt `D` if the firm (asset) value is larger than `D`, and keep the remaining amount.
- If the firm value at time `T` is less than `D`, bond holders take over, and recover a "recovery" $$V_t$$, while equity holders get nothing

$$
\begin{align}
B_t & = min(V_T, D) = D - max(D-V_t, 0) \\
S_t & = max(V_t - D, 0)
\end{align}
$$

- D : Debt
- $$V_T$$ : Asset value

$$
C_t = max(S_t - K, 0)
$$

- $$C_t$$ : Option payoff at maturity T
- $$S_t$$ : Stock price at maturity T
- K : Option strike

## Black-Scholes-Merton (BSM) Model

### Option Pricing: Black-Scholes-Merton

Black-Scholes model

- It finds a perfect replicating portfolio whose price is always equal to the option price no matter what the stock price goes in the future.
- This model does it by just mimicking the option by a very frequent shuffling money between your stock investment and your bank cash account

### Idea of BSM Model

- Way to determine how many stocks you should have in your replicating portfolio, in each scenario for the future
  - So that the total portfolio value will always be zero in the future, no matter whaty happens with the stock price.
- This is possible ONLY if your time steps are infinitesimal(극소수의).
  - For this very special setting, the black-Scholes model finds a UNIQUE option price and UNIQUE number of shares you should have in your replicating portfolio NOW

Traders use options and other financial derivatives both as investment vehicles and as hedging instruments(위험 회피 수단).

- And this means that options are not redundant.
  - The reason that options are not redundant is that they carry a substantial risk notwithstanding(~에도 불구하고) the preposition of the classical Black-Scholes model.
- Nobody in the market trades options at their Black-Scholes prices.
  - And differences between Black-Scholes prices and trader prices reflect a dealer's perception of actual risk embedded in options

Financial professionals are well aware of the fact that the classical Black-Scholes model completely eliminates any risk in options for the sake of stability by making two strong assumptions that do not hold in practice.

- 가정을 위해 블랙-숄즈 모형에서는 리스크를 고려하지 않음
- The assumptions of the classical Black-Scholes model that make it totally miss risk and options are continuously hedging and zero transaction costs. 

## Discrete-time BSM Model

We start with a discrete-time version of the BSM model. The problem of option hedging and pricing in this formulation amounts to a ***sequential risk minimization***.

- Risk that we have to minimize in this setting is the risk of mis-hedging, that is the risk of mis-balance between your replication portfolio and your option.

How to define risk in an option?

- To define risk in an option, ***we follow a local risk minimization approach***
- We take the view of a seller of a European option(e.g. a put option) with maturity T and the terminal payoff of $$H_T(S_T)$$.
- To hedge the option, the seller use the proceeds of the sale to set up a replicating (hedge) portfolio $$\Pi_t$$ made of the stock $$S_t$$ and risk-free bank deposit $$B_t$$.
  - 위험을 회피하기 위해, 판매자는 복제 포트폴리오를 만든다.
  - 복제 포트폴리오를 만들어 과거의 옵션 적정가를 보고 변동성을 파악하여 위험을 회피하려 함
- The value of hedge portfolio at any time $$t \le T$$ is

$$
\Pi_t = u_tS_t + B_t
$$

- $$u_t$$ is a stock position at time t, taken to hedge risk in the option

### Hedge Portfolio Evaluation

The replicating portfolio tries to exactly mtch the option price in all possible future states of the world. Start at t = T
$$
\Pi_T = H_T(S_T)
$$

- This sets a terminal condition for $$\Pi_t$$ at t = T
- To find $$B_t$$ for previous times $$t<T$$, we impose the self-financing constraint which requires that ***all future changes in the hedge portfolio should be funded from an initially set bank account***.

$$
u_tS_{t+1} + e^{r \Delta t}B_t = u_{t+1}S_{t+1} + B_{t+1}
$$

- In the left hand side of this equation, we have the portfolio value that we have immediately before a re-hedge.
- And on the right hand side, we have a value that is obtained after a re-hedge.

This can be expressed as a recursive relation for $$B_t$$ at any time $$t < T$$ using its value at the next time instance:
$$
B_t = e^{-r\Delta t}[B_{t+1} + (u_{t+1} - u_t)S_{t+1}], t = T-1, ...,0
$$
