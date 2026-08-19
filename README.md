# Predicting Emergency Department Crowding: A Markov Chain Model of Hourly NEDOCS States

A predictive modeling project investigating whether historical emergency
department crowding patterns can be used to forecast future National Emergency
Department Overcrowding Score (NEDOCS) states.

## Project Overview

Emergency department overcrowding is a persistent operational challenge that
can affect patient flow, resource utilization, and patient care. The National
Emergency Department Overcrowding Score (NEDOCS) provides a standardized measure
of current ED crowding, but the score itself does not predict how conditions may
change over the next several hours.

This project explores the use of time-dependent Markov chains to forecast future
NEDOCS states using historical emergency department operational data.

Rather than modeling ED crowding as an independent prediction at each point in
time, the model uses the probability of transitioning from the current NEDOCS
state to another state during subsequent hours.

## Research Question

**Can historical NEDOCS state transitions be used to predict emergency department
crowding several hours into the future?**

The project also evaluates whether a longer historical training period improves
forecast performance.

## Methodology

Historical hourly NEDOCS observations were classified into six crowding states:

1. Not Busy
2. Busy
3. Extremely Busy
4. Overcrowded
5. Severe
6. Critical

Because emergency department activity varies throughout the week, the model
incorporates temporal patterns by constructing a separate transition probability
matrix for each hour of each day:

**7 days × 24 hours = 168 transition matrices**

Each matrix represents the observed probabilities of transitioning between
NEDOCS states for a specific hour of the week.

Two historical analysis periods were evaluated: approximately **16 months** and
**52 months** of observations. For consistency with the original analysis,
these are referred to as the **one-year** and **four-year** models.

Each dataset was divided using an 80/20 train-test split, with predictions
evaluated at forecast horizons ranging from one hour (t+1) through six hours
(t+6).

## Model Evaluation

Forecast performance was evaluated using:

- Overall prediction accuracy
- Accuracy by NEDOCS state
- Brier score for probabilistic forecast performance
- Normalized confusion matrices
- Performance across multiple forecast horizons
- Comparison of the shorter and longer historical training periods
- Analysis of temporal performance and NEDOCS state distribution

## Key Findings

Both models achieved approximately **80% overall accuracy when forecasting
NEDOCS one hour into the future (t+1)**, with accuracy declining as the forecast
horizon increased.

The model using the longer historical period generally performed modestly better
overall, although the benefit varied across individual NEDOCS states. The most
noticeable differences occurred for the **Extremely Busy** and **Critical**
states.

Prediction of the **Critical** NEDOCS state demonstrated particularly strong
performance in the four-year model:

| Forecast Horizon | Critical-State Accuracy |
| ---------------- | ----------------------: |
| t+1 | 87% |
| t+2 | 83% |
| t+3 | 82% |
| t+4 | 84% |

Analysis of the training data identified substantial class imbalance toward
higher crowding states. This may have contributed to differences in state-level
forecast performance and was considered when interpreting the model results.

Overall, the results suggest that time-dependent Markov chains may provide
useful short-term forecasts of emergency department crowding, while predictive
performance becomes more limited as the forecast horizon increases.

## Technologies & Methods

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Markov chains
- Transition probability matrices
- Probabilistic forecasting
- Brier score
- Confusion matrices
- Predictive model evaluation
- Data visualization
- Healthcare analytics

## Data Availability

The analysis was conducted using real-world, non-public emergency department
operational data.

The underlying dataset is **not included in this repository** due to data-use
and privacy considerations. The repository contains the modeling methodology,
Python implementation, aggregate analytical results, and supporting
documentation without distributing the source healthcare data.

See [`data/README.md`](data/README.md) for additional information about the
expected data structure and availability.

## Repository Contents

```text
nedocs-markov-chain/
│
├── README.md
├── .gitignore
├── NEDOCS_Markov_Forecasting.ipynb
├── NEDOCS_Markov_Forecasting.pdf
│
└── data/
    └── README.md
