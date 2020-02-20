# Week01

## Glossary

- AI : we mean the science of making machines that achieve human-level performance on specific tasks that require some imitation of the cognitive process of the humans.
- Machine Learning : A subfield of AI that focuses on algorithms that teaches computers to perform a task starting directly from data, rather than being explicitly programmed for the task. 
- Data mining : It uses Machine Learning techniques to find actionable patterns in data.
- Data science : It is the name of a new emergent profession for folks that apply statistics and Machine Learning to help monetizing(수익창출) information and data. 

## Differences Between ML and Statistical Modeling

통계모델은 사전 확률 이용하고, 기계학습은 사후 확률 이용하는 차이인듯?

### Statistical Modeling

- Parametric models that try to "explain" the world.
  - The focus is on modeling causality
- ***Deduce***(추정하다) relations for observed quantities by parameter estimation for a pre-specified model of the world.
- Scalability is typically not the major concern
- Based on a probabilistic approach

### Machine Learning

- Non-parametric models that try to "mimic" the world rather than "explain" it.
  - Often uses correlations as proxies to causality
- ***Induce***(유도하다)  relations between observable quantities, main goal is predictive power.
- Scalability is often critical in applications
- Some ML methods are not probabilistic(SVM, Neural networks, Clustering, etc...)

## Elements of ML

### Types of ML tasks

- The agent can also interact with the environment in different modes.
  - For example, the agent can interact with the physical environment in real-time via sensors.
  - This type of learning is called ***online learning***. 
- When the agent doesn't have a real-time access to the environment, but only has access to its snapshot stored as data on the computer disk.
  - This is called ***offline or batch learning***. 

#### Agent <- Data from environment : Perception Learning

Perception task

- The agent should learn from its environments to perform one specific predefined action.
  - Perception and learning from data
  - There is a fixed action
  - The output is a learned function of data

#### Agent -> Environment : Actions

Action tasks:

- There are multiple possible actions
- Similar to perception tasks, they involve as sub task of learning from the environment
  - ***But the final goal is to find an optimal action among many.***
  - Involve sub-tasks of learning from an environment
- For sequential(multi-step) problems, action tasks involve planning and forecasting the future
- Therefore, Action tasks are generally more complex than perception tasks.

### Performance Measure P

Typically, performance measure P is specific to the task T

- One possible choice for classification tasks
- One measure of the model accuracy can be the Error Rate

$$
\text{Error rate } = \frac{N_{\text{incorrectly-classified}}}{N_{total}} \\
Accuracy = 1 - \text{Error rate}
$$

- But such performance measure is inconvenient in practice because it might change this continuously when parameters of the model are changed continuously.
  - In other words such performance metric would be a non-differentiable function of its parameters, so that no gradient-based optimization manhoods could be applied in this setting

A smooth version is available for probabilistic model: the ***log probability***

- A smooth and differentiable alternative to the Error Rate as a performance measure is to consider instead the probability or low probability of the observed data under assumptions of the model.
  - This leads to a differentiable objective function for the process of tuning all the parameters to the data, which can be efficiently done using gradient based optimization software.
  - If we deal with Regression problems, one particular performance metric is a mean square lossm L1-loss.

### Experience E

The performance measure `P` improves with Experience `E` as a result of learning Learning from experience = ability to generalize

- Generally, we can distinguish three major types of learning from experience.
  - Supervised learning()
    - A teacher provides a training ser of features and label pair $$(X_i, C_i)$$
  - Unsupervised learning
    - Only examples with attributes `X` are given
    - No `teacher`: only a dataset of features vector $$(X_i)$$ is provided
  - Reinforcement learning
    - There is a teacher, but it gives only a partial feedback (a reward)
    - Reinforcement learning forces unsupervised learning algorithms to behave in a similar way to supervised learning algorithms using the latest groundbreaking research in Deep learning.

## Machine learning landscape

- Supervised learning 
  - Perception tasks
    - Should perform one particular action like classify an image
  - Regression(e.g. Demand forecast)
    - Learn regression function
      - $$\mathcal{f} : \mathbb{R}^n \rightarrow \mathbb{R}$$
      - Maps n dimensional space to a real line
    - Given : input and output pairs $$(X_i, Y_i)$$
  - Classification(e.g. Spam detection, Image recognition, Document classification)
    - Learn class function
      - $$\mathcal{f}:\mathbb{R}^n \rightarrow \{1,...,k\}$$
    - Given : input and output pairs  $$(X_i, C_i)$$
