---
layout: page
title: COVID-19 Diagnosis with Deep Learning from CT Scans
description: A novel CNN architecture, AdjCNet, for classifying COVID-19, CAP, and Normal cases from CT scans.
img: assets/img/covid_confusion_matrix.png
importance: 3
category: AI & Computer Vision
---

During the COVID-19 pandemic, rapid and accurate diagnosis was critical. This project addresses the challenge of differentiating COVID-19 from Community Acquired Pneumonia (CAP) and normal cases using chest CT scans. The core contribution is a novel CNN architecture, **AdjCNet**, which achieves superior performance through specialized design.

{% include figure.liquid
  path="assets/img/covid_ct_scans.png"
  alt="Examples of CT scans for COVID-19, CAP, and Normal patients."
  caption="Sample CT scans illustrating the visual similarities and differences between (a) COVID-19, (b) CAP, and (c) Normal lungs."
%}

### 1. The AdjCNet Architecture
The proposed model, AdjCNet, is a hybrid architecture designed to maximize classification accuracy.
- It uses **ResNet** as a powerful backbone for deep feature extraction.
- It incorporates a **Convolutional Block Attention Module (CBAM)** to help the model focus on the most salient regions within the CT scan.
- The novel component, **AdjCNet**, is a custom CNN layer that specifically analyzes grayscale variations among adjacent areas in the image, a key differentiator in pneumonia diagnostics.
This combined model achieved an outstanding classification accuracy of **99.23%**.

### 2. Rigorous Evaluation with Cross-Validation
To ensure the model's robustness and generalizability, its performance was evaluated using a **four-folds cross-validation** methodology. This process involves splitting the data into four parts, training on three, and testing on the remaining one, rotating until each part has been used for testing. The final model demonstrated a mean accuracy of **98.98%** and mean precision of **99.01%** across the four folds, proving its superiority over existing methods on the dataset.

{% highlight python %}
# Pseudocode for the evaluation process

FUNCTION Evaluate_Model_Performance(dataset):
    # Split data into 4 folds for cross-validation
    data_folds = split_into_k_folds(dataset, k=4)
    
    fold_accuracies = []
    fold_precisions = []
    
    FOR i FROM 1 TO 4:
        # Assign training and testing data for the current fold
        training_data = get_folds(data_folds, except=i)
        testing_data = get_fold(data_folds, i)
        
        # Build and train a new model instance
        model = build_AdjCNet_model()
        model.train(training_data)
        
        # Evaluate on the hold-out test set
        accuracy, precision = model.evaluate(testing_data)
        
        fold_accuracies.add(accuracy)
        fold_precisions.add(precision)
        
    mean_accuracy = mean(fold_accuracies)
    mean_precision = mean(fold_precisions)
    
    RETURN mean_accuracy, mean_precision
{% endhighlight %}

---
### Technical Summary
- **Model Components:** Custom CNN (AdjCNet), ResNet, CBAM
- **Frameworks:** TensorFlow, Keras
- **Evaluation:** 4-Fold Cross-Validation
- **My Role (CRediT):** Conceptualization, Design, Data collection, Analysis, Writing of the manuscript.