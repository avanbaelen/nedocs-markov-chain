# Predicting Emergency Department Crowding with Markov Chains

A predictive modeling project investigating whether historical emergency department crowding patterns can be used to forecast future National Emergency Department Overcrowding Score (NEDOCS) states.

**[Technical Research Paper](NEDOCS_Markov_Forecasting.pdf)** | **[Jupyter Notebook](NEDOCS_Markov_Forecasting.ipynb)** | **[Data Availability](data/README.md)**

## At a Glance

- **Problem:** Forecast future emergency department crowding from historical NEDOCS state transitions
- **Approach:** Time-dependent Markov chain using 168 hour-of-week transition matrices
- **Forecast Horizon:** 1–6 hours ahead
- **Evaluation:** 80/20 train-test evaluation across two historical analysis periods
- **Key Result:** Approximately 80% overall forecast accuracy at the one-hour horizon
- **Data:** Real-world, non-public emergency department operational data

## Project Overview

Emergency department overcrowding is a persistent operational challenge that can affect patient flow, resource utilization, and patient care. The National Emergency Department Overcrowding Score (NEDOCS) provides a standardized measure of current ED crowding but does not predict how conditions may change over the next several hours.

This project investigates whether historical NEDOCS state transitions can be used to forecast future crowding conditions using a time-dependent Markov chain model.

Rather than treating each forecast as an independent prediction, the model estimates the probability of transitioning from the current NEDOCS state to another state based on the hour and day of the week.

## Research Question

**Can historical NEDOCS state transitions be used to predict emergency department crowding several hours into the future?**

The project also evaluates whether using a longer historical period improves forecast performance.

## Modeling Approach

Hourly NEDOCS observations were classified into six crowding states:

`Not Busy → Busy → Extremely Busy → Overcrowded → Severe → Critical`

To account for temporal differences in emergency department activity, the model constructs a separate state-transition matrix for every hour of the week:

**7 days × 24 hours = 168 time-dependent transition matrices**

Each matrix estimates the probability of transitioning from the current NEDOCS state to each possible state during the following hour for a specific day and hour.

Two historical analysis periods were evaluated: approximately **16 months** and **52 months** of observations. For consistency with the original analysis, these are referred to as the **one-year** and **four-year** models.

Each dataset was divided using an 80/20 train-test split. Predictions were evaluated at forecast horizons ranging from one hour (**t+1**) through six hours (**t+6**).

## Key Findings

- Both models achieved approximately **80% overall accuracy at the one-hour forecast horizon (t+1)**.
- Forecast accuracy declined as the prediction horizon increased from **one to six hours**.
- The longer historical period produced a **modest improvement in overall forecast performance**, although the benefit varied across individual NEDOCS states.
- The largest differences between training periods were observed for the **Extremely Busy** and **Critical** states.
- The **Critical** state demonstrated particularly strong forecast performance in the four-year model.
- Training observations were substantially imbalanced toward higher crowding states, which may have influenced state-level forecast performance.

### Critical-State Forecast Performance

| Forecast Horizon | Four-Year Model Accuracy |
| ---------------- | -----------------------: |
| t+1 | 87% |
| t+2 | 83% |
| t+3 | 82% |
| t+4 | 84% |

The results suggest that time-dependent Markov chains can provide useful short-term forecasts of emergency department crowding, while predictive performance becomes increasingly limited as the forecast horizon increases.

## Model Evaluation

Model performance was evaluated using multiple complementary measures:

- Overall forecast accuracy
- Accuracy by NEDOCS state
- Brier score for probabilistic forecast performance
- Normalized confusion matrices
- Performance across forecast horizons from t+1 through t+6
- Comparison of the shorter and longer historical analysis periods
- Analysis of temporal performance and NEDOCS state distribution

State-level evaluation was particularly important because overall accuracy alone can obscure substantial differences in predictive performance between individual crowding states.

## Technologies & Methods

**Technical:** Python, Jupyter Notebook, Pandas, NumPy, Matplotlib, Seaborn

**Modeling:** Markov chains, transition probability matrices, probabilistic forecasting, multi-step forecasting

**Evaluation:** Prediction accuracy, Brier score, normalized confusion matrices, state-level performance analysis

**Domain:** Healthcare analytics, emergency department operations, NEDOCS

## Data Availability

The analysis was conducted using real-world, non-public emergency department operational data.

The underlying dataset is **not included in this repository** due to data-use and privacy considerations. The repository contains the modeling methodology, Python implementation, aggregate analytical results, and supporting documentation without distributing the source healthcare data.

The public version of the notebook has been sanitized to remove source data, internal file paths, and organization-specific information.

See **[Data Availability](data/README.md)** for additional information.

## Limitations

The model primarily uses historical transitions between NEDOCS states and therefore does not directly incorporate many operational factors that may influence future ED crowding, including patient arrivals, staffing, inpatient bed availability, patient acuity, diagnostic delays, and other hospital capacity constraints.

The distribution of observations across NEDOCS states was substantially imbalanced toward higher crowding states, which may have influenced state-specific prediction accuracy.

Forecast accuracy also decreased as the prediction horizon increased, suggesting that the model is better suited to short-term forecasting than longer-range prediction.

## Future Work

Potential extensions of this project include:

1. Establishing persistence and time-of-week forecasting baselines
2. Incorporating additional emergency department operational variables
3. Comparing Markov forecasting with statistical and machine-learning approaches
4. Further evaluating approaches for handling state imbalance
5. Investigating alternative Markov model structures
6. Evaluating model performance across additional emergency department environments

## Repository Contents

```text
nedocs-markov-chain/
│
├── data/
│   └── README.md
│
├── .gitignore
├── NEDOCS_Markov_Forecasting.ipynb
├── NEDOCS_Markov_Forecasting.pdf
└── README.md
