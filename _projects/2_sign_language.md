---
layout: page
title: AI-Powered Sign Language & Assistive Technology
description: A real-time sign language translation and speech therapy platform.
img: assets/img/sign_language.png
importance: 2
category: AI & Machine Learning
permalink: /projects/2_sign_language/
---

This project focuses on making communication easier for people who are deaf or have speech disorders. It combines machine learning and computer vision to translate between sign language, spoken language, and text in real time.

{% include figure.liquid
  path="assets/img/sign_language_use_case.png"
  alt="Use Case diagram for the Sign Language assistive technology system."
  caption="Use Case Diagram illustrating the main actor interactions with the system."
%}



---

### Demonstration

<video width="50%" controls muted loop playsinline>
  <source src="{{ '/assets/video/sign_language_demo.mp4' | relative_url }}" type="video/mp4">
  Your browser does not support the video tag.
</video>

---

### 1. Real-Time Sign Language Recognition

This module takes video input from a camera and converts the observed sign language gestures into text.

It starts by using **MediaPipe Holistic**, a fast and lightweight pose estimation tool, to extract 3D landmarks from the hands, face, and body of the person signing. These landmarks are converted into a sequence of coordinates representing the movement and shape of gestures over time.

The sequence is then processed by a deep learning model. The model architecture includes:

- **1D Convolutional Layers** to capture short-term motion patterns.
- **Transformer Blocks** to understand the overall temporal structure and context of the gesture.

This allows the system to correctly classify even complex, time-dependent signs.

{% highlight python %}
def get_model(dim=192, dropout_step=0):
    inp = tf.keras.Input((MAX_LEN, CHANNELS))
    x = tf.keras.layers.Dense(dim, use_bias=False)(inp)
    x = tf.keras.layers.BatchNormalization(momentum=0.95)(x)
    x = Conv1DBlock(dim, ksize=17, drop_rate=0.2)(x)
    x = TransformerBlock(dim, expand=2)(x)
    x = tf.keras.layers.GlobalAveragePooling1D()(x)
    x = LateDropout(0.8, start_step=dropout_step)(x)
    x = tf.keras.layers.Dense(NUM_CLASSES, activation='softmax')(x)
    return tf.keras.Model(inp, x)
{% endhighlight %}

---

### 2. Text & Voice to Sign Language (Video Retrieval System)

This part of the system works in the opposite direction: the user provides **text** (by typing or speaking), and the system returns a sequence of **sign language videos** that express the same message.

Rather than matching each word directly to a video, this module understands the full sentence using sentence embeddings. It then retrieves the most relevant sign videos using a two-phase approach:

- **Offline Phase:** All sign language videos are clustered into topics (e.g., food, health, greetings). For each topic, a fast search index is created using tools like FAISS and precomputed video embeddings.

- **Online Phase:** When the user inputs a sentence, the system:
  1. Creates a vector representing the sentence.
  2. Finds the most related topic cluster.
  3. Breaks the sentence into phrases.
  4. For each phrase, it searches the topic-specific index for the best matching video.

The result is a smooth and context-aware translation from spoken or written language into sign video sequences.

{% highlight python %}
# Offline: Build topic-based index
FUNCTION Build_Hierarchical_Index(video_corpus):
    clusters = K_Means_Clustering(video_corpus)
    index = {}
    FOR topic IN clusters:
        videos = get_videos_for_cluster(topic)
        index[topic.id] = FAISS.build_index(embeddings_for(videos))
    RETURN index

# Online: Retrieve video sequence from text
FUNCTION CAHR_Retrieve_Sign_Sequence(text_query, text_model, index):
    embedding = text_model.predict(text_query)
    topic_id = find_closest_cluster(embedding, index.keys())
    sub_index = index[topic_id]
    sequence = []
    FOR phrase IN extract_key_phrases(text_query):
        emb = text_model.predict(phrase)
        video = sub_index.search(emb, K=1)
        sequence.add(video)
    RETURN sequence
{% endhighlight %}

---

### Technical Summary

- **ML Frameworks:** TensorFlow, Keras  
- **Pose Estimation:** MediaPipe Holistic  
- **ASR (Speech Recognition):** OpenAI Whisper  
- **Embedding Models:** Sentence-Transformers  
- **Backend:** FastAPI with modular microservices  

