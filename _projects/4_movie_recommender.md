---
layout: page
title: Generating Explainable Recommendations with Reinforcement Learning
description: A hybrid framework combining Biclustering and Q-Learning to solve the cold start and gray sheep problems in movie recommendations.
img: assets/img/reinforcement_learning_diagram.png
importance: 5
category: AI & Reinforcement Learning
---

Many recommendation systems fail with new users (**cold start problem**) or users with unique tastes (**gray sheep problem**). This project tackles these challenges with a novel hybrid framework that uses **Reinforcement Learning (RL)** to create a smarter, more transparent, and adaptable recommendation engine.

{% include figure.liquid
  path="assets/img/rl_website_pages.png"
  alt="Screenshots of the web-based platform."
  caption="A fully functional web platform was developed to demonstrate the algorithm's real-world applicability, allowing users to receive personalized movie suggestions."
%}

### 1. Hybrid Framework: Biclustering + Q-Learning
The system's intelligence comes from a two-stage process:
1.  **Biclustering:** First, the MovieLens 100K dataset is analyzed to group both users and movies simultaneously based on rating patterns. This creates a structured grid of user/movie clusters, revealing hidden relationships in the sparse data.
2.  **Q-Learning:** This grid of clusters is then treated as the environment for a Reinforcement Learning agent. The agent's goal is to learn an optimal policy for navigating this grid. Instead of recommending a single movie, the agent learns to recommend an entire *bicluster* of movies, which is a much more effective strategy for new users.

### 2. The Q-Learning Algorithm
The agent learns through trial and error using the Q-Learning algorithm. It continuously explores the environment and updates its knowledge based on feedback.
- **State:** The agent's current position in the grid of movie biclusters.
- **Action:** Choosing which adjacent bicluster to recommend next.
- **Reward:** Positive feedback based on the relevance of the recommended movies (e.g., using Jaccard similarity).
- **Policy:** The agent uses an **ɛ-greedy policy**, where it mostly chooses the best-known action but sometimes explores new paths to discover better recommendation strategies over time.

{% highlight python %}
# Pseudocode for the Q-Learning training loop

FUNCTION Train_Q_Learning_Agent(trials):
    # Q_table stores the learned value of taking an action in a state
    Q_table = initialize_with_zeros()
    
    FOR i FROM 1 TO trials:
        state = select_random_state() # Start at a random movie cluster
        
        WHILE episode_is_not_over:
            # Choose action using an epsilon-greedy policy
            IF random_number < epsilon:
                action = choose_random_action() # Explore
            ELSE:
                action = choose_best_action_from_q_table(state) # Exploit
            
            # Execute action and get the reward and new state
            new_state, reward = environment.get_next_state_and_reward(state, action)
            
            # Update the Q-table with the new knowledge (Bellman equation)
            old_q_value = Q_table[state, action]
            future_optimal_value = max(Q_table[new_state])
            new_q_value = old_q_value + alpha * (reward + gamma * future_optimal_value - old_q_value)
            Q_table[state, action] = new_q_value
            
            state = new_state
            
    RETURN learned_policy_from(Q_table)
{% endhighlight %}

---
### Technical Summary
- **Algorithms:** Q-Learning, Biclustering
- **Key Concepts:** Reinforcement Learning, Explainable AI, Cold Start Problem
- **Dataset:** MovieLens 100K
- **Metrics:** Jaccard Similarity