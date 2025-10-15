# 🧠 Customer Behavior Analytics Platform  
**Predicting 30-Day Churn & Personalized Travel Recommendations**

---

## 📘 Overview  
This repository contains the complete implementation of a **Customer Behavior Analytics System** for a travel-tech platform aiming to:  

1. **Predict user churn within 30 days** using behavioral, temporal, and contextual data.  
2. **Generate top-N personalized travel recommendations** (destinations or hotels) using review content.  
3. **Explain churn drivers and recommendation logic** with SHAP-based interpretability.  
4. **Quantify the ROI of targeted retention offers** for high-risk segments.  

The system integrates churn prediction and recommendation into a **unified ML pipeline**, enabling both **product** and **growth** teams to make data-driven retention and personalization decisions.  

---

## 🧩 Problem Statement  
Travel platforms experience **high drop-offs during trip planning** and inconsistent repeat bookings. The objective was to design a scalable ML framework that can:  

- **Predict 30-day churn** probability for each active user.  
- **Recommend top-5 destinations or hotels** per user with clear “why this” explanations.  
- **Classify customers into engagement tiers** (High, Medium, Low).  
- **Surface interpretable insights** for product strategy, pricing, and marketing.  

---

## 📊 Data Sources  

| Dataset | Description | Purpose |
|----------|--------------|----------|
| **Hotel Booking Demand** | ~120K tabular booking records with fields like lead time, ADR, special requests, deposit type | Core dataset for churn modeling |
| **TripAdvisor Hotel Reviews** | Text dataset of hotel reviews and ratings | Feature enrichment and content-based recommendations |

---

## 🔍 Exploratory Data Analysis (EDA)  
**Objectives:** Profile users, sessions, and bookings to quantify funnel performance and churn risk.  

- Missing values handled (children → 0, undefined country → “Unknown”).  
- Outlier filtering for ADR and guest count.  
- **Class Imbalance:** Churners ≈ 30%, Retained ≈ 70%.  
- **Top Correlated Churn Drivers:** Lead time, previous cancellations, length of stay.  
- Seasonal patterns visualized via monthly cancellation rates and booking lead times.  

---

## 🧠 Feature Engineering  

**Core Features:**
- **RFM Metrics:** Recency, Frequency, Monetary (derived per user).  
- **Temporal Signals:** Arrival month, day of week, booking window buckets.  
- **Behavioral Metrics:** Engagement score (search depth + wishlist + clicks).  
- **Context Features:** Domestic vs international flag, deposit type, ADR tier.  
- **Advanced Derived Features:**
  - Booking stability = special_requests / previous_cancellations  
  - Room mismatch = assigned vs reserved room  
  - Fourier-encoded seasonality for month cycles  

All features are standardized and stored in `engineered_features_v4.csv`.

---

## 🧮 Modeling Approach  

### 1. **Churn Prediction**
| Model | Type | ROC-AUC | PR-AUC | F1 |
|--------|------|----------|---------|----|
| Logistic Regression | Interpretable baseline | 0.882 | 0.837 | 0.79 |
| Random Forest | Ensemble baseline | 0.926 | 0.898 | 0.82 |
| **XGBoost** | Tuned ensemble (final) | **0.955** | **0.935** | **0.84** |

**Validation:** Temporal split + 5-fold stratified CV  
**Calibration:** Isotonic calibration for churn probabilities  

### 2. **Recommendation System**
- **Type:** Content-based recommender using TF-IDF embeddings of TripAdvisor reviews  
- **Similarity Metric:** Cosine similarity between hotel text vectors  
- **Auxiliary Model:** Calibrated Linear SVM for sentiment scoring and embedding extraction  
- **Output:** Top-N (default N=5) hotel or destination recommendations per user with “why this” rationale based on keyword overlap.  

---

## 🔎 Explainability & Insights  

### SHAP Interpretability
- **Global importance:** Lead time, previous cancellations, special requests.  
- **Local explanations:** Per-user waterfall plots illustrating churn contribution by feature.  
- **Segment-level insights:** Clustered SHAP profiles to derive behavioral personas.

### User Personas
| Persona | Description | Churn Risk | Key Traits |
|----------|--------------|-------------|-------------|
| **A – Loyal Frequent Travelers** | Repeat customers, low variance in lead time | 10% | Business focus, stable behavior |
| **B – Occasional Planners** | Moderate frequency, predictable booking cycles | 30% | Domestic, flexible plans |
| **C – Price-Sensitive Explorers** | New or budget users, long lead time | 75% | High cancellations, deal seekers |

---

## 💰 ROI Simulation  

Modeled impact of retention offers using predicted churn probabilities.  

| Scenario | Discount | Retention Uplift | Net ROI |
|-----------|-----------|------------------|----------|
| Base | 0% | 0% | ₹0 |
| Moderate Offer | 5% | 20% | **+₹46M** |
| Aggressive Offer | 10% | 10% | +₹31M |
| Heavy Offer | 20% | 5% | −₹12M |

Positive ROI observed for targeted offers (≤10%) focused on high-risk personas (Persona C).

---


## ⚙️ Technical Specifications  

| Component | Details |
|------------|----------|
| **Language** | Python 3.10 |
| **Key Libraries** | pandas, numpy, scikit-learn, xgboost, shap, matplotlib, nltk, streamlit |
| **Validation Strategy** | Time-based split with stratified cross-validation |
| **Evaluation Metrics** | ROC-AUC, PR-AUC, F1 (churn); Cosine similarity inspection (recommendations) |
| **Interpretability** | SHAP values for global + local feature importance |

---

## 📈 Business Insights  

- **Lead Time** and **Booking Stability** are actionable churn levers.  
- **Price sensitivity** and **cancellation history** are top churn indicators.  
- **Moderate retention incentives (5–10%)** yield optimal ROI.  
- **Content-based recommendations** enhance re-engagement for churn-prone users.  

---

## 🚀 Next Steps  

1. Extend to **hybrid recommendation system** (content + collaborative).  
2. Deploy scoring API via **FastAPI** or **Streamlit**.  
3. Integrate **drift monitoring** and retraining triggers.  
4. Develop **personalized campaign dashboard** for marketing teams.  


## 🧩 Deliverables  

| Deliverable | Description |
|--------------|-------------|
| `Analysis Notebooks` | End-to-end EDA, modeling, interpretability, and ROI analysis |
| `Technical Report (PDF)` | 10-page document summarizing full pipeline |
| `Stakeholder Deck (PPTX)` | Non-technical summary for business teams |
| `README.md` | Documentation for setup and results |

---


## 📚 References  

- Hotel Booking Demand Dataset – Antonio et al., 2019  
- TripAdvisor Hotel Reviews Dataset – Kaggle  
- Lundberg & Lee (2017). *A Unified Approach to Interpreting Model Predictions (SHAP).*  
- [XGBoost Documentation](https://xgboost.readthedocs.io/)  

---

## 🧾 License  
MIT License © 2025 — Customer Behavior Analytics Platform  
