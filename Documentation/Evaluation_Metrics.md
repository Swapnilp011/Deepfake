# Evaluation Metrics for Deepfake Research

In a presentation, you cannot just say "It looks good." You need mathematical proof. This document explains the standard metrics used to grade Deepfake quality (Generation) and detection accuracy (Detection).

---

## 1. Generation Quality Metrics (How "Real" is it?)

### 1.1 SSIM (Structural Similarity Index Measure)
-   **Definition**: Measures the similarity between two images based on luminance, contrast, and structure.
-   **Scale**: -1 to +1 (where +1 is identical).
-   **Usage**: Compare the *generated* face to the *original target* face. A good deepfake usually scores **> 0.85**.
-   **Limitations**: It focuses on structure, so a blurry image might still score high.

### 1.2 PSNR (Peak Signal-to-Noise Ratio)
-   **Definition**: The ratio between the maximum possible power of a signal (image) and the power of corrupting noise.
-   **Scale**: Measured in decibels (dB). Higher is better.
-   **Benchmark**: > 30dB is considered high quality.

### 1.3 LPIPS (Learned Perceptual Image Patch Similarity)
-   **Definition**: Uses a neural network (VGG or AlexNet) to measure how similar images look *to a human eye*.
-   **Scale**: LOWER is better (0 = identical).
-   **Why use it**: It is better than SSIM at detecting "blurry" textures which look bad to humans but mathematically similar to computers.

### 1.4 FID (Fréchet Inception Distance)
-   **Definition**: Measures the distance between the distribution of real images and generated images.
-   **Usage**: The gold standard for GANs (Generative Adversarial Networks).
-   **Scale**: Lower is better.

---

## 2. Detection Performance Metrics (How good is the catcher?)

### 2.1 Accuracy (ACC)
-   **Formula**: (True Positives + True Negatives) / Total Samples.
-   **Problem**: If you have 99 real videos and 1 fake, a model that says "All Real" has 99% accuracy but is useless.

### 2.2 AUC-ROC (Area Under the Receiver Operating Characteristic Curve)
-   **Definition**: Evaluation metric for binary classification problems. It tells how much the model is capable of distinguishing between classes.
-   **Scale**: 0.5 (Random Guessing) to 1.0 (Perfect).
-   **Target**: A publishable research paper usually needs an AUC **> 0.90** on the FaceForensics++ (c23) dataset.

### 2.3 EER (Equal Error Rate)
-   **Definition**: The point where the False Acceptance Rate (FAR) equals the False Rejection Rate (FRR).
-   **Scale**: Lower is better.

### 2.4 Cross-Dataset Generalization
-   **Concept**: Train on Dataset A (FaceForensics), Test on Dataset B (Celeb-DF).
-   **Typical Result**: Accuracy usually drops by 20-30%. Showing this drop in your PPT demonstrates honesty and scientific rigor.
