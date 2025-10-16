# Rethinking-Generalisation (Data Mining)

## Overview
**Rethinking-Generalisation** is a reproducible study that examines why modern deep neural networks often **generalize well despite being heavily over-parameterized**. The project re-implements core experiments, extends them to additional corruptions, and contrasts **explicit** vs **implicit** regularization effects on model performance and robustness.

> TL;DR — We show how deep nets can fit data perfectly (even noise) yet still generalize on real data, highlighting the importance of the **optimization process**, **data structure**, and **implicit biases** over classical capacity control alone.

---

## Research Basis
This project is **inspired by and based on** the findings in the research paper:

- **“Understanding Deep Learning Requires Rethinking Generalization” — Zhang et al., 2017.**

### Key Ideas We Re-create & Extend
- **Memorization vs. generalization:** Modern networks can **fit random labels** (training error → 0) but fail to generalize, showing that classical complexity measures don’t fully explain generalization.
- **Explicit regularization isn’t sufficient:** Techniques like weight decay, dropout, and data augmentation help but **do not fully explain** why deep nets generalize.
- **Implicit regularization matters:** The **optimizer + architecture + training dynamics** induce biases (e.g., preference for “simpler” solutions) that are critical for generalization.
- **Data matters:** Real-world structure (natural images) drives generalization, whereas **shuffled/label-corrupted** data exposes memorization.

---

## Objectives
1. **Reproduce** canonical experiments showing that deep nets can fit **true** data and **randomized** labels.
2. **Quantify** the effects of **explicit** (weight decay, dropout, augmentation) vs **implicit** (optimizer/architecture) regularization.
3. **Stress-test** generalization with **input corruptions** (noise, blur, occlusion, label noise) and report robustness deltas.
4. **Document** takeaways for practitioners: which knobs matter most for stable generalization on small/medium datasets.

---

## Methods

### Datasets
- **CIFAR-10** (primary), with optional variants:
  - **Label noise:** flip a % of labels at random.
  - **Input corruptions:** Gaussian noise, blur, cutout/occlusion, pixelation.

### Models & Training
- **CNN backbones:** small ConvNet / ResNet-like.
- **Optimizers:** SGD (with/without momentum), Adam.
- **Schedulers:** Step/ cosine decay; early stopping variants.
- **Explicit regularizers:** weight decay (L2), dropout, Mixup/CutMix (optional), augmentation (flip/crop/color jitter).

### Experimental Conditions
1. **Clean labels, standard aug**
2. **Random labels (100%)**
3. **Partial label noise (e.g., 20%, 40%)**
4. **Input corruptions at test time** (CIFAR-10-C-style or custom)
5. **Ablations:** with/without weight decay, dropout, data aug; SGD vs Adam.

---

## Metrics & Evaluation
- **Accuracy / Error** on clean and corrupted test sets.
- **Generalization gap:** train vs test performance.
- **Robustness deltas:** accuracy drop from clean → corrupted inputs.
- **Training dynamics:** loss/accuracy curves; time-to-fit noisy labels.

---

## Results (Example Template — fill with your runs)
| Setting                               | Train Acc | Test Acc | Gen Gap | Robustness Δ (avg) |
|---------------------------------------|-----------|----------|---------|--------------------|
| Clean labels + Aug + WD + SGD         | 99.9%     | 92.4%    | 7.5%    | −11.8 pts          |
| Clean labels + No Aug/No WD           | 100%      | 87.1%    | 12.9%   | −18.4 pts          |
| Random labels (100%)                  | 100%      | 10.2%    | 89.8%   | —                  |
| 40% label noise + Aug + WD            | 100%      | 74.0%    | 26.0%   | −21.3 pts          |

**Takeaways (sample):**
- Networks **memorize** noisy labels (train → 100%) but **don’t generalize** (test ≈ chance).
- **Augmentation + WD** consistently improves test accuracy and robustness, but **cannot explain** generalization alone.
- **SGD** often yields better generalization than Adam at matched training loss → evidence of **implicit bias**.

---

## Repository Structure (suggested)
