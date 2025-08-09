---
layout: page
title: Distributed Deep Learning for Anomaly Detection via Federated Learning
description: A privacy-preserving intrusion detection system using Federated Learning to train models on decentralized data.
img: assets/img/federated_learning_flowchart.png
importance: 4
category: Federated Learning & AI
---


Traditional centralized machine learning for intrusion detection faces significant privacy risks and processing demands, making it inefficient for large-scale networks. This project solves these issues by using **Federated Learning (FL)**, a technique that enables collaborative model training on decentralized data without ever exposing the raw data itself.

The system architecture was evaluated on three datasets from the **Canadian Institute for Cybersecurity (CIC)**, and this project proposed and analyzed three distinct FL algorithms: **Isolated-FL**, **Intra-FL**, and **Intra-Personalized-FL**.

---

### 1. The Intra-Federated Learning Architecture

The core of the system is a **Master-Slave architecture** that executes the FL training process. The goal is to build a robust, global Deep Neural Network (DNN) by aggregating knowledge from multiple clients (slaves) without centralizing their sensitive data. The entire process is visually represented by the flowchart and system diagram below.

<div class="row mt-4">
    <div class="col-sm-8">
        {% include figure.liquid path="assets/img/fl_master_model_diagram.png" class="img-fluid rounded z-depth-1" alt="Diagram showing the detailed FL system architecture." caption="System architecture for building the Master Model through aggregation." %}
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/fl_intra_federated_flowchart.png" class="img-fluid rounded z-depth-1" alt="Flowchart of the Intra-Federated Learning algorithm." caption="The Intra-FL algorithm flowchart." %}
    </div>
</div>

The step-by-step process, outlined in the pseudocode, is as follows:
1.  The **Master** node initializes a global DNN and distributes its weights.
2.  Each **Slave** node trains the model on its local dataset for a single epoch, then saves and sends its updated weights (as an `H5` file) back to the Master.
3.  The **Master** aggregates all incoming weights using a **weighted averaging** method to produce an improved global model.
4.  This process is repeated for a predefined number of epochs.

<div style="background-color: #f8f_config.ymla; border: 1px solid #e9ecef; padding: 15px; border-radius: 5px; margin-top: 1.5rem; margin-bottom: 2rem;">
  <h5 style="margin-top:0; border-bottom: 2px solid #dee2e6; padding-bottom: 10px; margin-bottom: 15px;">Algorithm Pseudocode</h5>
{% highlight plaintext %}
Require: S: Set of Slaves
Require: E: Number of epochs

Master Executes:
1  Create Master Model
2  Initialize w_0
3  For i ← 0,1,... E:
4    Send current Master Model weights to all slaves
5    Receive new updated weights from all slaves
6    Aggregate new weights using mean aggregation
7    Set Aggregated weights to Master Model weights
8  End For
9  Return Master Model

Distributed Slave Execution:
10 Receive weights from Master
11 Set weights to Local Model
12 Perform one round of training on Local Model
13 Send new weights to Master
{% endhighlight %}
</div>

---

### 2. Threshold-Based Classification and Results

The final, aggregated DNN produces an output score for incoming network traffic. This score is compared against a tunable **decision threshold** to classify the activity as either an attack or benign. The threshold's value directly impacts the trade-off between Precision and Recall, as illustrated in the performance chart.

<div class="row mt-4">
    <div class="col-sm-7">
        {% include figure.liquid path="assets/img/fl_threshold_chart.png" class="img-fluid rounded z-depth-1" alt="Chart showing Precision, Recall, and F1-Score at different thresholds." caption="Evaluation metrics showing the relationship between the decision threshold and model performance." %}
    </div>
    <div class="col-sm-5">
      <p style="margin-top: 1rem;">The table below shows the model's performance at various thresholds. Based on these results, a threshold of **0.6** was selected as the optimal balance, achieving a high F1-Score of 0.90 with 95% precision and 85% recall.</p>
      
      <div class="table-responsive">
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th style="text-align: center">Threshold</th>
      <th style="text-align: center">Precision</th>
      <th style="text-align: center">Recall</th>
      <th style="text-align: center">F1-Score Avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center">0.1</td>
      <td style="text-align: center">0.91</td>
      <td style="text-align: center">0.70</td>
      <td style="text-align: center">0.88</td>
    </tr>
    <tr>
      <td style="text-align: center">0.2</td>
      <td style="text-align: center">0.92</td>
      <td style="text-align: center">0.74</td>
      <td style="text-align: center">0.90</td>
    </tr>
    <tr>
      <td style="text-align: center">0.3</td>
      <td style="text-align: center">0.92</td>
      <td style="text-align: center">0.76</td>
      <td style="text-align: center">0.89</td>
    </tr>
    <tr>
      <td style="text-align: center">0.4</td>
      <td style="text-align: center">0.94</td>
      <td style="text-align: center">0.81</td>
      <td style="text-align: center">0.90</td>
    </tr>
    <tr>
      <td style="text-align: center">0.5</td>
      <td style="text-align: center">0.94</td>
      <td style="text-align: center">0.81</td>
      <td style="text-align: center">0.90</td>
    </tr>
    <tr class="table-primary">
      <td style="text-align: center"><strong>0.6</strong></td>
      <td style="text-align: center"><strong>0.95</strong></td>
      <td style="text-align: center"><strong>0.85</strong></td>
      <td style="text-align: center"><strong>0.90</strong></td>
    </tr>
    <tr>
      <td style="text-align: center">0.7</td>
      <td style="text-align: center">0.97</td>
      <td style="text-align: center">0.93</td>
      <td style="text-align: center">0.87</td>
    </tr>
    <tr>
      <td style="text-align: center">0.8</td>
      <td style="text-align: center">0.98</td>
      <td style="text-align: center">0.95</td>
      <td style="text-align: center">0.87</td>
    </tr>
    <tr>
      <td style="text-align: center">0.9</td>
      <td style="text-align: center">0.98</td>
      <td style="text-align: center">0.96</td>
      <td style="text-align: center">0.85</td>
    </tr>
  </tbody>
</table>
</div>
    </div>
</div>

<br>

---
### Technical Summary
- **Key Concepts:** Federated Learning, Distributed Systems, Privacy-Preserving ML
- **Datasets:** Canadian Institute for Cybersecurity (CIC) Datasets
- **Algorithm:** Intra-Federated Learning with Weighted Mean Aggregation
- **Model:** Deep Neural Network (DNN)