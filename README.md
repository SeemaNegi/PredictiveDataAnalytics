# Replication Study: Predicting the Helpfulness of Online Customer Reviews (Video Games)

This repository contains a replication of a study on predicting the helpfulness of online customer reviews using machine learning and topic modeling, based on the Amazon Video Games review dataset.

The work was completed as part of a replication study assignment, with the goal of reproducing and understanding the results reported in the original research.

---

## Original Study Being Replicated

This project replicates the illustrative study presented in:

**Müller, O., Junglas, I., vom Brocke, J., & Debortoli, S. (2016).**  
*Utilizing big data analytics for information systems research: Challenges, promises, and guidelines.*  
European Journal of Information Systems, 25(4), 289–302.

In the original study, the authors demonstrate how textual content from online reviews, combined with machine learning techniques, can be used to predict whether a review is perceived as helpful by other users.

This repository focuses on the **Video Games** review category and follows the same general modeling approach described in the paper.

---

## Project Overview

The objective of this project is to predict whether an online customer review is **helpful** or **not helpful** using:

- Numeric features (review length and star rating)
- Textual features extracted using **Latent Dirichlet Allocation (LDA)**
- A **Random Forest classifier**

The performance of the topic‑based model is compared against a **baseline model** that does not use textual features.

---

## Dataset

- Source: UCSD / Amazon Reviews Dataset
- Category used: **Video Games**
- Format: **JSON‑Lines** (one JSON object per line)

Each review includes:
- Review text and summary
- Star rating
- Helpfulness votes (helpful votes, total votes)

During data loading:
- **231,780 reviews** were read
- **1 malformed JSON line** was skipped to ensure robust ingestion

Reviews with fewer than **2 total helpfulness votes** were removed to improve label reliability.  
After filtering, **103,572 reviews** were used for modeling.

---

## Label Definition

Review helpfulness is defined as:
helpfulness ratio = helpful votes / total votes

- A review is labeled **helpful (1)** if the ratio is **greater than 0.5**
- Otherwise, it is labeled **not helpful (0)**

This labeling approach follows the method described in the original study.

---

## Methodology

### Text preprocessing
- Lowercasing and basic text cleaning
- Removal of standard English stopwords
- Removal of domain‑specific stopwords:
  - **"game"**
  - **"play"**

These words occur very frequently in video game reviews and provide little information for distinguishing topics.  
The original paper does not explicitly list the stopwords used, so this represents a reasonable preprocessing choice.

---

### Feature extraction
- Review length (number of words)
- Star rating
- Topic probabilities from **LDA (100 topics)**

---

### Models
- **Main model:** Random Forest classifier (128 trees)
- **Baseline model:** Logistic Regression using only review length and star rating

---

## Results

Train–test split:
- Train: **82,857 reviews**
- Test: **20,715 reviews**

Performance (ROC AUC):

| Model | AUC |
|------|-----|
| Baseline (length + rating) | 0.7159 |
| Random Forest + LDA topics | 0.7527 |
| Improvement | **+0.0368** |

Adding topic‑based textual features clearly improves predictive performance over numeric features alone.

---

## ROC Curve

The repository includes a ROC curve visualization for the Random Forest model.

- The dashed diagonal line represents a **random classifier** (AUC = 0.5)
- The model’s ROC curve lies well above this line
- This visually confirms the reported AUC value (0.7527)

---

## Comparison with the Original Study

The original study reports a ROC AUC of approximately **0.73** for predicting review helpfulness using Random Forests with topic‑based features.

In this replication, the model achieved a slightly higher AUC (**0.7527**), despite using a smaller subset of the data. Possible reasons include:
- Differences in preprocessing choices
- Differences in topic modeling implementation
- Reduced noise due to filtering and sampling

Overall, the replication confirms the main conclusion of the original study:  
**textual content provides meaningful additional information for predicting the helpfulness of online customer reviews.**

---
