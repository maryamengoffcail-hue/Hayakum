# OPERA — AI Travel & Event Intelligence System

## Overview

OPERA is an AI-powered travel and event intelligence system designed to simplify how users discover, evaluate, and plan experiences.

Instead of manually browsing multiple platforms, OPERA combines:

* Event data collection (scraping + structured extraction)
* Machine learning-based recommendation (NCF model)
* AI-driven ranking and filtering
* Personalized itinerary generation

The goal is to move from **information overload → personalized experience planning in under minutes**.

---

# System Architecture

OPERA is built using **3 core intelligent layers (agents)**:

## 1. Data Collection Agent (Scraper Layer)

### Purpose:

Collect structured event data from multiple sources.

### Data Sources:

* Event platforms (Eventbrite, Timeout, Songkick, etc.)
* GEMINI AI to search for events
* Web listings and event pages

### Responsibilities:

* Fetch raw HTML pages
* Extract event candidates (title, date, venue, links)
* Clean and normalize text
* Remove noise (ads, navigation, duplicates)

### Output:

Structured event dataset:

```json
{
  "title": "AI Conference Riyadh",
  "date": "2026-06-10",
  "venue": "Riyadh Front",
  "category": "Tech",
  "source": "eventbrite"
}
```

---

## 2. Recommendation Engine (NCF Model)

### Model Type:

Neural Collaborative Filtering (NCF)

### Purpose:

Predict user interest in events using learned behavioral patterns.

---

### Core Idea:

Instead of manually defining preferences, the system learns:

* User behavior patterns → via embeddings
* Event characteristics → via embeddings
* Interaction probability → via neural network

---

### Embeddings

An embedding is a **learned dense vector representation** of:

* Users (preferences, behavior style)
* Events (type, category, attractiveness)

Example:

```
User 12 → [0.21, -0.8, 0.44, ...]
Event 55 → [-0.1, 0.9, 0.33, ...]
```

These vectors are not manually defined — they are learned from interaction data.

---

### Model Architecture

```
User ID ──► Embedding ┐
                      ├──► Concatenate ─► Dense Layers ─► Sigmoid ─► Score
Event ID ─► Embedding ┘
```

---

### Code Components

* `nn.Embedding` → converts IDs into vectors
* Fully connected layers → learn interaction patterns
* Sigmoid → outputs probability of interest

---

### Training Strategy

* Implicit feedback dataset (clicks, interactions)
* Negative sampling (user did not interact)
* Leave-one-out evaluation split

---

### Output:

Each event receives a score:

```
Event A → 0.91 (high interest)
Event B → 0.34 (low interest)
```

---

## 3. AI Personal Planner (Decision Layer)

### Purpose:

Convert ranked events into a personalized itinerary.

---

### Inputs:

* Recommended events (from NCF model)
* User constraints:

  * Budget
  * Group size
  * Experience level
  * Location preference
  * Category preferences

---

### Logic:

Each event is scored using:

### 1. Category Fit

Matches user interests

### 2. Budget Fit

Penalizes expensive events

### 3. Experience Level Matching

(beginner / intermediate / advanced)

### 4. Location Preference

(online vs in-person weighting)

### 5. ROI Score

Value-to-cost ratio

### 6. Model Confidence

NCF prediction score contribution

---

### Final Score Formula:

```
Total Score =
Category Score +
Budget Score +
Location Score +
Experience Score +
ROI Score +
Model Confidence Score
```

---

### Output:

A ranked personalized plan:

```
1. AI Bootcamp Riyadh
   Cost: $120
   ROI: High
   Fit: Intermediate

2. Tech Meetup
   Cost: $25
   ROI: Medium
   Fit: Beginner
```

---

## Data Flow

```
Web Sources
    ↓
Scraper Agent
    ↓
Clean Event Dataset
    ↓
NCF Recommendation Model
    ↓
Ranked Events
    ↓
AI Personal Planner
    ↓
Final User Itinerary
```

---

## Technologies Used

### Backend

* Python
* Requests / BeautifulSoup (scraping)
* PyTorch Lightning (NCF model)

### AI / ML

* Neural Collaborative Filtering
* Embeddings
* Sigmoid ranking model
* Negative sampling

### Data Processing

* Pandas
* JSON pipelines
* Deduplication + normalization

---

## Key Concepts Implemented

* Implicit feedback learning
* Embedding-based representation learning
* Neural ranking systems
* Event data normalization
* Multi-factor scoring systems
* Personalized recommendation pipelines

---

## Why This System Works

Traditional event platforms:

* Show static lists
* Ignore user preferences
* Require manual filtering

OPERA:

* Learns user behavior
* Predicts interest before interaction
* Builds personalized itineraries automatically
* Continuously improves via embeddings

---

## Future Improvements

* Transformer-based recommendation model
* Real-time streaming user behavior
* Multi-modal embeddings (text + image + clicks)
* LLM-based itinerary reasoning agent
* Reinforcement learning for ranking optimization

---

## Summary

OPERA is a hybrid system combining:

* Web-scale event data collection
* Deep learning-based recommendation (NCF)
* AI-driven decision making

Its goal is to turn fragmented event data into **personalized real-world experiences**.
