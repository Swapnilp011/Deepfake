# Deepfake Research Project

This project focuses on the comprehensive analysis, documentation, and benchmarking of deepfake generation and detection tools.

## Documentation Modules

### 1. Generation Frameworks (The "Red Team")
- **[DeepFaceLab](Documentation/DeepFaceLab.md)**: 
    - *Scope*: The modern industry standard.
    - *Contents*: Autoencoder theory, SAEHD architecture explanation, and full step-by-step extraction-to-merge workflow.
- **[Faceswap](Documentation/Faceswap.md)**: 
    - *Scope*: Modular, educational framework.
    - *Contents*: Plugin system architecture, GUI walkthrough, and comparative model analysis (Villain vs Original).
- **[Model Architecture Reference](Documentation/Models_Reference.md)**:
    - *Scope*: Deep dive into specific neural networks.
    - *Contents*: SAEHD (LIAE/DF), Villain, RealFace, and Detection Backbones (XceptionNet, MesoNet).

### 2. Detection & Benchmarking (The "Blue Team")
- **[FaceForensics++](Documentation/FaceForensics.md)**: 
    - *Scope*: The academic gold standard for datasets.
    - *Contents*: Details on the 4 manipulation types (Deepfakes, Face2Face, FaceSwap, NeuralTextures), compression levels (C0/C23/C40), and XceptionNet benchmarking.
- **[Sensity AI](Documentation/SensityAI.md)**: 
    - *Scope*: Commercial threat intelligence.
    - *Contents*: Active vs Passive liveness detection theory and API integration concepts.
- **[Hive AI](Documentation/HiveAI.md)**: 
    - *Scope*: Large-scale content moderation.
    - *Contents*: Taxonomy of generative AI (Diffusion vs GAN) and dataset purification strategies.

### 3. Presentation Resources
- **[PPT Outline Guide](Documentation/Presentation_Guide.md)**: 
    - *Scope*: 16-slide structure for your final presentation.
    - *Contents*: Slide-by-slide topics from Intro to Future Trends.
- **[Ethics & Legal](Documentation/Ethics_and_Society.md)**: 
    - *Scope*: Critical non-technical analysis.
    - *Contents*: Non-consensual deepfakes, Political disinformation, and EU/US laws.
- **[Evaluation Metrics](Documentation/Evaluation_Metrics.md)**: 
    - *Scope*: The Math behind the "Realism".
    - *Contents*: Explaining SSIM, PSNR, LPIPS, and AUC to an audience.

## Project Structure
- `Documentation/`: Detailed technical guides.
- `Data/`: (Placeholder) Store your raw video datasets here.
- `Models/`: (Placeholder) Store your trained `.dat` or `.h5` model files here.
- `Results/`: (Placeholder) Store your detection scores/graphs here.

## Research Goals
1.  **Understand** the underlying Autoencoder and GAN architectures.
2.  **Master** the workflow of creating high-quality datasets for testing.
3.  **Evaluate** current detection limits against compression and unknown attacks.
