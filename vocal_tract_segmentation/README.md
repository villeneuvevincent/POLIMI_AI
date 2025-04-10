# Automatic Segmentation of Vocal Tract Articulators in Noisy dsMRI using IMU-Net

## 🧠 Clinical Context

Speech articulation is one of the most intricate motor functions, requiring precise coordination of numerous muscles governed by a network of cortical and subcortical brain structures. Disruptions in this system, known as **motor speech impairments**, often emerge as early indicators of neurodegenerative diseases such as *non-fluent/agrammatic primary progressive aphasia (nfvPPA)*.

These impairments manifest mainly in two forms:

- **Apraxia of Speech (AOS):** Characterized by inconsistent speech errors, distorted phonemes, and irregular prosody.
- **Dysarthria:** Exhibits more systematic patterns, but with slowed, imprecise, or weak muscle activity that affects clarity and voice quality.

Current diagnostic approaches rely heavily on subjective clinical evaluation. However, **dynamic speech MRI (dsMRI)** enables non-invasive, real-time visualization of the vocal tract during speech production. With advancements in high temporal resolution MRI, there is an opportunity to shift toward **objective, quantitative** tools for diagnosing and monitoring these conditions.

Our project aims to develop a robust system for **automatic segmentation** of vocal tract articulators in dsMRI images contaminated by *Salt & Pepper noise*, using the **IMU-Net** neural architecture.

---

## 🧪 Our Model

### ♻️ Preprocessing

- **Normalization:** Image intensities were rescaled from `[0, 255]` to `[0, 1]` to avoid saturation effects in activation functions.
- **Noise Filtering:** We applied a **3x3 median filter**, chosen after testing various kernel sizes, as it provides the best balance between noise reduction and edge preservation. Median filters are particularly effective against Salt & Pepper noise due to their robustness to outliers.

---

### 🧠 Architecture: IMU-Net

Our segmentation model is based on an enhanced **U-Net** architecture, designed for robustness under noisy conditions.

#### 🧹 Key Features:

- **Residual Connections:** Improve gradient flow and feature propagation.
- **Batch Normalization:** Accelerates convergence and stabilizes training.
- **Dilated Convolutions:** Increase receptive field without added parameters, ideal for contextual understanding.
- **Dropout Layers:** Reduce overfitting and improve generalization.

#### 🔍 Module Overview:

1. **Input Block**
   - Conv → BatchNorm → ReLU
   - Residual connection to next layer

2. **Encoder (x4 Blocks)**
   - Each block: BN → ReLU → Conv (x2) + Residual
   - MaxPooling for downsampling
   - Dropout for regularization

3. **Bottleneck**
   - Dilated convolutions to capture global spatial context

4. **Decoder (x4 Blocks)**
   - Mirrors the encoder
   - BN → ReLU → Conv (x2) + Residual
   - Skip connections from encoder
   - Dropout

5. **Output Block**
   - Final Conv → Softmax for pixel-wise multi-class segmentation

---

## 📊 Training and Evaluation

### 📁 Dataset

- **Frames:** 820 grayscale dsMRI images with Salt & Pepper noise
- **Classes:** 7 binary masks — background, head, tongue, soft palate, hard palate, upper lip, lower lip
- **Split:** 80% training, 10% validation, 10% testing

### 🧼 Loss & Metrics

- **Custom Loss Function:** `DiceFocalLoss` = Dice Loss + Focal Loss (α=0.5, β=0.5)
- **Evaluation Metric:** *Mean DICE Score* — optimized to measure overlap accuracy

### ⚙️ Training Strategy

- **K-Fold Cross-Validation (k=5):** Ensures generalization across the full dataset
- **Epochs:** Up to 70 per fold
- **Batch Size:** 8
- **Optimizer:** Adam (learning rate = 1e-3)
- **Callbacks:**
  - *ModelCheckpoint:* Save best model based on val_loss
  - *EarlyStopping:* Patience = 5, with best weight restoration

*Note:* We chose not to apply data augmentation due to limited computational resources and the desire to preserve anatomical integrity.

---

## 🌟 Prediction & Post-Processing

- **Per-Class Evaluation:** Focus on tongue and soft palate due to their clinical relevance in AOS and dysarthria
- **Key Metric:** *Recall* — to minimize false negatives
- **Post-Processing:**
  - One-hot encoding from Softmax output
  - 7 binary masks created for clarity and compatibility with ground truth

---

## 🎥 Real-World Testing

We tested the model on a dsMRI video of a control subject articulating the word **"microscopic"** three times. Segmentation masks were overlaid on the video in real time.

> 🔬 The model hasn't yet been tested on pathological speech data, but early results suggest **high accuracy and clinical potential**.

---

## 🌍 Potential Applications

This segmentation system opens up a wide range of possibilities in clinical and research contexts:

- **Diagnosis Support:** Differentiate between AOS and dysarthria via error patterns in articulator motion
- **Therapy Monitoring:** Track improvements in articulation during rehabilitation
- **Surgical Planning & Prosthetics:** Customize devices based on precise anatomy (e.g., palatal prostheses)
- **Speech Synthesis:** Enhance synthetic speech in assistive communication systems
- **Breathing & Sleep Studies:** Analyze articulator impact on airflow, potentially guiding interventions

---

## 📌 Conclusion

Our work presents a promising step toward automating the analysis of vocal tract dynamics in noisy MRI data. The IMU-Net architecture demonstrates robustness, precision, and adap

📹 **See below the video presentation of our project**
<div align="center">
  <a href="https://www.youtube.com/embed/Nb_6BuZ3Brc">
    <img src="https://img.youtube.com/vi/Nb_6BuZ3Brc/hqdefault.jpg" alt="Watch the video" />
  </a>
</div>
