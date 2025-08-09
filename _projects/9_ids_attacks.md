---
layout: page
title: Launching and Thwarting Attacks on AI-based Intrusion Detection Systems
description: Using GANs and SHAP to generate polymorphic attacks for testing and improving the robustness of an IDS.
img: assets/img/ids_usecase_diagram.png
importance: 2
category: AI for Cybersecurity & Privacy
---

This project takes an offensive security approach to "think like an attacker." It leverages **Generative Adversarial Networks (GANs)** to simulate advanced, polymorphic DDoS attacks, with the goal of exposing weaknesses in AI-based Intrusion Detection Systems (IDS) and developing methods to re-optimize them. The entire process, from data generation to testing, is managed via a full-stack **Django** dashboard.

---

### 1. Understanding Polymorphic Attacks

A polymorphic attack is an advanced cyber threat that constantly changes its identifiable features (like its code or data patterns) to evade detection. Unlike traditional attacks that have a fixed, recognizable signature, each new instance of a polymorphic attack looks different from the last, making it extremely difficult for signature-based or simple pattern-matching Intrusion Detection Systems to stop. This project uses GANs to artificially create this evolving, shape-shifting attack data to train a more robust IDS.

{% include figure.liquid path="assets/img/polymorphic_attack_explanation.png" class="img-fluid rounded z-depth-1" alt="Diagram explaining a polymorphic attack." caption="Illustration of a polymorphic attack changing its features to bypass defenses." %}

---

### 2. Data Preprocessing and Explainable Feature Selection

The project began by analyzing the massive **CIC-IDS2017 dataset (80GB)**. To create effective attacks, it was crucial to first identify the most impactful network features.

The **SHAP (SHapley Additive exPlanations)** algorithm was utilized to analyze feature importance, providing a clear, explainable ranking of the most distinguishing characteristics of DDoS attacks versus benign traffic. This allowed for a targeted approach to attack generation.

<div class="row mt-4">
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/ids_shap_diagram.png" class="img-fluid rounded z-depth-1" alt="Diagram of the SHAP feature selection process." caption="The iterative SHAP process for selecting the most impactful features." %}
    </div>
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/ids_shap_barchart.png" class="img-fluid rounded z-depth-1" alt="Bar chart showing the SHAP values for top DDoS features." caption="SHAP results identifying the most significant features for DDoS attacks." %}
    </div>
</div>

---

### 3. Polymorphic Attack Generation with WGANs

A **Wasserstein Generative Adversarial Network (WGAN)** was trained to generate novel, realistic attack data based on the features selected by SHAP. The WGAN architecture consists of two competing models:
- The **Generator** learns to create attack samples from a noise vector, attempting to mimic the statistical properties of real DDoS attacks.
- The **Discriminator** (or Critic) learns to differentiate between real and generated attacks, providing feedback to the Generator.

Through this iterative, adversarial process, the Generator becomes proficient at creating evolving, polymorphic attack patterns.

<div style="background-color: #f8f9fa; border: 1px solid #e9ecef; padding: 20px; border-radius: 5px; margin-top: 1.5rem; margin-bottom: 2rem;">
  <h5 style="margin-top:0; border-bottom: 2px solid #dee2e6; padding-bottom: 10px; margin-bottom: 15px;">Adversarial Attack Generation Algorithm</h5>
  {% highlight plaintext %}
Algorithm 2: Adversarial Attack Generation
-------------------------------------------------------------------------------
Input:
    Generator - noise vector N, DDoS Attack Data
    Critic/Discriminator - attack, and benign data
Output:
    Trained Generator
    Trained Critic / Discriminator

loop:
    Generate attack
    Update loss function "G_loss"
    
    while <Discriminator iterations> do
        Update loss function "D_loss"
    
    Send feedback to Generator and to Discriminator
end loop
  {% endhighlight %}
</div>

---

### 4. Attack Simulation and IDS Enhancement

We launched attacks against a baseline AI-based IDS by injecting the 5 and 10 most malicious features identified by SHAP. As the results show, this significantly reduced the detection accuracy.

We then improved the AI-based IDS by re-training it on a dataset augmented with our generated polymorphic attacks. This enhancement significantly increased its ability to detect these advanced threats.

<div class="row mt-4">
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/ids_results_chart_1.png" class="img-fluid rounded z-depth-1" alt="Bar chart showing the impact of GAN-based attacks on IDS accuracy." caption="Injecting 5 and 10 malicious features causes a noticeable drop in detection rate across multiple ML models." %}
    </div>
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/ids_results_chart_2.png" class="img-fluid rounded z-depth-1" alt="Bar chart showing IDS performance before and after enhancement." caption="Model accuracy before enhancement (5_attack) and after enhancement (5_enh) using 5 malicious features." %}
    </div>
</div>

---

### 5. Interactive Admin Dashboard

To make the system practical, a full administrative dashboard was developed using **Django**. This interface allows a security analyst to:
- Generate new malicious datasets with a customizable number of attack features.
- Test both a standard IDS and an enhanced, re-trained IDS against the generated attacks.
- Visualize traffic flow characteristics and compare IDS performance side-by-side.

<div class="row mt-3 mb-3">
    <div class="col-sm-7">
        {% include figure.liquid path="assets/img/ids_dashboard_1.png" class="img-fluid rounded z-depth-1" alt="Screenshot of the adversarial data generation interface." %}
    </div>
    <div class="col-sm-5">
        {% include figure.liquid path="assets/img/ids_dashboard_2.png" class="img-fluid rounded z-depth-1" alt="Screenshot of the IDS testing interface." %}
    </div>
</div>

<br>

---
### Technical Summary
- **Key Concepts:** Generative Adversarial Networks (WGAN), Explainable AI (SHAP), Offensive Security, Polymorphic Attacks
- **Dataset:** CIC-IDS2017
- **Frameworks:** TensorFlow, Keras, Scikit-learn
- **Dashboard:** Django