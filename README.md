# ENGG2112-Extreme-Weather
Extreme-weather forecasting using Random Forest and Logistic Regression

# Extreme Weather Prediction with Machine Learning

An ENGG2112 “AI for Good” project investigating whether machine learning can support earlier warnings of extreme heat and heavy rainfall.

We use Random Forest as our main model and logistic regression as a comparison. Both models use archived weather forecasts, with additional recent weather observations used by logistic regression.

## Project Objective

Our goal is to predict extreme weather with at least **70% precision and 70% recall**.

An extreme day is defined as either:

- Maximum temperature of **35°C or higher**, or
- Total rainfall of **50 mm or higher**.

Each prediction covers a 24-hour window beginning **24 hours after the prediction issue time**.

These thresholds define the events studied in this project. They are not universal evacuation thresholds.

## Data

The project dataset contains **7,211 city-days** from May 2021 to August 2025 across:

- Sydney
- Tokyo
- Hong Kong
- Madrid
- Auckland

| Data | Source |
|---|---|
| Archived GFS weather forecasts | [Dynamical](https://dynamical.org/) |
| Historical weather observations | [NOAA Integrated Surface Database](https://www.ncei.noaa.gov/products/land-based-station/integrated-surface-database) |
| Sydney daily rainfall observations | [NOAA Global Historical Climatology Network Daily](https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily) |

The report provides further details about station selection, processing and data limitations.

## Method

1. Align observations and forecasts by city and time.
2. Create extreme-weather labels from the future target window.
3. Build features from forecasts and available recent observations.
4. Split the data chronologically into training, validation and test sets.
5. Fit missing-value imputation using training data only, with additional standardisation for logistic regression.
6. Train Random Forest and logistic regression.
7. Select decision thresholds using the 2024 validation data.
8. Evaluate the selected models on the held-out 2025 data.

Development included comparing candidate models and feature sets using validation data. The final notebook reproduces the selected models rather than repeating the full development search.

Direct forecast rules are used as baselines to assess whether machine learning improves on the forecast inputs.

## Evaluation Design

| Dataset | City-days | Extreme days |
|---|---:|---:|
| Training | 4,534 | 267 |
| Validation: 2024 | 1,571 | 82 |
| Primary test: four cities in 2025 | 868 | 76 |
| Sydney 2025: exploratory only | 228 | 6 |

Sydney's 2025 results are reported separately because they had already been viewed during development.

## Main Results

Results on the pooled four-city test set:

| Method | Precision | Recall | Correct warnings | False alarms | Missed events |
|---|---:|---:|---:|---:|---:|
| Random Forest | 74.0% | 75.0% | 57 | 20 | 19 |
| Logistic regression | 70.1% | 71.1% | 54 | 23 | 22 |
| Direct forecast baseline | 85.5% | 69.7% | 53 | 9 | 23 |
| Validation-adjusted forecast baseline | 78.9% | 73.7% | 56 | 15 | 20 |

Both machine learning models achieved the 70% precision and recall targets on the **pooled test set**.

However, Random Forest detected only one more event than the adjusted forecast baseline while producing five more false alarms. These results do not establish that Random Forest is the best option for every warning system.

## Important Limitations

- **Performance varies substantially by city and hazard.** Random Forest detected 56 of 63 heat events but only 1 of 13 rainfall events.
- **Madrid strongly influences the pooled results.** Outside Madrid, Random Forest detected 8 of 27 extreme events.
- **Forecast age differs between development and testing**, which limits direct comparisons.
- Historical observation availability at prediction time was not independently verified.
- Daily event detection does not establish how reliably the model warns before the start of a new weather episode.
- The project does not demonstrate reliable performance in cities outside the evaluated dataset.

This is an academic prototype for investigating decision support. It is not an operational warning or evacuation system.

## Running the Notebook in VS Code

### 1. Download the project

On GitHub, select **Code → Download ZIP**, then extract the folder.

Alternatively:

```bash
git clone https://github.com/tarnished498/ENGG2112-Extreme-Weather.git
cd ENGG2112-Extreme-Weather
```

### 2. Install the VS Code extensions

Install Microsoft's **Python** and **Jupyter** extensions.

### 3. Create a Python environment

For macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

For Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 4. Install the main analysis packages

```bash
python -m pip install pandas numpy scikit-learn matplotlib jupyter ipykernel
```

### 5. Open and run the notebook

Open the project folder in VS Code, then open the project `.ipynb` notebook.

Click **Select Kernel**, choose the `.venv` environment, and run the cells in order.

Keep the supplied CSV files in the locations referenced by the notebook. Check the data-loading cell if a file cannot be found.

## Report and AI Assistance

The final report explains the methodology, results, ethical considerations and proposed use of the system.

AI assistance with coding, analysis and report drafting is documented in the report's appendix.
