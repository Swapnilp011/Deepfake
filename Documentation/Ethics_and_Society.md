# Ethical, Legal, and Social Implications of Deepfakes

This document covers the critical non-technical aspects of deepfake research. Every academic presentation on this topic **must** address these issues to be considered complete.

---

## 1. The Ethical Landscape

### 1.1 Non-Consensual Pornography (NCP)
-   **Statistic**: ~96% of deepfake videos online are non-consensual pornography involved women (Source: Sensity AI).
-   **Impact**: Psychological harm, reputation damage, harassment.
-   **Research Stance**: Researchers must adhere to strict codes of conduct (e.g., never creating NCP, using only consented datasets).

### 1.2 Disinformation & Politics
-   **The "Liar's Dividend"**: The existence of deepfakes allows politicians to claim *real* evidence is fake. It erodes trust in truth itself.
-   **Election Interference**: Synthetic audio (robocalls) or video clips released days before an election ("October Surprise") can sway results before they can be debunked.

### 1.3 Fraud & Cybersecurity
-   **CEO Fraud**: Synthesizing a CEO's voice to order a wire transfer.
-   **Identity Verification**: Bypassing "KYC" (Know Your Customer) video checks for banking apps.

---

## 2. Legal Frameworks

### 2.1 United States
-   **NO federal ban** (as of 2024), but patchwork state laws exist.
-   **California**: Laws against using deepfakes in elections (within 60 days) and non-consensual porn.
-   **Virginia**: First state to criminalize NCP deepfakes.

### 2.2 European Union (EU)
-   **The AI Act**: Categorizes deepfakes as "Limited Risk" but **Mandates Disclosure**. Content must be clearly labeled as artificially manipulated.
-   **GDPR**: The "Right to be Forgotten" applies to biometric data used in training models.

### 2.3 China
-   **CAC Regulations**: Requires "Deep Synthesis" providers to watermark content and verify user identities.

---

## 3. Defense Strategies (Countermeasures)

### 3.1 Provenance (C2PA)
-   **Concept**: Instead of detecting fakes, cryptographic signatures verify *real* photos at the point of capture (Sony/Nikon cameras).
-   **Content Credentials**: An open standard (Adobe, Microsoft) to attach edit history to a file.

### 3.2 Watermarking
-   **Visible**: Logos (easily removed).
-   **Invisible**: Embedding noise patterns (like "SynthID") into the pixels that survive screenshots and compression.
