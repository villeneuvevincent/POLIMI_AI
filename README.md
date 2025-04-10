# Biomedical AI Projects

## 🌐 Overview
This repository presents two AI-driven biomedical projects developed during my exchange semester in POLIMI:

1. **Automatic Segmentation of Vocal Tract Articulators using dsMRI and IMU-Net**
2. **Lung Cancer CT Scan Classification with Confidence Estimation**

Each project focuses on solving a unique challenge in medical imaging through deep learning, emphasizing robustness, interpretability, and clinical relevance.

---

## 🔜 Project 1: Dynamic MRI Vocal Tract Segmentation

### Objective
To segment vocal tract articulators from dynamic speech MRI (dsMRI) contaminated by Salt & Pepper noise using the **IMU-Net** architecture.

### Highlights
- **Architecture:** Modified U-Net with residual connections, dilated convolutions, and dropout for robustness
- **Preprocessing:** Normalization, 3x3 Median filtering
- **Custom Loss:** Dice + Focal loss for precise segmentation across 7 binary masks (e.g., tongue, soft palate)
- **Validation:** k-Fold cross-validation on 820 frames
- **Evaluation Metric:** Mean DICE Score

### Applications
- Diagnosis of speech disorders (apraxia, dysarthria)
- Functional articulation analysis
- Therapy monitoring, surgical planning, prosthetic design
- Research on sleep/breathing disorders

### Real-World Testing
- Tested on dsMRI of a control subject saying "microscopic"
- Promising results; not yet validated on pathological cases

---

## 🌐 Project 2: Lung CT Scan Classification

### Objective
To classify lung nodules from CT scans as **benign** or **malignant** with confidence estimation using deep learning models.

### Highlights
- **Models:** Baseline CNNs, VGG19, MobileNetV2, EfficientNetB0, ResNet50
- **Augmentation:** Applied before/after split; class weights used for balancing
- **Explainability:** Grad-CAM applied to MobileNetV2
- **Performance:** Best results with VGG19 on zoomed slices (Accuracy: 89%, F1: 0.89)

### Key Takeaways
- Zoomed slices outperformed full slices due to focused information
- Model generalization highly dependent on data imbalance strategies
- Grad-CAM shows potential for better diagnostic trust

### Note
**NB:** Les résultats ont été obtenus sur un jeu de données fourni par notre professeur, dont le contenu nous était inconnu au départ.

---

## 🔬 Technologies Used
- **Frameworks:** TensorFlow, Keras
- **Techniques:** Transfer learning, data augmentation, k-fold CV, Grad-CAM, custom loss functions
- **Evaluation:** Accuracy, Precision, Recall, F1, Mean DICE, class-specific metrics

---

