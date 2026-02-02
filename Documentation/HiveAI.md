# Hive AI (Hive Moderation): Scalable Detection

## 1. Executive Summary
Hive AI provides one of the most robust and scalable APIs for "Generative AI" detection. While standard deepfake tools focus on faces, Hive is distinct in its ability to detect **Diffusion Models** (Midjourney, DALL-E, Stable Diffusion) as well as **Face Swaps**. For researchers cleaning large datasets (e.g., LAION-5B), Hive is often used to filter out synthetic noise.

---

## 2. Classification Taxonomy

Hive doesn't just say "Fake"; it categorizes the *source* of the manipulation.

1.  **AI-Generated (Text-to-Image)**:
    -   *Triggers*: Midjourney, DALL-E 3, Stable Diffusion.
    -   *Features*: Unnatural fingers, illogical text, oversaturated "fantasy" lighting, pixel-perfect smoothness.
2.  **Face Swap (Deepfake)**:
    -   *Triggers*: DeepFaceLab, ReFace.
    -   *Features*: Blurring around the hairline, mismatched ear lighting.
3.  **Face Edit (Filter)**:
    -   *Triggers*: Facetune, Snapchat filters.
    -   *Features*: Warped geometry (slimmed nose, enlarged eyes) without texture replacement.

---

## 3. Research & Integration

### 3.1 Dataset Cleaning
One of the primary uses of Hive in research is **Dataset Purification**.
-   *Problem*: You download 10,000 "real" faces from the web for training.
-   *Risk*: 500 of them might be AI-generated (StyleGAN faces). If you train on these, your model learns wrong features.
-   *Solution*: Run the dataset through Hive. Discard any image with `score > 0.9` for `class: midjourney`.

### 3.2 API Integration
```python
import requests

url = "https://api.thehive.ai/api/v2/task/sync"
headers = {"Authorization": "token YOUR_API_KEY"}
data = {
    "url": "https://example.com/suspicious_image.jpg",
    "classes": ["general_ai_generated"]
}

response = requests.post(url, headers=headers, json=data)
# Output:
# "classes": [
#   {"class": "yes_ai_generated", "score": 0.99},
#   {"class": "no_ai_generated", "score": 0.01}
# ]
```

---

## 4. Performance Characteristics

-   **Latency**: Hive is designed for real-time (sub-500ms).
-   **False Positives**: Researchers have noted Hive can struggle with highly processed *digital art* (paintings drawn in Photoshop), flagging them as AI due to smooth textures.
-   **Adversarial Robustness**: Like all detectors, adding slight Gaussian noise or resizing the image can sometimes drop the confidence score, which is a key area of study in "Adversarial Attacks on Deepfake Detectors".