- Unsupervised learning
  - Perception tasks(Same with Supervised learning)
    - Should perform one particular action like classify an image
  - No teacher to provide the right answers
  - Clustering-also known as segmentation in business(e.g. Customer segmentation, Anomaly detection)
    - To cluster or partition set of data points into homogenous groups of points, you only need to give the algorithm attributes x of all points
    - Learn class function
      - $$\mathcal{f}:\mathbb{R}^n \rightarrow \{1,...,k\}$$
      - k-the number of clusters
    - Given : inputs only $$(X_i)$$
  - Representation learning(e.g. Text recognition, machine translation)
    - Learning representer function
      - $$\mathcal{f} : \mathbb{R}^n \rightarrow \mathbb{R}^k$$
    - Feature extraction, dimension reduction
    - Given : inputs only $$(X_i)$$
    - ***It is very common in finance***
- Reinforcement learning 
  - Action tasks
    - Optimization of strategy for a task(e.g. Robotics, Computational advertising)
      - Learning policy function
        - $$\mathcal{f}:\mathbb{R}^n \rightarrow \mathbb{R}^k$$
        - Pick optimal action to maximize total reward
      - Given : tuples $$(X_i, a_t, X_{i+1}, r_i)$$
    - IRL : Learn Objective from Behavior (e.g. Imitation learning for robotics)
      - Learn reward function
        - $$\mathcal{f}:\mathbb{R}^n \rightarrow \mathbb{R}$$
      - Find the reward function that explains behavior
      - Given : tuple $$(X_i, a_i, X_{i+1})$$

## ML in Finance vs ML in Tech

### ML by financial application Areas

- Banking(Supervised learing)
  - Retail P2P, Lending
    - Customer segmentation, Loan defaults, Credit card defaults, Fraud detection, Anti-money laundry
  - Commercial and Investment
    - Rating prediction, Default modeling, Client data mining, Recommender systems
- Asset Management(Unsupervised learing)
  - Representation learning
    - Factor modeling, De-noising, Regime change detection, Stock segmentation
  - Portfolio optimization
    - Multi-period portfolio optimization, Derivatives trading
- Quantitative Trading(Reinforcement learning)
  - Optimal trade execution
    - Profit-maximizing trade execution
  - Quantitative trading strategies
    - Earning prediction, Algorithmic trading, Optimal market making

### ML in finance

### Forecasting Tasks

- There are many types of forecasting problems in Finance that belong in the class of perception tasks
  - Security price predictions (stocks, bonds, commodities etc.
  - Methods : SL/UL
- Other forecasting tasks in Finance including predicting actions of individual players
  - Corporate actors action prediction (dividends, mergers, defaults etc.) Methods : SL/UL/RL
  - Individual actors action prediction (loan defaults, fraud, AML(Anti Money laundrying), etc.) Methods : SL/UL/RL

### Valuation Tasks

- Other typical perception tasks which are commonly used in Finance deal with valuation
  - Asset valuation (stocks, futures, commodities, bonds, etc.).
    - Related to forecasting. Methods : SL/UL
  - Derivatives valuation. Methods : SL/UL/RL

Why many perception tasks in Finance involve RL?

- In finance, perception tasks are often about the future
- The future is partly driven by future actions of decision makers
- This brings RL into the game even for "perception" tasks in Finance!

### Trading and Asset management

Action : Trading and asset management

- Optimal execution for brokerage trading. Methods : SL/UL/RL
- Optimal strategies for day trading. Methods : SL/UL/RL
- Active portfolio management. methods : SL/UL/RL

### Decision-Making in banking

Action : Decision-making in banking

- Loan approvals. Methods : SL/UL/RL, Bayesian networks
- Credit and operational risk management. methods : SL/UL/RL, Bayesian networks
- Decision-making in compliance analytics (fraud, AML, etc.). Methods : SL/UL/RL, Bayesian networks

### Summary

- We saw that in Finance, perception tasks might involve elements of RL. This is unlike typical perception tasks for ML in Tech
  - This happens because some perception tasks in Finance involve predicting future actions of rational (or semi-rational) actors, or own actions (like e.g. with American options)
  - Moreover the fact that the reinforcement learning appears as a methods of choice for both perception and action tasks in Finance unlike Tech