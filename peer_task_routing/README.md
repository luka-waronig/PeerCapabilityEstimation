# Foundations of adaptive peer routing dynamics

A mathematical model and executable simulation study of local peer selection under partial, nonstationary feedback. A router estimates neighboring workers' latency and failure probability, then balances expected task cost against an optimistic uncertainty bonus.

## Contents

`PeerTaskRoutingModel.ipynb` is self-contained and includes executed outputs. `requirements.txt` pins the package versions used for that execution. Python 3.13.5 was used. No model API, external service, or additional data file is needed.

The original four-peer illustration is retained first, with a fixed seed and explicit interpretation limits. Exercises below it use a separate, corrected research implementation.

| Part | Experiment |
| --- | --- |
| 1 | Original four-peer shock illustration |
| 2 | Elapsed-time prediction, cost-projected covariance, physical observation model, and executable verification |
| 3 | Eight-policy comparison across stationary, shock, and drift scenarios |
| 4 | Matched Gaussian diagnostics and common-schedule Bernoulli calibration |
| 5 | Mechanism ablations, equal-size validation grids, and independent confirmation runs |
| 6 | Delayed/missing feedback, verification blackouts, and peer recovery |
| 7 | Task-specific capability estimates versus pooled reputation |
| 8 | Latency-reliability operating points across the failure penalty |

## Running

Open the notebook in Jupyter or VS Code and select a Python kernel. Install the supplied packages into that kernel when needed:

```python
%pip install -r requirements.txt
```

Restart the kernel after installation, then run the notebook from top to bottom. Existing environments with compatible package versions may already contain everything needed. Exact pins document the supplied execution; they are not a promise that every older Python version is compatible.

The configuration cell defines a 600-task horizon, 32 fixed-baseline seeds, 12 validation seeds, 16 stress seeds, and 32 separate confirmation seeds. Changing these values changes the experiment. The 2,500-replication Gaussian calibration and 10,000-replication stationary Beta check have separate explicit sizes.

## Model and evaluation

Research observations use positive lognormal latency and Bernoulli failures. All peer trajectories and potential outcomes are generated before routing, and all policies receive the same paired worlds. Policies observe only selected-peer outcomes when their feedback arrives. Expected-cost truth is reserved for evaluation.

The main routing score is `estimated latency + lambda * estimated error - beta * projected cost uncertainty`. The covariance projection is `sqrt(c.T @ P @ c)`, with `c = [1, lambda]`. Elapsed time increases uncertainty for unobserved peers. A log-latency/Beta-Bernoulli comparator distinguishes observation-model choice from routing policy choice; forgetting makes its error posterior an explicitly labeled approximation.

The primary metric is dynamic pseudo-regret against an oracle knowing current expected peer costs. Realized cost and expected cost are different quantities. Confidence intervals and paired comparisons use independent seeds, not timesteps. Curve intervals are pointwise. The parameter search uses validation seeds only, with nine candidates for each of four policy families; confirmation seeds are held out of all earlier experiments.

Feedback delays are additional task rounds. Delayed observations retain their sampling timestamps. Fixed delays preserve per-peer message order; out-of-order and duplicate observations are rejected. Missing verification is not treated as a task outcome. Adaptation metrics retain non-adapting runs using a capped lag and a separate adaptation indicator.

## Scope

This is a synthetic local-router experiment, without implementations of language models or a decentralized Rust/libp2p mesh. Also it does not implement queueing, action-dependent congestion, service-time completion events, censored timeouts, imperfect verification, correlated peer failures, or multiple communicating routers. The original illustration is not silently equated with the research observation model. The reference list and discussion in the notebook distinguish established methods from hypotheses and untested extensions.

Running the notebook does not create files. Tables and figures remain in memory or standard notebook outputs.
