# FaceForensics++: The Deepfake Detection Benchmark

## 1. Executive Summary
FaceForensics++ (FF++) is the premier dataset and benchmark for training and evaluating deepfake detection algorithms. Created by the Technical University of Munich (TUM), it provides over 1.8 million manipulated frames. Any serious research project involving detection *must* benchmark against this dataset to be considered valid by the academic community.

---

## 2. Dataset Structure & Manipulations

The dataset consists of 1,000 original video sequences (from YouTube) manipulated by four distinct methods.

### 2.1 The Four Manipulation Methods
1.  **Deepfakes (DF)**:
    -   *Type*: Identity Swap.
    -   *Method*: Autoencoder-based (similar to early DeepFaceLab/Faceswap).
    -   *Artifacts*: Blurring, temporal jitter, mismatched eye colors.
2.  **Face2Face (F2F)**:
    -   *Type*: Expression Transfer (Reenactment).
    -   *Method*: 3D Morphable Model (3DMM). The source actor controls the expressions (mouth, eyes) of the target person without changing their identity.
    -   *Artifacts*: Mouth interior artifacts, unnatural blinking patterns.
3.  **FaceSwap (FS)**:
    -   *Type*: Identity Swap.
    -   *Method*: Graphics-based (Non-AI). Uses 3D generic face fitting and texture blending.
    -   *Artifacts*: Sharp boundaries, lack of skin detail, poor lighting matching.
4.  **NeuralTextures (NT)**:
    -   *Type*: Expression Transfer.
    -   *Method*: GAN-based. Uses a neural renderer to modify the texture of the 3D mouth region.
    -   *Artifacts*: Very subtle, mostly high-frequency noise in the mouth region.

### 2.2 Compression Levels (Crucial)
Real-world deepfakes are rarely raw; they are compressed by social media platforms. FF++ provides data in three qualities:
-   **Raw (C0)**: Lossless video options. Easiest to detect.
-   **High Quality (C23)**: H.264 compression with Constant Rate Factor (CRF) 23. Represent good upload quality.
-   **Low Quality (C40)**: H.264 with CRF 40. Highly compressed (blocky). Very difficult to detect. *Research usually focuses here.*

---

## 3. Implementation Steps

### 3.1 Gaining Access
1.  Go to the [GitHub Repository](https://github.com/ondyari/FaceForensics).
2.  Fill out the Google Form request to get the download script.
3.  You will receive a python script `download_faceforensicspp.py`.

### 3.2 Downloading Data
Use the script to download specific subsets. Be warned: the full dataset is >2TB.
```bash
# Example: Download Deepfakes (DF) method in compressed quality (c23)
python download_faceforensicspp.py ./dataset --dataset FaceForensics++ --type deepfakes --compression c23
```

### 3.3 The Standard Split
To ensure fair comparison, you **must** use the official train/test/val split provided in the repo (`train.json`, `test.json`, `val.json`).
-   **Train**: 720 videos.
-   **Validation**: 140 videos.
-   **Test**: 140 videos.
*Do not mix these! If a person appears in Train, they must not appear in Test.*

### 3.4 Benchmarking (XceptionNet)
The baseline model provided is **XceptionNet**, a CNN pretrained on ImageNet.
1.  **Preprocessing**: Use the provided `face_extraction_cuda` tool to crop faces from the videos.
2.  **Training**: Run the training script pointing to your `c23` or `c40` folders.
3.  **Evaluation**:
    -   **ACC (Accuracy)**: Percentage of correct classifications (Real vs Fake).
    -   **AUC (Area Under Curve)**: Measures reliability across different thresholds. Current SOTA models achieve >90% AUC on C23 but drop significantly on C40.

---

## 4. Research Analysis Tips
-   **Generalization**: Train on "Deepfakes" and test on "Face2Face". Most models fail this (Cross-Manipulation Check). This is a hot research topic.
-   **Frequency Analysis**: Apply Discrete Cosine Transform (DCT) to the images. Deepfakes often hallucinate high-frequency details incorrectly compared to real cameras.
