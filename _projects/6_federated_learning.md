---
layout: page
title: Distributed Deep Learning for Anomaly Detection via Federated Learning
description: A privacy-preserving intrusion detection system using Federated Learning to train models on decentralized data.
img: assets/img/federated_learning_flowchart.png
importance: 4
category: Federated Learning & AI
---

Traditional centralized machine learning for intrusion detection faces significant privacy risks and processing bottlenecks. This project solves these issues by using **Federated Learning (FL)**, a technique that enables collaborative model training on decentralized data without ever exposing the raw data itself. The project explored three distinct FL algorithms: Isolated-FL, Intra-FL, and Intra-Personalized-FL.

{% include figure.liquid
  path="assets/img/federated_learning_architecture.png"
  alt="Diagram showing the master-slave architecture of the Federated Learning system."
  caption="The FL architecture: A central Master node coordinates training, while multiple Slave nodes train locally on their private datasets."
%}

### 1. The Intra-Federated Learning Algorithm
The core of the system is a Master-Slave architecture that executes the FL process over multiple epochs.
- **Master Node:** Initializes the global model and distributes its weights to all slave nodes. After each training round, it aggregates the updated weights from the slaves to create an improved global model.
- **Slave Nodes:** Each slave holds its own private dataset. It receives the global model weights, performs one round of training locally on its data, and sends only the updated weights (not the data) back to the Master.

This process repeats, allowing the global model to learn from the collective knowledge of all slaves without compromising data privacy.

{% highlight python %}
# Pseudocode for the Federated Learning Process

# --- Master Node Execution ---
FUNCTION Master_Execute(slaves, epochs):
    global_model = initialize_model()
    FOR i FROM 1 TO epochs:
        # Distribute current model weights to all slaves
        broadcast_weights(global_model.weights, to=slaves)
        
        # Wait to receive updated weights from all slaves
        updated_weights_list = receive_from_all(slaves)
        
        # Aggregate the updates to improve the global model
        new_weights = mean_aggregate(updated_weights_list)
        global_model.set_weights(new_weights)
        
    RETURN global_model

# --- Slave Node Execution ---
FUNCTION Slave_Execute(local_data):
    # Receive global model weights from Master
    model_weights = receive_from_master()
    local_model = build_model()
    local_model.set_weights(model_weights)
    
    # Train for one round on local, private data
    local_model.train(local_data, epochs=1)
    
    # Send only the updated weights back to the Master
    send_weights_to_master(local_model.weights)
{% endhighlight %}

### 2. Threshold-Based Classification
The final, aggregated Deep Neural Network (DNN) produces an output score for incoming network traffic. This score is compared against a tunable threshold (e.g., 0.6) to classify the activity. A higher threshold prioritizes **precision** (fewer false positives), while a lower threshold prioritizes **recall** (detecting more potential attacks), allowing for flexible deployment based on security needs.

---
### Technical Summary
- **Key Concepts:** Federated Learning, Distributed Systems, Privacy-Preserving ML
- **Algorithm:** Weighted Mean Aggregation
- **Model:** Deep Neural Network (DNN)