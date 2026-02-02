# Sensity AI: Enterprise-Grade Visual Intelligence

## 1. Executive Summary
Sensity AI (formerly Deeptrace) is a leading commercial platform specializing in deepfake detection and identity verification. For researchers, it represents the "State of the Art" (SOTA) in industrial application, serving as a critical baseline against which academic models are compared. It focuses heavily on **Liveness Detection**—determining if a face is present in real-time or if it is a spoof/injection.

---

## 2. Core Technologies

### 2.1 Passive Liveness Detection
Unlike "Active" liveness (which asks a user to smile or turn their head), **Passive Liveness** analyzes a static image or video frame without user interaction.
-   **Texture Analysis**: Detects the "smoothness" introduced by GANs or Autoencoders. Real skin has complex micro-texture; fakes often look "waxy".
-   **Environment Consistency**: Checks if the lighting on the face matches the lighting of the background.
-   **Double Encoding**: Detects artifacts left by re-encoding a video multiple times (a common trait of fakes).

### 2.2 Deepfake Threat Intelligence
Sensity does not just detect; it tracks.
-   **Botnet Detection**: Identifying networks of deepfake profiles on social media.
-   **Weaponization Tracking**: Monitoring "sextortion" campaigns or political misinformation using specific deepfake templates.

---

## 3. Integration & Usage (API)

Sensity operates primarily via a REST API. While the source is closed, the workflow for a researcher integrating it into a test harness is as follows:

### 3.1 Authentication
Obtain an API Key from the Sensity Dashboard.

### 3.2 Detection Request Structure
```json
// POST /api/v1/detect/image
{
  "image_base64": "data:image/jpeg;base64,/9j/4AAQSk...",
  "sensitivity_level": "strict"
}
```

### 3.3 Interpreting the Response
A typical response includes breakdown scores:
```json
{
  "is_fake": true,
  "confidence": 0.98,
  "artifacts": {
    "blending": 0.85,  // Boundary artifacts
    "texture": 0.92,   // Skin smoothness
    "lighting": 0.45   // Inconsistent shadows
  }
}
```
*Research Note: When writing a paper, you graph these confidence scores against your own model's scores to show correlation.*

---

## 4. Research Utility

### 4.1 The "Black Box" Benchmark
Since you cannot see Sensity's code, you treat it as an Oracle.
-   **Experiment**: Feed the same 1,000 deepfakes to your model and to Sensity.
-   **Goal**: If your model achieves 95% accuracy and Sensity achieves 96%, you are close to commercial grade. If Sensity gets 99% and you get 60%, your model is failing on features Sensity is catching (likely compression artifacts).

### 4.2 Liveness Datasets
Sensity often releases whitepapers on specific vulnerabilities (e.g., "Virtual Camera Injection"). Researchers can replicate these attack vectors to test their own defenses.
