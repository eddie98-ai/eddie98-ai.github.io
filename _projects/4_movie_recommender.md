---
layout: page
title: Generating Explainable Recommendations with Reinforcement Learning
description: A hybrid framework combining Biclustering and Q-Learning to solve the cold start and gray sheep problems in movie recommendations.
img: assets/img/reinforcement_learning_diagram.png
importance: 5
category: Intelligent Systems & User Experience
---

Traditional movie recommendation systems often fail with new users (**cold start problem**) or users with unique tastes (**gray sheep problem**). This project addresses these limitations by proposing a novel hybrid framework that integrates **Biclustering** with **Q-Learning-based Reinforcement Learning (RL)** to enhance accuracy, adaptability, and interpretability.

To demonstrate its real-world applicability, the algorithm was implemented in a **fully functional web-based platform**, allowing users to receive and understand their personalized movie suggestions.

<div class="row mt-3 mb-3">
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/rl_recommendation_output.png" class="img-fluid rounded z-depth-1" alt="Screenshot of the recommendation output." caption="The web platform's main recommendation interface." %}
    </div>
    <div class="col-sm-6">
        {% include figure.liquid path="assets/img/rl_recommendation_explanation.png" class="img-fluid rounded z-depth-1" alt="Screenshot of the recommendation explanation." caption="The system provides clear explanations for its suggestions." %}
    </div>
</div>

---


### 1. Hybrid Framework: Biclustering + Q-Learning

The system's intelligence comes from a two-stage process designed to handle sparse data and learn optimal policies over time:

1.  **Biclustering for Structure Discovery:** The system first applies biclustering to the MovieLens 100K dataset. Unlike traditional clustering, this technique groups both users and movies simultaneously based on their rating patterns. This effectively captures hidden relationships in the sparse data and organizes it into a structured grid network of user/movie clusters.

2.  **Q-Learning for Policy Optimization:** This structured grid is then used as the environment for a Reinforcement Learning agent. The agent's goal is to learn an optimal recommendation policy by navigating this grid. Instead of recommending a single movie, the agent learns to recommend an entire *bicluster* of movies, a much more effective strategy for handling cold-start users. The agent's learning is guided by **Jaccard similarity**, which serves as the reward signal for making relevant recommendations.

---

### 2. The Q-Learning Algorithm in Depth

The agent learns through trial and error using the Q-Learning algorithm, which continuously refines the agent's "Q-table" (a memory of the value of taking a certain action in a certain state). The process is guided by an **ε-greedy policy**, which balances exploiting known good recommendations with exploring new ones to ensure continuous improvement.

<div style="background-color: #f8f9fa; border: 1px solid #e9ecef; padding: 20px; border-radius: 5px; margin-top: 1rem; margin-bottom: 2rem;">
  <h5 style="margin-top:0; border-bottom: 2px solid #dee2e6; padding-bottom: 10px; margin-bottom: 15px;">Q-Learning Pseudocode</h5>
  <p>The core logic of the agent's learning process is captured by the following algorithm:</p>
  
  {% highlight plaintext %}
Algorithm 2 Pseudocode for Q-Learning
-------------------------------------------------------------------------------
Require: State Space S, Action Space A, Transition Function T, Agent
Ensure:  Policy π

1:  for i = 1 to trials do
2:      Select a random state s
3:      while step < max steps per episode or No game over do
4:          a = ξ-greedy with action
5:          Execute action a
6:          s' = T(s, a)
7:          Q_new = (1 - α) * Q_old + α * (r + γ * max Q_value_in_s')
8:          if No new movie found or Agent moves off the grid then
9:              game over = True
10:         else
11:             s = s'
12:         end if
13:     end while
14: end for
  {% endhighlight %}

  <h5 style="margin-top:20px; border-bottom: 2px solid #dee2e6; padding-bottom: 10px; margin-bottom: 15px;">How It Works</h5>
  <ul>
      <li><strong>State:</strong> The agent's current position in the grid of movie biclusters.</li>
      <li><strong>Action:</strong> Choosing which adjacent bicluster to recommend next.</li>
      <li><strong>Reward (r):</strong> A score based on the relevance of the recommended movies, calculated using Jaccard similarity to handle sparse data effectively.</li>
      <li><strong>Policy (π):</strong> The agent's strategy, which is continuously improved by updating the Q-table based on the rewards it receives from the environment.</li>
  </ul>
</div>

---

### 3. Results and Evaluation

Evaluation results show that the framework outperforms traditional recommendation models, particularly in handling cold start and gray sheep users, while maintaining a high level of recommendation accuracy and interpretability. The agent successfully learns to maximize its rewards over time, indicating an effective recommendation policy.

<div class="row mt-4">
    <div class="col-sm-12">
        {% include figure.liquid path="assets/img/rl_rewards_chart.png" alt="Chart showing rewards per episode during Q-Learning." caption="The agent's total reward per episode increases over time, showing a successful learning process." %}
    </div>
</div>
<div class="row mt-3">
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/rl_accuracy_chart.png" alt="Distribution of accuracy scores." caption="Distribution of Accuracy" %}
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/rl_recall_chart.png" alt="Distribution of recall scores." caption="Distribution of Recall" %}
    </div>
    <div class="col-sm-4">
        {% include figure.liquid path="assets/img/rl_precision_chart.png" alt="Distribution of precision scores." caption="Distribution of Precision" %}
    </div>
</div>

<br>

---
### Technical Summary
- **Algorithms:** Q-Learning, Biclustering
- **Key Concepts:** Reinforcement Learning, Explainable AI, Cold Start Problem
- **Dataset:** MovieLens 100K
- **Metrics:** Jaccard Similarity, Precision, Recall, Accuracy![alt text](image.png)