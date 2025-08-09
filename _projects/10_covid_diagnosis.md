---
layout: page
title: COVID-19 Diagnosis with Deep Learning from CT Scans
description: A novel CNN architecture, AdjCNet, for classifying COVID-19, Pneumonia, and Normal cases from CT scans.
img: assets/img/covid_confusion.png
importance: 3
category: AI for Healthcare & Life Sciences
---

During the COVID-19 pandemic, a primary challenge was the frequent misdiagnosis of COVID-19 as normal pneumonia, leading to delayed treatments and increased transmission. This project addresses this critical issue by proposing a novel deep learning architecture for the accurate and rapid classification of chest CT scans.

The core contribution is a hybrid CNN, **AdjCNet**, which combines state-of-the-art techniques with a novel component to achieve superior diagnostic performance.

---

### 1. The AdjCNet Architecture

The proposed model, AdjCNet, is a hybrid architecture designed to maximize classification accuracy by leveraging multiple techniques:
- It uses a **ResNet** backbone for powerful, deep feature extraction from the CT scan images.
- It incorporates a **Convolutional Block Attention Module (CBAM)** to help the model focus on the most salient and diagnostically relevant regions within the scan.
- The novel component, **AdjCNet**, is a custom CNN layer that specifically analyzes **grayscale variations among adjacent areas** in the image. This is a key differentiator, as subtle textural changes are often indicative of specific types of lung inflammation.

This combined model achieved an outstanding overall classification accuracy of **99.23%**.

{% include figure.liquid
  path="assets/img/covid_ct_scans.png"
  alt="Examples of CT scans for COVID-19, CAP, and Normal patients."
  caption="Sample CT scans illustrating the visual differences between (a) COVID-19, (b) Community Acquired Pneumonia (CAP), and (c) Normal lungs."
%}

---

### 2. Rigorous Evaluation and Results

To ensure the model's robustness and generalizability, its performance was evaluated using a **four-folds cross-validation** methodology. This process prevents overfitting and provides a more reliable measure of the model's performance on unseen data.

<div class="row mt-4">
    <div class="col-sm-7">
        {% include figure.liquid path="assets/img/covid_confusion_matrix.png" class="img-fluid rounded z-depth-1" alt="Confusion matrix of the model's predictions." caption="Confusion matrix showing the classification results for COVID-19, CAP, and Normal cases." %}
    </div>
    <div class="col-sm-5">
      <p style="margin-top: 1rem;">The confusion matrix provides a detailed breakdown of the model's performance. The strong diagonal from top-left to bottom-right indicates a high number of correct predictions. Specifically, the model correctly identified <strong>1880 COVID-19 cases</strong> while only misclassifying 17, and achieved a near-perfect result for Community Acquired Pneumonia (CAP), with <strong>655 correct predictions</strong> and only 1 error. This demonstrates the model's high sensitivity and precision.</p>
      
      <p>This strong performance is further validated by the four-folds cross-validation results, shown in the table below, which demonstrate a final mean accuracy of <strong>98.98%</strong>.</p>
      
      <div class="table-responsive">
      <table class="table table-striped table-bordered">
        <thead>
          <tr>
            <th style="text-align: center">Fold</th>
            <th style="text-align: center">Accuracy</th>
            <th style="text-align: center">Precision</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td style="text-align: center">1</td>
            <td style="text-align: center">0.9855</td>
            <td style="text-align: center">0.9867</td>
          </tr>
          <tr>
            <td style="text-align: center">2</td>
            <td style="text-align: center">0.9923</td>
            <td style="text-align: center">0.9921</td>
          </tr>
          <tr>
            <td style="text-align: center">3</td>
            <td style="text-align: center">0.9934</td>
            <td style="text-align: center">0.9935</td>
          </tr>
          <tr>
            <td style="text-align: center">4</td>
            <td style="text-align: center">0.9880</td>
            <td style="text-align: center">0.9882</td>
          </tr>
          <tr class="table-primary">
            <td style="text-align: center"><strong>Mean</strong></td>
            <td style="text-align: center"><strong>0.9898</strong></td>
            <td style="text-align: center"><strong>0.9901</strong></td>
          </tr>
        </tbody>
      </table>
      </div>
    </div>
</div>

<br>

---
### Technical Summary
- **Model Components:** Custom CNN (AdjCNet), ResNet, CBAM
- **Frameworks:** TensorFlow, Keras
- **Evaluation:** 4-Fold Cross-Validation
- **My Role (CRediT):** Conceptualization, Design, Data collection, Analysis, Writing of the manuscript.