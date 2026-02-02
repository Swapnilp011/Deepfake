# Faceswap: Modular Deepfake Research Framework

## 1. Executive Overview
Faceswap is a highly modular, open-source deepfake framework written in Python. Unlike DeepFaceLab, which is a singular optimized pipeline, Faceswap offers a user-friendly Graphical User Interface (GUI) and a "plugin" architecture. This makes it ideal for educational research and comparing different model architectures (GANs, Autoencoders) side-by-side.

---

## 2. Theoretical Architecture

### 2.1 The Plugin System
Faceswap treats every step as a swappable module:
- **Aligners**: Algorithms to find face landmarks (FAN, Dlib).
- **Detectors**: Algorithms to find faces (S3FD, MTCNN, CV2).
- **Models**: The Neural Network architecture (Original, Villain, RealFace).
- **Trainers**: Optimization strategies (Adam, RMSProp).

### 2.2 Model Types (Common)
Faceswap allows you to switch between different neural network structures easily:
- **Original**: The classic autoencoder (similar to the original "Preview" scripts). Low VRAM usage, basic quality.
- **Villain**: A more complex model offering higher detail but requiring more VRAM. It adds intermediate layers to capture texture.
- **Dfaker**: Modeled after the DFaker implementation, known for handling side profiles slightly better.
- **RealFace**: Focuses on photorealism using complex loss functions (perceptual, gradients).
- **Unbalanced**: Uses different sizes for Encoder and Decoder, useful when source data is of different quality than destination.

### 2.3 Warping
Faceswap relies heavily on "warping" the input face before feeding it to the encoder. This data augmentation helps the model generalize better, effectively "showing" the model the face from slightly different angles/scales during training to make it robust.

---

## 3. Installation & Environment

### Prerequisites
- **OS**: Windows, Linux, macOS.
- **Python**: 3.10 (Recommended).
- **Git**: For cloning the repository.
- **Conda**: Anaconda/Miniconda is highly recommended to manage environments.

### Setup Steps
1.  **Clone Repository**: `git clone https://github.com/deepfakes/faceswap.git`
2.  **Installer Script**: Run the `setup.py` (Linux/Mac) or the Windows Installer provided on their site.
3.  **Drivers**: The installer handles CUDA setup if NVIDIA is selected. It also supports AMD (ROCm) and CPU (slow).
4.  **Launch**: `python faceswap.py gui`

---

## 4. GUI Workflow Implementation

The Faceswap GUI is divided into three main tabs: **Extract**, **Train**, and **Convert**.

### Phase 1: Extraction (Tab: Extract)
**Objective**: gather training data.
1.  **Input**: Select your `video_src.mp4` or image folder.
2.  **Output**: Create a folder `faces_src`.
3.  **Settings**:
    - **Detector**: `s3fd` (best accuracy) or `mtcnn` (fast).
    - **Aligner**: `fan` (Face Alignment Network).
4.  **Action**: Click "Extract". The tool will crop faces and save alignment data (`.fsa`) files.
5.  **Clean**: Go to `Tools > Sort` to sort faces by "Blur" or "Face" similarity and manually delete bad extractions.

### Phase 2: Training (Tab: Train)
**Objective**: Teach the neural network.
1.  **Input A**: Folder of aligned source faces.
2.  **Input B**: Folder of aligned destination faces.
3.  **Model**: Select `Villain` (balanced) or `Original` (fast learning).
4.  **Settings**:
    - **Batch Size**: 16-64 (depending on GPU).
    - **Validations**: 20 (helps monitor generalization).
5.  **Graph**: The GUI shows a live loss graph.
    - **Loss A / Loss B**: Should both decrease and converge.
    - **Preview**: Updates every few iterations.
6.  **Stop**: When the detailed preview looks crisp and loss flattens.

### Phase 3: Conversion (Tab: Convert)
**Objective**: Swap the faces.
1.  **Input**: Original `video_dst.mp4`.
2.  **Model Directory**: Path where you saved the trained model.
3.  **Alignments**: The `.fsa` file generated during extraction (crucial for stability).
4.  **Writer**: `ffmpeg` (for video output).
5.  **Color Adjustment**:
    - **Match-hist**: Histogram matching.
    - **Seamless-clone**: Using standard CV2 seamless cloning (heavy but good blending).
6.  **Action**: Click "Convert".

---

## 5. Research Analysis

Faceswap is excellent for comparative studies:
- **A/B Testing**: You can train a "Villain" model and an "Original" model on the exact same dataset to compare SSIM scores.
- **Augmentation Study**: You can toggle Warp, Flip, and Color augmentation to see how it affects learning speed vs. generalization.
- **Hardware Benchmarking**: Since it supports CPU/AMD/NVIDIA, it is often used to benchmark deepfake generation efficiency across hardware.
