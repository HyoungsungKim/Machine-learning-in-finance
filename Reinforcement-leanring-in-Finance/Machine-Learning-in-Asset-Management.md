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

