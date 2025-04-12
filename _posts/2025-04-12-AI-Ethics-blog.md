---
layout: post
title: "Reconciling privacy and accuracy in AI for Medical Imaging"
date: 2025-04-12
---

This is a blog post about the paper **"Reconciling privacy and accuracy in AI for Medical Imaging"** published in *Nature Machine Intelligence* in 2024 for my AI Ethics class.

---
## Introduction

As artificial intelligence (AI) continues to revolutionize healthcare, **medical imaging** stands at the forefront of this transformation. From radiology to dermatology, deep learning models are now capable of achieving diagnostic performance on par with medical professionals. However, these advancements come with a caveat—**patient privacy**.

In the era of data-driven models, **reconstruction attacks and data leakage** have emerged as serious concerns. Even anonymized medical images, when used in model training, can potentially be reconstructed or traced back to individuals. To address this, the 2024 paper titled **"Reconciling Privacy and Accuracy in AI for Medical Imaging"**, published in *Nature Machine Intelligence*, presents a compelling framework to navigate the trade-off between **privacy preservation** and **model performance**.

---

## Motivation

The key tension addressed in this paper is:

> **How can we ensure patient privacy while still maintaining high-performing AI models trained on sensitive medical imaging data?**

Traditional approaches, such as data de-identification, often fall short due to the high-dimensional nature of medical data. **Differential Privacy (DP)** has emerged as a mathematically grounded framework to mitigate privacy risks. However, its application in the **medical imaging domain** has been limited by concerns over degraded model performance.

This study aims to **quantitatively evaluate this trade-off** across realistic settings using state-of-the-art datasets and models.

---

## Datasets and Tasks

The authors test their hypotheses across three diverse and clinically relevant datasets:

1. **RadImageNet**: A large-scale dataset of radiological images used for classification.
2. **HAM10000**: Dermatoscopic images used for classifying skin lesions.
3. **MSD Liver (Medical Segmentation Decathlon)**: Abdominal CT scans used for liver segmentation.

Each of these datasets represents a **different medical imaging task**—classification and segmentation—enabling a generalizable analysis of privacy-accuracy trade-offs.

---

## Methodology

### 1. **Differential Privacy Training with DP-SGD**
- The core privacy mechanism used is **Differentially Private Stochastic Gradient Descent (DP-SGD)**.
- Noise is added to the gradients during model updates to ensure that the contribution of any individual image is masked.
- The strength of privacy is controlled by the **privacy budget ε (epsilon)**:
  - Smaller ε = stronger privacy, but higher noise
  - Larger ε = weaker privacy, but better utility

### 2. **Evaluation Metrics**
- **Utility Metrics**:
  - *Matthews Correlation Coefficient (MCC)* for classification
  - *Dice Coefficient* for segmentation
- **Privacy Metrics**:
  - *Empirical Reconstruction Attacks*: The authors use feature inversion techniques to reconstruct images from trained models and assess them using **Structural Similarity Index (SSIM)**.

### 3. **Threat Model**
- Assumes an adversary with **white-box access** to the trained model.
- This is a stronger, more realistic threat model than black-box settings used in some prior work.

---

## Results and Analysis

###  Trade-off Between Privacy and Accuracy

| Dataset       | Task         | Baseline (ε = ∞) | ε = 32 | ε = 16 | ε = 8  | ε = 1  |
|---------------|--------------|------------------|--------|--------|--------|--------|
| RadImageNet   | Classification | 71.83% MCC        | 69.99% | 67.80% | 64.12% | 58.90% |
| HAM10000      | Classification | 87.65% MCC        | 85.92% | 83.70% | 80.11% | 75.44% |
| MSD Liver     | Segmentation  | 0.894 Dice        | 0.879  | 0.858  | 0.827  | 0.768  |

**Interpretation**:
- Models trained with **ε = 32** show only minor performance degradation, making it a **sweet spot** between privacy and accuracy.
- As ε decreases (stronger privacy), utility declines—but not catastrophically.
- The drop in accuracy is gradual and manageable, especially for ε values between 8 and 32.

---

### Privacy Protection Against Reconstruction Attacks

The authors reconstruct images from trained models and measure SSIM between original and reconstructed images.

- **High ε** (weaker DP): Reconstructed images still appear noisy and indistinct.
- **Low ε** (stronger DP): Reconstructions are nearly useless—mostly random noise.
- Even with ε = 32, reconstructions lack fine detail, suggesting strong **practical privacy** even if the theoretical ε is higher.

> “We find that the conventional worst-case interpretation of ε may be overly pessimistic in real-world healthcare applications.” — *Authors*

---

## Key Takeaways

1. **DP Can Be Practical for Medical Imaging**: Contrary to conventional wisdom, differentially private training **can maintain high model performance**, especially at moderate ε levels (e.g., 16–32).

2. **Stronger Threat Models Yield Better Insights**: Using realistic white-box adversaries provides a more honest view of privacy risks.

3. **Empirical ≠ Theoretical**: Even when theoretical guarantees are weaker (i.e., higher ε), **empirical reconstruction attacks still fail**, indicating a gap between worst-case analysis and real-world threats.

4. **Dataset and Task Matter**: The impact of DP varies across modalities—segmentation tasks seem more sensitive to privacy noise than classification.

---

## Broader Implications

This paper contributes to a growing body of work showing that **privacy-preserving AI is not only possible but also practical** in sensitive domains like healthcare. It sets the stage for **differential privacy** to be included as a standard part of regulatory frameworks (e.g., HIPAA, GDPR) governing AI systems trained on medical data.

Furthermore, the study promotes a **rethinking of ε**. Instead of striving for very low values that harm utility, it advocates for **context-aware calibration** of privacy budgets, balancing theoretical risk with empirical evidence.

---

## GitHub & Reproducibility

The authors have made their **code and implementation public**:
> 🔗 [GitHub Repository – RePrAAIMI](https://github.com/a1302z/RePrAAIMI)

It includes:
- DP training pipelines using Opacus
- Evaluation scripts for reconstruction attacks
- Preprocessing pipelines for all datasets

---

## Final Thoughts

The balance between **privacy** and **accuracy** has long been a tightrope for AI in healthcare. This paper proves that with **carefully engineered DP mechanisms**, we don’t have to sacrifice one for the other. It’s a **landmark contribution** that bridges the gap between theoretical guarantees and clinical practicality.

If you’re working in **medical AI, federated learning, or privacy-preserving ML**, this paper is a must-read and a roadmap for building trustworthy, private-by-design AI systems.


