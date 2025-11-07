## Model Simulation:


A minimal discrete-time model that simulates the evolution of consumption (C) and spending (X) under a threshold-based adjustment rule. This repository includes the simulation script and visualizations.

# Overview
At each period the model checks whether consumption (C_t) is inside a user-defined steady-state band ([ss_{Low}, ss_{High}]).

If (C_t) is inside the band, consumption decays by depreciation and no spending occurs.

If (C_t) is outside the band, consumption is reset to a target level (1/p) and spending (X_{t+1}) equals the gap between the target and the decayed previous level.

This README documents the model, the math, how to run the code, and how to export results for submission or sharing.

# Model equations (math)
Let:

1. \(C_t\) be consumption at time \(t\)
2. \(X_t\) be spending at time \(t\)
3. \(\delta \in [0,1]\) be the depreciation rate
4. \(p > 0\) be a price/target parameter
5. \((ss_{Low}, ss_{High})\) define the steady-state interval
6. \(T\) be the total number of discrete periods (time steps)

The update rules are:

$$
C_{t+1} =
\begin{cases}
(1 - \delta)C_t, & \text{if } ss_{Low} \le C_t \le ss_{High} \\
\frac{1}{p}, & \text{otherwise}
\end{cases}
$$

$$
X_{t+1} =
\begin{cases}
0, & \text{if } ss_{Low} \le C_t \le ss_{High} \\
\frac{1}{p} - (1 - \delta)C_t, & \text{otherwise}
\end{cases}
$$

Interpretation

When inside the band: consumption decays by ((1-\delta)) (exponential decay if repeated), and no corrective spending occurs.
When outside the band: the system performs a reset to the target (1/p); spending equals the correction needed to reach that target after accounting for natural decay.
Note on fixed points: The trivial mathematical fixed point of the decay equation (C = (1-\delta)C) is (C=0). The practical "steady-state" of interest is enforced by the band ([ss_{Low}, ss_{High}]) and the reset mechanism (so the model’s dynamics are governed by bounded oscillations around the target rather than convergence to zero).

Parameter definitions
CInit — initial consumption (C_0)
XInit — initial spending (X_0)
delta — depreciation rate (\delta) (e.g., 0.05)
p — price/target parameter; target level is (1/p) (e.g., p = 1.25 → target = 0.8)
ssLow, ssHigh — bounds of the steady-state interval (e.g., 0.9 and 1.1)
T — number of periods to simulate (e.g., 30)
Example (pseudo / Python-like)


# Parameters (example)
CInit = 1.0
XInit = 0.0
delta = 0.05
p = 1.25
ssLow = 0.9
ssHigh = 1.1
T = 31

# Arrays (preallocate)
C = [0.0]*T
X = [0.0]*T
time = list(range(T))

C[0] = CInit
X[0] = XInit
time[0] = 0

for t in range(0, T-1):
    if ssLow <= C[t] <= ssHigh:
        C[t+1] = (1 - delta) * C[t]
        X[t+1] = 0.0
    else:
        C[t+1] = 1.0 / p
        X[t+1] = (1.0 / p) - ((1 - delta) * C[t])
​
