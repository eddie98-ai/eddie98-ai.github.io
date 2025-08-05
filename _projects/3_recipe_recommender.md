---
layout: page
title: Smart Recipe Recommendation System
description: A full-stack Django app recommending recipes based on ingredients and health needs.
img: assets/img/recipe_recommender.png
importance: 3
category: Full-Stack & AI
---

This project is a full-stack Django web application that delivers personalized recipe recommendations. It was built upon a unique, fused dataset combining over 400 thousand global recipes with a custom-collected corpus of Syrian cuisine. The system leverages a hybrid recommendation engine and a nutritional analysis pipeline to provide meal suggestions that are not only tailored to a user's available ingredients but also to their specific health and dietary requirements.

---

<!-- 1. The Styles (CSS) -->
<style>
  .slider-wrapper {
    position: relative;
    width: 100%;
    margin: 1rem auto;
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }
  .slider {
    display: flex;
    /* The transition creates the smooth sliding effect */
    transition: transform 0.5s ease-in-out;
  }
  .slider img {
    width: 100%;
    flex-shrink: 0;
    display: block;
  }
  .slider-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.5);
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 50%;
    cursor: pointer;
    font-size: 18px;
    font-weight: bold;
    user-select: none; /* Prevents text selection on double click */
    z-index: 10;
    transition: background-color 0.2s;
  }
  .slider-btn:hover {
    background-color: rgba(0, 0, 0, 0.8);
  }
  #prevBtn {
    left: 15px;
  }
  #nextBtn {
    right: 15px;
  }
</style>

<!-- 2. The Slider Structure (HTML) -->
<div class="slider-wrapper">
  <!-- The slider container that moves -->
  <div class="slider">
    <img src="/assets/img/web-prochour/slider1.jpg" alt="Recipe Image 1">
    <img src="/assets/img/web-prochour/slider2.jpg" alt="Recipe Image 2">
    <img src="/assets/img/web-prochour/slider3.jpg" alt="Recipe Image 3">
    <img src="/assets/img/web-prochour/slider4.jpg" alt="Recipe Image 4">
  </div>
  <!-- Manual Arrow Buttons -->
  <div class="slider-btn" id="prevBtn">❮</div>
  <div class="slider-btn" id="nextBtn">❯</div>
</div>

<!-- 3. The Functionality (JavaScript) -->
<script>
document.addEventListener("DOMContentLoaded", function() {
  const slider = document.querySelector('.slider');
  // Important: Get all images inside the slider
  const slides = document.querySelectorAll('.slider img');
  const prevBtn = document.querySelector('#prevBtn');
  const nextBtn = document.querySelector('#nextBtn');

  let currentIndex = 0;
  // Store the interval timer so we can reset it
  let autoSlideInterval;

  // Function to move the slider to the correct position
  function updateSlider() {
    slider.style.transform = `translateX(-${currentIndex * 100}%)`;
  }

  // Function to handle moving to the next slide
  function nextSlide() {
    // The modulo operator (%) creates the infinite loop
    currentIndex = (currentIndex + 1) % slides.length;
    updateSlider();
  }

  // Function to handle moving to the previous slide
  function prevSlide() {
    // This formula correctly handles looping backwards from 0
    currentIndex = (currentIndex - 1 + slides.length) % slides.length;
    updateSlider();
  }

  // Function to start or reset the automatic sliding timer
  function resetTimer() {
    // Clear any existing timer
    clearInterval(autoSlideInterval);
    // Start a new one
    autoSlideInterval = setInterval(nextSlide, 6000); // 6000ms = 6 seconds
  }

  // Add event listeners for the arrow buttons
  nextBtn.addEventListener('click', () => {
    nextSlide();
    resetTimer(); // Reset the timer whenever the user clicks
  });

  prevBtn.addEventListener('click', () => {
    prevSlide();
    resetTimer(); // Reset the timer whenever the user clicks
  });

  // Start the automatic slideshow when the page loads
  resetTimer();
});
</script>
---

### System Architecture & Features

The application is architected around three core modules: a hybrid recommendation engine, a nutritional analysis pipeline, and an AI-powered health chatbot.

#### 1. Hybrid Recommendation Engine

To provide robust recommendations, the system combines two distinct methods: content-based filtering for granular matching and set-based similarity for broader suggestions.

*   **Content-Based Filtering via Cosine Similarity:**
    This method treats each recipe as a document and its ingredients as terms. Ingredients are converted into numerical vectors using a TF-IDF (Term Frequency-Inverse Document Frequency) approach. The similarity between two recipes is then calculated as the cosine of the angle between their vectors, providing a sophisticated measure of ingredient overlap and importance.

    The similarity score is calculated using the formula:
    $$ \text{similarity} = \cos(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|} = \frac{\sum_{i=1}^{n} A_i B_i}{\sqrt{\sum_{i=1}^{n} A_i^2} \sqrt{\sum_{i=1}^{n} B_i^2}} $$

*   **Set-Based Similarity via Jaccard Index:**
    This method provides a faster, broader comparison by treating the ingredients of two recipes as sets. The Jaccard Index measures the similarity between these sets, defined as the size of the intersection divided by the size of the union. It is particularly effective for finding recipes with common core ingredients.

    The Jaccard Index is calculated as:
    $$ J(A, B) = \frac{|A \cap B|}{|A \cup B|} $$

#### 2. Nutritional Analysis Pipeline

This module acts as a powerful filtering layer on top of the recommendation engine. For each recipe, the system aggregates the nutritional profile (salt, carbohydrates, fat, free sugars, protein) from its constituent ingredients. When a user specifies a health condition (e.g., hypertension, diabetes), this pipeline filters the recommended recipes to ensure they comply with dietary guidelines, such as being low in sodium or sugar.

#### 3. Specialized Health Q&A Chatbot

A dedicated chatbot was developed to answer user questions about healthy eating and nutrition. To ensure factual and safe responses, it was built using a Retrieval-Augmented Generation (RAG) approach with open-source Hugging Face models.

*   **Prompt Engineering:** When a user asks a question, the system first retrieves relevant, pre-vetted nutritional facts from a curated knowledge base.
*   **Contextual Injection:** These facts are then injected directly into a carefully crafted prompt for a generative language model. This grounds the model's response in factual data, preventing hallucination and ensuring the advice is focused and reliable.

---

### Technology Stack
*   **Backend:** Django
*   **ML & Data Science:** Scikit-learn, Pandas, NLTK
*   **NLP & Chatbot:** Hugging Face Transformers (for models and embeddings)
*   **Database:** SQLite