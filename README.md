# Bayesian Marketing Mix Modeling with PyMC

This case study estimates how seven anonymized marketing channels are associated with weekly revenue while accounting for delayed advertising effects, long-term trend, and annual seasonality.

The project demonstrates Bayesian model design, time-series feature engineering, posterior diagnostics, and careful interpretation of uncertain business estimates. It is an exploratory portfolio analysis—not a causal budget-allocation system.

## Business question

Given 104 weeks of revenue and channel-spend observations, can we:

- estimate how long each channel's effect persists;
- compare channel contribution and posterior-mean ROI;
- separate marketing signals from trend and seasonality; and
- communicate the uncertainty and limitations behind those estimates?

## Data

The included challenge dataset contains:

- 104 weekly observations from August 2020 to August 2022;
- weekly revenue;
- spend for seven anonymized marketing channels; and
- no promotion, price, competitor, or macroeconomic variables.

Channel identities are anonymized, so the findings are useful for demonstrating methodology rather than making operational recommendations.

## Method

The notebook fits a Bayesian regression in PyMC with:

1. **Geometric adstock** for channel-specific carry-over:

   `adstock[t] = spend[t] + decay * adstock[t-1]`

2. **Positive channel coefficients** using Half-Normal priors.
3. **A normalized linear trend**.
4. **Annual seasonality** using one sine/cosine Fourier pair.
5. **NUTS sampling**, prior predictive checks, posterior diagnostics, and posterior predictive checks.

Revenue is standardized and spend is scaled by each channel's maximum before fitting. Channel contribution is then transformed back to euros for an exploratory ROI calculation.

## Results from the committed notebook

| Diagnostic | Result |
|---|---:|
| In-sample R² using the posterior predictive mean | 0.420 |
| In-sample MAE | €26,481 |
| In-sample MAPE | 19.8% |
| Maximum reported R-hat | 1.01 |
| Posterior draws | 2 chains × 1,000 draws after tuning |

Channel 2 has the largest posterior-mean ROI estimate (19.72x), but it also has the lowest total spend and a wide coefficient posterior. This should be treated as a hypothesis for further testing—not evidence that the channel is underfunded.

Channels 3 and 7 have posterior-mean ROI estimates below 1.0. Without experimental variation, additional control variables, and uncertainty intervals for ROI, this is not sufficient evidence to cut either channel.

## What the project shows

- Translating a business question into a probabilistic model
- Implementing channel-specific adstock with PyTensor
- Choosing and explaining priors
- Checking convergence with R-hat and effective sample size
- Using prior and posterior predictive checks
- Separating statistical estimates from business decisions

## Important limitations

- Performance is reported on the training period; there is no time-based holdout evaluation.
- Observational MMM does not establish causal lift by itself.
- Missing promotion, price, competitor, and macroeconomic variables may confound channel effects.
- Correlated channel spending can make individual coefficients difficult to identify.
- The model includes adstock but no diminishing-returns or saturation transformation.
- ROI is calculated from posterior-mean parameters rather than a full posterior ROI distribution.
- The committed run uses two chains; four chains would provide a more robust convergence check.

## Repository structure

```text
.
├── MMM_BayesianMixedMediaModel.ipynb  # End-to-end analysis and outputs
├── MMM_test_data.csv                  # 104-week challenge dataset
├── requirements.txt                   # Python dependencies
└── README.md                          # Project overview and interpretation
```

## Run locally

```bash
git clone https://github.com/adelF-a/marketing-mix-modeling-challenge.git
cd marketing-mix-modeling-challenge

python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab MMM_BayesianMixedMediaModel.ipynb
```

Run the notebook from top to bottom. Sampling time depends on the machine; the committed run took approximately three minutes with two sequential chains.

## Environment used for the committed run

- Python 3.14.4
- PyMC 5.28.5
- ArviZ 0.23.4

## Next improvements

- Use a rolling or final-period holdout for out-of-sample evaluation
- Add a saturation curve for diminishing marginal returns
- Propagate posterior uncertainty into ROI intervals
- Compare the Bayesian model with a simpler regularized baseline
- Use four sampling chains and report posterior predictive coverage
