# Machine Learning in Asset Management—Part 1: Portfolio Construction—Trading Strategies

## Introduction

Security : 증권

Financial machine learning research can loosely be divided into four streams.

- The first concerns ***asset price prediction*** where researchers attempt to predict the future value of securities using a machine learning methodology.
- The second stream involves ***the prediction of hard or soft financial events like earnings surprises, regime changes, corporate defaults, and mergers and acquisitions***.
- The third stream ***entails the prediction and/or estimation of values that are not directly related to the price of a security***, such as future revenue, volatility, firm valuation, credit ratings, and factor quantiles.
- The fourth and last stream comprises the use of machine learning techniques to ***solve traditional optimization and simulation problems*** in finance like optimal execution, position sizing, and portfolio optimization.

It is necessary to define the difference between the aforementioned themes.

- ***Technical trading*** is the use of market data and its transformations to predict the future price of an asset.
- ***Trend trading*** involves strategies where one takes a position in the asset only after predicting a change in trend.
- ***Statistical arbitrage*** seeks mispricing by detecting asset relationships and/or potential anomalies, believing the anomaly will return to normal.
- ***Risk parity strategies*** diversifies across assets according to the volatility they exhibit; when one asset class’s volatility exceeds another rebalancing can occur by selecting individual units within each asset class or simply by using leverage.
  - 가격차이로 보는 이득 말하는 듯
- ***Event trading*** involves the prediction of hard or soft financial events like corporate defaults, mergers and acquisitions, and earnings surprises.
- ***Factor investing*** attempts to buy assets that exhibit a trait historically associated with promising investment  returns.
- ***Systematic  global macro*** relies on macroeconomic principles to trade across asset classes and countries.
- ***Fundamental trading*** relies on the use of accounting, management, and sentiment data to predict whether a stock is over or undervalued.

## Reinforcement Learning

### Tiny RL - Technical/RL/Policy

[Stock Trading with Recurrent Reinforcement Learning (RRL)](http://cs229.stanford.edu/proj2006/Molina-StockTradingWithRecurrentReinforcementLearning.pdf)

In this example we will make use of gradient descent to maximize a reward function.

- The Sharpe ratio will be used as the reward function.
- The Sharpe ratio is used as an ***indicator to measure the risk-adjusted performance*** of an investment over time.

Assuming a risk-free rate of zero, the Sharpe ratio can be written as
$$
S_t = \frac{A}{\sqrt{B-A^2}}
$$
or
$$
S_t = \frac{Average(R_t)}{Standard \text{ } Deviation(R_t)}
$$

- 분산이 크면 수익(S) 낮아짐

Further, to know what percentage of the portfolio should buy the asset in a long-only strategy, we can specify the following function, which will generate a value between -1 and 1

- 이 자산을 구매하는게 좋은지 아닌지를 판단하는 함수

$$
F_t = \tanh(\theta^Tx_i)
$$

The input vector is $$x_t= [1, r_t, r_{t–M}, ..., F_{t–1}]$$ where $$r_t$$ is the percent change between the asset at time t and t – 1 and M is the number of time-series inputs.

- $$r_t = p_t - p_{t-1}$$
  - $$p_t$$ : value asset at time period t
- ***This means that at every step the model will be fed its last position and a series of historical price changes*** that are used to calculate the next position. 
- 마지막 포지션의 tanh 결과 값과와 시간 변화마다 생긴 가격 차이가 들어가있음
- There are three types of positions that can be held : long, short, or neutral
  - Long position : When $$F_t > 0$$.
    - In this case, the trader buys an asset at price $$p_t$$ and hopes that it appreciates by period t + 1
    - Long position에서는 자산을 구매하고 가격이 상승하길 원함
  - Short position : When $$F_t < 0$$
    - The trader sells an asset when it does not own at price $$p_t$$, with the expectation to produce the shares at period t + 1.
    - Short position에서는 자산을 먼저 판매하고 가격이 떨어지길 원함(가격이 상승하면 손해)
  - Neutral : $$F_t = 0$$
    - No effect on the trader's profits

We can calculate our returns R at each time steps using the following formula
$$
R_t = \mu (F_{t-1}r_t - \delta|F_t - F_{t-1}|)
$$

- $$\mu$$ : The maximum possible number of shares per transaction.
- $$\delta$$ : Transaction cost
- The first term $$(\mu F_{t-1}r_t)$$ is the return resulting from the investment decision from the period t -1