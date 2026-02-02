# Deepfake Model Architectures Reference

This document provides a technical deep-dive into the specific neural network architectures (models) available within the major deepfake frameworks (`DeepFaceLab` and `Faceswap`) and the standard backbones used for detection.

---

## 1. DeepFaceLab (DFL) Models

DeepFaceLab focuses on a single, highly mutable architecture called **SAEHD**, but also offers a legacy lightweight model.

### 1.1 SAEHD (Sparse AutoEncoder High Definition)
This is the industry standard. It is not just one model but a configurable framework.
-   **Architecture Type**: Autoencoder with shared Encoder and separate Decoders.
-   **Core Architectures**:
    -   **DF (DeepFake)**: The standard "Identity Swap".
        -   *Theory*: The Source and Dest decoders are completely separate.
        -   *Pros*: Better lighting matching, more stable.
        -   *Cons*: Might look less like the source ID (less "likeness").
    -   **LIAE (Learned Identity AutoEncoder)**:
        -   *Theory*: The Encoder outputs a code that is slightly interconnected with the decoder.
        -   *Pros*: Superior **Likeness** (looks more like the source actor) and better morphing.
        -   *Cons*: Can struggle with extreme lighting changes.
-   **Key Parameters**:
    -   **Resolution**: 128px to 640px. (Standard HD is 224px or 256px).
    -   **AE Dims**: The customized depth of the latent vector. Higher = more detail, but harder to train (overfitting risk).
    -   **Ed/De Dims**: Capacity of the Encoder/Decoder layers.

### 1.2 Quick96
-   **Architecture**: Fixed low-resolution, shallow Autoencoder.
-   **Use Case**: Testing if the pipeline works, or running on extremely weak hardware (e.g., 2GB VRAM).
-   **Status**: Obsolete for quality research, but useful for quick logical verification.

### 1.3 AMP (Amplified / Mixed Precision)
-   **Description**: A variant of SAEHD optimized for NVIDIA RTX cards uses FP16 (half-precision) math to double training speed and reduce VRAM usage with minimal quality loss.

---

## 2. Faceswap Models

Faceswap uses a "plugin" system, allowing distinct model files. These are the most relevant for research:

### 2.1 Original
-   **Based on**: The original Reddit/fakeapp script.
-   **Architecture**: Simple shared encoder, dual decoder.
-   **VRAM Usage**: Very Low (<4GB).
-   **Usage**: The baseline. "If it doesn't work here, your data is bad."

### 2.2 Villain
-   **Description**: A higher-capacity model designed to capture fine details (wrinkles, pores).
-   **Theory**: Adds additional convolutional layers and residual constraints.
-   **Pros**: Much sharper than `Original`.
-   **Cons**: Slower training, risk of "color flickering" if not trained long enough.

### 2.3 Dfaker
-   **Based on**: The `dfaker` repo implementation.
-   **Result**: Known for handling **side profiles** (90-degree turns) better than others due to its specific loss masking.

### 2.4 RealFace
-   **Focus**: Photorealism over exact likeness.
-   **Theory**: Uses a "Perceptual Loss" heavily weighted towards texture rather than just structural shape.
-   **Outcome**: The face looks very "real" (skin-like), even if it doesn't 100% match the source identity perfectly.

### 2.5 Unbalanced
-   **Architecture**: Asymmetric Encoder/Decoder.
-   **Use Case**: When your "Source" is 4K quality but your "Destination" is 480p (or vice versa). You can allocate more neural power to the complex side.

---

## 3. Detection Backbones (Classifiers)

When building a detection tool (The "Blue Team"), you don't use DFL models. You use **Classifiers**. These are the standard models you will find in literature (like FaceForensics++).

### 3.1 XceptionNet (The Gold Standard)
-   **Type**: CNN (Convolutional Neural Network).
-   **Why use it**: It uses "Depthwise Separable Convolutions," making it efficient yet powerful at spotting pixel-level anomalies.
-   **Research Status**: It is the baseline. Your new method *must* beat XceptionNet to be published.

### 3.2 MesoNet (Meso4 / MesoInception4)
-   **Type**: Shallow Custom CNN.
-   **Theory**: Deepfakes leave artifacts at the "mesoscopic" level (loss of texture compression). MesoNet focuses specifically on these mid-level features rather than semantic face features.
-   **Status**: Older, but highly cited.

### 3.3 EfficientNet (B0 - B7)
-   **Type**: Scalable CNN.
-   **Why use it**: Currently achieves State-of-the-Art (SOTA) results on many leaderboards due to its balance of width, depth, and resolution.

### 3.4 Vision Transformers (ViT)
-   **Type**: Transformer (Attention-based).
-   **Theory**: Instead of looking at pixels, it looks at patches of the image and their relationships. Excellent for detecting "Global Inconsistencies" (e.g., ear doesn't match the nose) which CNNs might miss.
