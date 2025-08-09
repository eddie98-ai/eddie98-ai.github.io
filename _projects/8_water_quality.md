---
layout: page
title: Predicting and Forecasting Water Quality
description: A deep learning system using LSTMs to predict and forecast the Water Quality Index (WQI).
img: assets/img/water_quality_factors.png
importance: 1
category: AI & Time Series
---

This project addresses the critical challenge of obtaining clean water by developing a system to predict and forecast the Water Quality Index (WQI). The core of the project is a **4-stacked LSTM model** designed to accurately forecast water quality, even with incomplete data, providing a vital tool for environmental management.

A full-stack **Django** admin dashboard was also developed to provide an interactive interface for monitoring water status and tracking key parameters in semi-real-time.

---

### 1. Forecasting with Stacked LSTMs

The primary prediction engine is a deep learning model composed of four stacked Long Short-Term Memory (LSTM) layers. This architecture was chosen for its proven ability to capture complex temporal dependencies in time-series data. The model was trained to forecast future WQI values, achieving a leading **RMSE of 0.013** on filtered data.

<div class="row mt-4">
    <div class="col-sm-8">
        {% include figure.liquid path="assets/img/wq_forecasting_chart.png" class="img-fluid rounded z-depth-1" alt="Graph showing predicted vs real WQI values." caption="The model's forecasting performance, showing the predicted WQI closely tracking the real WQI over time." %}
    </div>
    <div class="col-sm-4">
      <p style="margin-top: 1rem;">A key goal was to maintain accuracy with minimal testing. The table below shows the model's low error rates when predicting WQI even in the absence of certain tests.</p>
      <div class="table-responsive">
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th>Dataset (Test Omitted)</th>
      <th style="text-align: center">testing RMSE</th>
      <th style="text-align: center">testing MSE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Without total coliform</td>
      <td style="text-align: center">0.027</td>
      <td style="text-align: center">0.0008</td>
    </tr>
    <tr>
      <td>Without faecal coliform</td>
      <td style="text-align: center">0.068</td>
      <td style="text-align: center">0.0046</td>
    </tr>
    <tr>
      <td>Without dissolved oxygen</td>
      <td style="text-align: center">0.038</td>
      <td style="text-align: center">0.0014</td>
    </tr>
  </tbody>
</table>
</div>
    </div>
</div>

---

### 2. Interactive Monitoring Dashboard

To make the system practical, a full-stack admin dashboard was developed using **Django** and JavaScript. This interface provides a powerful tool for environmental management by allowing users to:
- Monitor the current water quality status.
- Track key parameters through interactive charts.
- View the model's real-time forecasts and historical performance.

<div class="row mt-3 mb-3">
    <div class="col-sm-8">
        {% include figure.liquid path="assets/img/wq_dashboard_rect_1.png" class="img-fluid rounded z-depth-1" alt="Rectangular screenshot of the Django dashboard." %}
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/wq_dashboard_square_1.png" class="img-fluid rounded z-depth-1" alt="Square screenshot of the Django dashboard." %}
    </div>
</div>
<div class="row mt-3 mb-3">
    <div class="col-sm-8">
        {% include figure.liquid path="assets/img/wq_dashboard_rect_2.png" class="img-fluid rounded z-depth-1" alt="Second rectangular screenshot of the Django dashboard." %}
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/wq_dashboard_square_2.png" class="img-fluid rounded z-depth-1" alt="Second square screenshot of the Django dashboard." %}
    </div>
</div>

<br>

---
### Technical Summary
- **ML Framework:** TensorFlow, Keras
- **Model:** 4-Stacked LSTM
- **Data Processing:** K-NN Imputation, Pandas
- **Backend & Visualization:** Django, JavaScript