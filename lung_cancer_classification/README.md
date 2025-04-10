# Lung Cancer CT Scan Classification with Confidence Estimation

## 🧠 Project Overview

This project tackles a **classification problem on lung cancer CT scans**, with an added focus on **prediction confidence estimation** — a critical component for clinical relevance. Our models aim to not only classify lung nodules as benign or malignant but also provide interpretable confidence in their predictions.

We tested several architectures (from basic CNNs to pre-trained transfer learning models) and evaluated various **data balancing**, **augmentation**, and **explainability strategies**.

---

## 🔎 Problem Analysis

- **Dataset:** 2,363 patients, each with CT scan slices and a malignancy score (1 to 5)
- **Labeling:**
  - **Binary classification:** Benign (1-3) vs. Malignant (4-5)
  - **Multi-class classification:** Scores from 1 to 5
- **Challenge:** Highly **imbalanced dataset**, with class 3 overrepresented and extremes (1 and 5) underrepresented
- **Visualization:** PCA of VGG19-extracted features revealed significant overlap among classes, confirming difficulty of separation

---

## 📊 Methodology

### 🌟 Data Preparation

- **Image Resizing:** All images resized to 224x224 (standard input size)
- **Normalization:** Min-max scaling to [0,1], done per image due to variable intensity distributions
- **Conversion to RGB:** Required for compatibility with pre-trained networks
- **Augmentation Techniques:**
  - Flips, Gaussian noise, zoom, brightness/contrast shifts
  - Two augmentation strategies:
    1. Applied **before split** (improves balance but causes potential overfitting)
    2. Applied **after split** (better generalization, closer to real-world scenario)
- **Class Weights:** Applied during loss computation using inverse frequency

### ⚖️ Dataset Splitting

- Stratified splitting into: 80% train, 10% validation, 10% test

---

### 🚀 Model Compilation

- **Loss Function:** `categorical_crossentropy`
- **Optimizer:** Adam, learning rate = 0.001

### ⌚ Training & Callbacks

- **ReduceLROnPlateau:** Adjusts LR on validation accuracy plateau
- **ModelCheckpoint:** Saves best weights
- **EarlyStopping:** Avoids overfitting by halting when val_accuracy stagnates

---

## 🔮 Experiments

### 🔄 Class Imbalance Handling

1. **Full dataset augmentation:** Effective but prone to overfitting
2. **Train-only augmentation:** Slightly lower results, but better generalization
3. **Class weights only:** Strong alternative for real-world imbalanced scenarios

### 🧶 Model Architectures

- **Basic CNNs:** Simple, quickly plateaued
- **Transfer Learning:** VGG19, MobileNetV2, EfficientNetB0, ResNet50
  - Fine-tuned with custom classification heads
- **Custom CNN (from literature):** Lighter model optimized for spatial features with dropout & softmax output

---

### 🔍 Explainability

- **Grad-CAM for MobileNetV2:** Visualized attention maps
- Early implementation showed weak localization on nodules, highlighting potential for improvement in future work

---

## 📊 Results Summary

**Binary Classification on Zoomed Slices**

- **Top model:** VGG19 with class weights and full dataset augmentation (Accuracy: 0.89, Precision:0.89, Recall: 0.88, F1: 0.89)

**Multi-class on Zoomed Slices**

- **Top model:** MobileNetV2 with class weights (Accuracy: 0.49, Precision:0.41, Recall: 0.43, F1: 0.40)

**Binary Classification on Full Slices**

- **Top model:** Custom CNN with class weights and full dataset augmentation (Accuracy: 0.68, Precision:0.65, Recall: 0.60, F1: 0.60)

**Multi-class on Full Slices**

- **Top model:** VGG19 with class weights and full dataset augmentation (Accuracy: 0.64, Precision:0.66, Recall: 0.64, F1: 0.64)

*NB: The results were obtained on a dataset provided by our teacher, the content of which was unknown to us at the outset.*
---

## 🤔 Discussion

- **Class imbalance** critically influenced model performance and generalization
- **Zoomed slices** yielded better results due to higher signal-to-noise ratio
- **Overfitting risk** higher with full-dataset augmentation, mitigated by class weighting
- **Explainability** (e.g., Grad-CAM) remains a crucial area for future exploration

---

## 🔝 Conclusion

This project demonstrates the feasibility of robust classification of lung cancer CT scans using transfer learning and proper data preprocessing. Further improvements can be made in:

- Designing **custom loss functions** for imbalanced data
- Improving **explainability pipelines**
- Validating on **external datasets** for generalization

---

## 📚 References

[1] Research on the Classification of Benign and Malignant Parotid Tumors Based on Transfer Learning and a Convolutional Neural Network
