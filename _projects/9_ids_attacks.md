---
layout: page
title: Launching and Thwarting Attacks on AI-based Intrusion Detection Systems
description: Using GANs to generate polymorphic attacks for testing and improving the robustness of an IDS.
img: assets/img/ids_shap_features.png
importance: 2
category: AI & Cybersecurity
---

This project takes an offensive security approach to "think like an attacker." It leverages **Generative Adversarial Networks (GANs)** to simulate advanced, polymorphic DDoS attacks, with the goal of exposing weaknesses in AI-based Intrusion Detection Systems (IDS) and developing methods to re-optimize them.

{% include figure.liquid
  path="assets/img/ids_gan_results.png"
  alt="Bar chart showing the impact of GAN-based attacks on IDS accuracy."
  caption="Results showing how injecting malicious features (5_attack) significantly reduces IDS accuracy, and how re-training improves it (5_enh)."
%}

### 1. Feature Selection with SHAP
The project began by analyzing the massive CIC-IDS2017 dataset (80GB). To create effective attacks, it was crucial to identify the most impactful network features. The **SHAP (SHapley Additive exPlanations)** algorithm was utilized to analyze the feature importance, pinpointing the most distinguishing characteristics of DDoS attacks versus benign traffic.

### 2. Polymorphic Attack Generation with WGANs
A **Wasserstein Generative Adversarial Network (WGAN)** was trained to generate novel, realistic attack data.
- The **Generator** learns to create attack samples from a noise vector, attempting to mimic the statistical properties of real DDoS attacks identified by SHAP.
- The **Discriminator** (Critic) learns to differentiate between real and generated attacks.
Through this iterative, adversarial process, the Generator becomes proficient at creating evolving, polymorphic attack patterns that can evade simple detection rules.

{% highlight python %}
# Pseudocode for the Adversarial Attack Generation loop

FUNCTION Train_WGAN(real_attack_data):
    generator = initialize_generator()
    discriminator = initialize_discriminator()
    
    FOR EACH training iteration:
        # Train the Discriminator
        real_samples = sample(real_attack_data)
        generated_samples = generator.predict(noise_vector)
        d_loss = discriminator.train_on_batch(real_samples, generated_samples)
        
        # Train the Generator
        g_loss = generator.train_on_batch(noise_vector) # Goal is to fool the discriminator
        
    RETURN generator
{% endhighlight %}

### 3. Attack Simulation & IDS Dashboard
A full administrative dashboard was developed to operationalize the research. The dashboard allows a security analyst to:
- Generate new malicious datasets with a customizable number of attack features.
- Test both a standard IDS and an enhanced, re-trained IDS against the generated attacks.
- Visualize traffic flow characteristics and compare IDS performance side-by-side.

---
### Technical Summary
- **Key Concepts:** Generative Adversarial Networks (WGAN), Explainable AI (SHAP)
- **Dataset:** CIC-IDS2017
- **Frameworks:** TensorFlow, Keras, Scikit-learn
- **Dashboard:** Django