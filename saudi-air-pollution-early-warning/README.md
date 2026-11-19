# Saudi Air Pollution Early Warning System

A machine learning experiment to predict whether PM2.5 in **Riyadh, Saudi Arabia** will exceed an **experimental threshold of 35 µg/m³ exactly 24 hours later**.

## Problem

Elevated particulate pollution can affect outdoor activity planning. The aim is to investigate whether historical air-quality and weather variables help flag potential threshold exceedances a day in advance.

## Dataset

The project uses a 2024 Riyadh hourly air-quality and weather dataset generated with the accompanying data-download notebook. The data are sourced from **Open-Meteo's modeled air-quality and weather products**, not necessarily measurements from Saudi ground stations.

Variables include PM2.5, PM10, temperature, relative humidity, wind speed, hour, and month. The 24-hour-ahead target is built by matching timestamps rather than shifting arbitrary row positions.

**The Excel dataset is not included in this repository.** Run `notebooks/download_data.ipynb` to create `Riyadh_Air_Quality_2024_Real_Data.xlsx`, then upload it when prompted in the modeling notebook.

Data documentation: [Open-Meteo Air Quality API](https://open-meteo.com/en/docs/air-quality-api) and [Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api).

## Method

1. Clean and inspect hourly data.
2. Create a binary target: whether PM2.5 exceeds **35 µg/m³** at **t + 24 h**.
3. Create past-reading and rolling-mean features.
4. Split chronologically into training, validation and test sets, with a 24-hour target gap at boundaries.
5. Compare Logistic Regression, Random Forest and a persistence baseline (the current category is assumed to persist).
6. Select by validation F1 and evaluate on the held-out test period.

## Results

The **persistence baseline** was selected in the completed experiment. Reported held-out test results:

| Metric | Result |
|---|---:|
| Accuracy | 0.86 |
| High-pollution precision | 0.92 |
| High-pollution recall | 0.91 |
| High-pollution F1 | 0.914 |
| Baseline test F1 | 0.914 |

There were **1,458 positive cases out of 1,746 test samples** (approximately 83.5%), so accuracy alone is not sufficient to assess warning quality. Neither trained ML model surpassed the selected baseline in this experiment. Exact validation scores for the other models are produced when the notebook is run; they are not invented here.

## Run in Google Colab

1. Open `notebooks/download_data.ipynb` in Google Colab and run all cells to download and export the Excel dataset.
2. Open `notebooks/air_pollution_prediction.ipynb` in Google Colab.
3. Run the cells in order, uploading the generated Excel file when requested.
4. Review the validation comparison, classification report and confusion matrix.
5. Download the generated `air_quality_outputs` folder if you want to archive your own reproducible metrics.

Install dependencies locally with `pip install -r requirements.txt` (the notebooks are designed primarily for Colab).

## Limitations

- The threshold is **experimental**, not an official Saudi warning threshold. A single hourly concentration must not be confused with a 24-hour-average regulatory standard.
- These are modeled environmental data; performance on official station measurements is not established.
- The selected baseline predicts **a category**, not a future numeric PM2.5 concentration or a calibrated probability.
- The test set is imbalanced and represents only one held-out time period.
- This is an educational prototype, **not a live air-quality alert service**.

## Tools

Python, Pandas, NumPy, Matplotlib, scikit-learn, Joblib and Google Colab.

## Author

Eman Mobark — Computer Engineering
