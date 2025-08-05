---
layout: page
title: Predicting and Forecasting Water Quality
description: A deep learning system using LSTMs to predict and forecast the Water Quality Index (WQI).
img: assets/img/water_quality_factors.png
importance: 1
category: AI & Time Series
---

This project addresses the critical challenge of water pollution by developing a system to monitor and forecast water quality. The core of the project is a **4-stacked LSTM model** designed to predict the Water Quality Index (WQI) based on various environmental factors, providing an early warning system for contamination.

{% include figure.liquid
  path="assets/img/water_quality_results.png"
  alt="Graph showing predicted vs real WQI values."
  caption="Sample of forecasting results, showing the model's ability to track real WQI fluctuations."
%}

### 1. Data Processing and Feature Engineering
The initial dataset contained multiple factors influencing water quality (pH, turbidity, dissolved oxygen, etc.). A key challenge was handling missing data points. This was addressed by applying **K-NN imputation** to fill gaps intelligently based on the nearest neighbors in the feature space. Further stability was achieved using an **annual mean filter** to smooth the time-series data.

### 2. Forecasting with Stacked LSTMs
The primary prediction engine is a deep learning model composed of four stacked Long Short-Term Memory (LSTM) layers. This architecture was chosen for its proven ability to capture complex temporal dependencies in time-series data. The model was trained to forecast future WQI values, achieving a leading **RMSE of 0.013**.

{% highlight python %}
# High-level pseudocode for the forecasting pipeline

FUNCTION Forecast_WQI(raw_data):
    # 1. Prepare data for the time-series model
    processed_data = K_NN_Impute(raw_data)
    smoothed_data = Annual_Mean_Filter(processed_data)
    
    # 2. Build and train the LSTM model
    model = build_stacked_lstm_model(layers=4)
    model.train(smoothed_data)
    
    # 3. Generate future predictions
    future_wqi = model.predict(last_known_data)
    
    RETURN future_wqi
{% endhighlight %}

### 3. Interactive Monitoring Dashboard
To make the system practical, a full-stack admin dashboard was developed using **Django** and JavaScript. This interface allows users to monitor real-time water quality status, track key parameters through interactive charts, and view the model's forecasts, providing a powerful tool for environmental management.

---
### Technical Summary
- **ML Framework:** TensorFlow, Keras
- **Model:** 4-Stacked LSTM
- **Data Processing:** K-NN Imputation, Pandas
- **Backend & Visualization:** Django, JavaScript