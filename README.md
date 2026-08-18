# Forecasting Emergency Department Crowding with Markov Chains

A predictive modeling project investigating whether historical emergency department crowding patterns can be used to forecast future National Emergency Department Overcrowding Score (NEDOCS) states.

## Project Overview

Emergency department overcrowding is a persistent operational challenge that can affect patient flow, resource utilization, and patient care. The National Emergency Department Overcrowding Score (NEDOCS) provides a standardized measure of current ED crowding, but the score itself does not predict how conditions may change over the next several hours.

This project explores the use of time-dependent Markov chains to forecast future NEDOCS states using historical emergency department operational data.

Rather than modeling ED crowding as an independent prediction at each point in time, the model uses the probability of transitioning from the current NEDOCS state to another state during subsequent hours.

## Research Question

**Can historical NEDOCS state transitions be used to predict emergency department crowding several hours into the future?**

The project also evaluates whether using a longer historical training period improves forecast performance.

## Methodology

Historical hourly NEDOCS observations were classified into six crowding states:

1. Not Busy
2. Busy
3. Extremely Busy
4. Overcrowded
5. Severe
6. Critical

Because emergency department activity varies throughout the week, the primary model incorporates temporal patterns by constructing a separate transition probability matrix for each hour of each day:

**7 days × 24 hours = 168 transition matrices**

Each matrix represents the observed probabilities of transitioning between NEDOCS states for a specific hour of the week.

The data were divided chronologically into training and testing periods, with the earliest 80% of observations used for model development and the remaining 20% reserved for out-of-sample evaluation.

Models using different amounts of historical training data were evaluated to determine whether additional historical observations improved forecasting performance.

Predictions were generated at forecast horizons ranging from one hour (t+1) through six hours (t+6).

## Model Evaluation

Forecast performance was evaluated using multiple measures:

* Overall prediction accuracy
* Accuracy by NEDOCS state
* Brier score for probabilistic forecast performance
* Normalized confusion matrices
* Performance across multiple forecast horizons
* Comparison of models using different historical training periods
* Analysis of performance across temporal patterns and crowding states

Additional experiments investigated weighting and balancing approaches to better understand the effect of differences in the frequency of individual NEDOCS states.

## Key Findings

Both primary models achieved approximately **80% overall accuracy when forecasting NEDOCS one hour into the future (t+1)**, with accuracy declining as the forecast horizon increased.

The model using the longer historical training period generally performed slightly better overall, with more noticeable improvements for some individual crowding states.

Prediction of the **Critical** NEDOCS state demonstrated particularly strong performance:

| Forecast Horizon | Critical-State Accuracy |
| ---------------- | ----------------------: |
| t+1              |                     87% |
| t+2              |                     83% |
| t+3              |                     82% |
| t+4              |                     84% |

Analysis of the underlying state distribution identified an imbalance toward higher crowding states. This may have contributed to stronger predictive performance for the Critical state and was considered when interpreting model performance.

The results suggest that time-dependent Markov chains may provide useful short-term forecasts of emergency department crowding, while predictive performance becomes more limited as the forecast horizon increases.

## Technologies & Methods

* Python
* Jupyter Notebook
* Pandas
* NumPy
* Markov Chains
* Transition Probability Matrices
* Probabilistic Forecasting
* Brier Score
* Confusion Matrices
* Predictive Model Evaluation
* Data Visualization
* Healthcare Analytics

## Data Availability

The original analysis was conducted using real-world emergency department operational data.

The underlying dataset is proprietary healthcare information and is **not included in this repository**. The public repository contains the modeling methodology, Python implementation, aggregate analytical results, and supporting documentation without distributing the source healthcare data.

The public version of the notebook has also been sanitized to remove source data, internal file paths, and organization-specific information.

## Repository Contents

```text
nedocs-markov-forecasting/
│
├── README.md
│
├── notebooks/
│   └── nedocs_markov_forecasting.ipynb
│
├── docs/
│   └── nedocs_markov_forecasting_paper.pdf
│
├── figures/
│   ├── forecast_accuracy.png
│   ├── state_accuracy.png
│   └── confusion_matrices.png
│
└── requirements.txt
```

### Jupyter Notebook

The notebook contains the Python implementation of the forecasting framework, including data preparation, NEDOCS state classification, transition-matrix construction, multi-hour forecasting, model evaluation, and visualization.

### Technical Paper

The accompanying technical paper documents the original research question, methodology, numerical results, analysis, and conclusions in greater detail.

## Limitations

The model primarily uses historical transitions between NEDOCS states and therefore does not directly incorporate many operational factors that may influence future ED crowding, such as patient arrivals, staffing, inpatient bed availability, patient acuity, diagnostic delays, or other hospital capacity constraints.

The distribution of observations across NEDOCS states was also imbalanced, particularly toward higher crowding states, which may have influenced state-specific prediction accuracy.

Forecast accuracy decreased as the prediction horizon increased, suggesting that the model is better suited to short-term forecasting than longer-range prediction.

## Future Work

Potential extensions of the project include:

* Comparing the Markov model against persistence and time-of-week forecasting baselines
* Incorporating additional ED operational variables
* Comparing Markov forecasting with statistical and machine-learning approaches
* Further evaluating approaches for handling state imbalance
* Investigating alternative Markov model structures
* Evaluating model performance across additional emergency department environments

## Project Background

This project was originally developed as undergraduate research exploring the application of stochastic modeling to a real-world healthcare operational problem.

It is presented here as a technical portfolio project demonstrating the application of probability, predictive modeling, Python, model evaluation, and healthcare domain knowledge to emergency department operations.
