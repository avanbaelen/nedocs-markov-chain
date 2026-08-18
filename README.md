# nedocs-markov-chain
# Forecasting Emergency Department Crowding with Markov Chains

A healthcare analytics project exploring whether time-dependent Markov chain models can predict future emergency department crowding states using the National Emergency Department Overcrowding Score (NEDOCS).

## Project Overview

Emergency department overcrowding is a persistent operational challenge that can affect patient flow, resource utilization, and quality of care. NEDOCS provides a standardized method for measuring the current level of emergency department crowding, but the score itself does not provide a forecast of future conditions.

This project investigates whether historical transitions between NEDOCS states can be modeled using Markov chains to predict emergency department crowding several hours into the future.

The analysis compares models trained using one year and four years of historical observations and evaluates forecast performance at horizons ranging from one to six hours.

## Research Question

**Can historical NEDOCS state transitions be used to predict emergency department crowding several hours in advance?**

A secondary objective was to determine whether a longer historical training period improves predictive performance.

## Methodology

NEDOCS scores were converted into six discrete crowding states:

1. Not Busy
2. Busy
3. Extremely Busy
4. Overcrowded
5. Severe
6. Critical

Rather than using a single transition matrix, the model incorporates time-of-week patterns by constructing a separate Markov transition matrix for each hour of each day.

This produces:

**7 days × 24 hours = 168 transition matrices**

Two models were developed using different amounts of historical training data:

* One-year training model
* Four-year training model

Each dataset was divided using an 80/20 train-test split. Predictions were generated for forecast horizons from one hour (t+1) through six hours (t+6).

## Model Evaluation

Forecast performance was evaluated using several approaches:

* Overall prediction accuracy
* Accuracy by individual NEDOCS state
* Brier score for probabilistic forecast performance
* Normalized confusion matrices
* Comparison of one-year and four-year training periods

Additional experiments investigated weighting and balancing techniques to determine whether state imbalance affected model performance.

## Key Findings

Both models achieved approximately **80% overall accuracy at the one-hour forecast horizon**, with accuracy decreasing as the forecast horizon increased.

The four-year model generally performed slightly better than the one-year model, with larger improvements observed for some individual NEDOCS states.

The Critical crowding state demonstrated particularly strong predictive performance in the four-year model, with approximately:

| Forecast Horizon | Critical-State Accuracy |
| ---------------- | ----------------------: |
| t+1              |                     87% |
| t+2              |                     83% |
| t+3              |                     82% |
| t+4              |                     84% |

Analysis of the state distribution showed an imbalance toward higher crowding states. This may partially explain the model's stronger performance when predicting the Critical state and represents an important limitation when interpreting the results.

The results suggest that time-dependent Markov chains may provide useful short-term forecasts of ED crowding states, particularly at shorter forecast horizons.

## Technologies and Methods

* Python
* Jupyter Notebook
* Markov Chains
* Transition Probability Matrices
* Probabilistic Forecasting
* Brier Score
* Confusion Matrices
* Predictive Model Evaluation
* Data Visualization
* Healthcare Analytics

## Data Availability

The original analysis was performed using healthcare operational data that is not included in this public repository.

The repository contains the modeling code, methodology, aggregate results, and supporting documentation necessary to demonstrate the analytical approach without distributing the underlying healthcare data.

## Repository Contents

```text
nedocs-markov-forecasting/
│
├── README.md
├── notebooks/
│   └── nedocs_markov_forecasting.ipynb
├── docs/
│   └── nedocs_markov_forecasting_paper.pdf
├── figures/
│   ├── forecast_accuracy.png
│   ├── state_accuracy.png
│   └── confusion_matrices.png
└── requirements.txt
```

### Jupyter Notebook

The notebook contains the Python implementation of the Markov forecasting model, including transition-matrix construction, multi-step forecasting, model evaluation, and visualization.

### Technical Paper

The accompanying paper provides a more detailed discussion of the motivation, methodology, numerical results, limitations, and conclusions of the original project.

## Limitations

Several limitations should be considered when interpreting the results.

The model relies primarily on historical transitions between NEDOCS states and does not incorporate many of the operational variables that may contribute to future ED crowding, such as patient arrivals, inpatient capacity, staffing, acuity, or diagnostic delays.

The distribution of observations across NEDOCS states was also imbalanced, particularly toward higher crowding states, which may have influenced state-specific prediction accuracy.

Finally, forecasting performance declined as the prediction horizon increased, suggesting that the model is more useful for short-term prediction than longer-range forecasting.

## Future Work

Potential extensions of this project include:

* Comparing Markov forecasts against simple persistence and time-of-week baseline models
* Incorporating additional operational features associated with ED crowding
* Comparing Markov forecasting with other statistical and machine-learning approaches
* Evaluating alternative methods for handling state imbalance
* Investigating non-homogeneous and higher-order Markov models
* Evaluating the model using additional datasets or healthcare environments

## Project Background

This project was originally developed as undergraduate academic research and is presented here as an example of applied probability, predictive modeling, and healthcare analytics.

The public repository has been prepared as a portfolio version of the project. Healthcare data used in the original analysis is not distributed with the repository.

