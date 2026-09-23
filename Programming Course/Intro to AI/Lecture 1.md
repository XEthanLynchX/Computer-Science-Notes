## AI 2000: Introduction to AI Programming - Class 01

### Course Overview

- **Prerequisites:** Basic Python programming skills.
    
- **Goals:** Learn foundational data science concepts and artificial intelligence applications, including natural language processing (NLP), large language models (LLMs), machine vision, and pure data processing.
    
- **Approach:** This course skips traditional search algorithms (like depth-first search or A*) and moves directly into modern machine learning and data processing tools (e.g., PyTorch, TensorFlow).
    

### Simple NLP Binary Classification Model

- **Task:** Classify an input sentence into one of two categories: Food or Weather.
    
- **Mechanism:**
    
    1. Extract words from a provided dataset of food-related sentences and a dataset of weather-related sentences.
        
    2. Parse an input sentence and split it into individual words.
        
    3. Count the frequency of the input words in both the food and weather datasets.
        
    4. Classify the sentence based on the higher tally.
        

### Model Examples and Limitations

- **Success Case 1:** "We ordered pizza" scores 3 for food and 0 for weather. The model correctly classifies it as food.
    
- **Success Case 2:** "The wind was hard tonight" scores 2 for food and 5 for weather. The model correctly classifies it as weather. Words like "the" and "was" appear in both datasets and add to both tallies equally.
    
- **Failure Case:** "The coffee was ice cold" misclassifies as weather. The dataset lacks representation for "coffee," while "ice" and "cold" exist only in the weather dataset. The model lacks human context and relies entirely on the provided data.
    

### Key AI Principles

- **A model is a function with knobs:** The model computes answers based on settings rather than looking up facts in a repository.
    
- **Data acts as the knobs:** You alter the model's output by changing the training datasets, not the core algorithm.
    
- **Knowledge-Based vs. Data-Driven AI:**
    
    - **Knowledge-Based:** Humans write explicit rules (e.g., if-else statements defining specific food words).
        
    - **Data-Driven:** The model learns meanings by association through large datasets (e.g., neural networks). This course focuses on the data-driven approach.