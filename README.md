# 🛡️ Adversarial Attacks on ResNet-34

## 📌 Overview

This project implements and analyzes adversarial attacks on a pretrained ResNet-34 model trained on ImageNet-1K. We evaluate the model’s robustness to various perturbation methods by generating adversarial test sets and measuring top-1 and top-5 accuracy drops. The attacks include:

- **FGSM (Fast Gradient Sign Method)** — pixel-wise single-step
- **PGD (Projected Gradient Descent)** — pixel-wise multi-step
- **Patch-based PGD** — spatially localized attacks
- **Transferability Evaluation** — testing adversarial examples on other models (e.g., DenseNet-121)

---

## 📁 Dataset

- **Directory**: `./TestDataSet/`
- **Samples**: 500 images from 100 ImageNet classes
- **Labels**: `labels_list.json` provides class index-to-name mapping
- **Normalization**:
  ```python
  mean = [0.485, 0.456, 0.406]
  std = [0.229, 0.224, 0.225]
✅ Tasks Implemented
🔹 Task 1: Baseline Evaluation
Evaluated ResNet-34 on clean test set

Reported top-1 and top-5 accuracies
🔹 Task 2: FGSM Attack (ε = 0.02)
Applied untargeted FGSM to generate Adversarial Set 1

Visualized perturbed vs. original images

Measured accuracy drop

🔹 Task 3: PGD Attack (ε = 0.02, α = 0.005, 140 steps)
Multi-step gradient attack

Generated Adversarial Set 2

Notable performance degradation

🔹 Task 4: Patch-Based PGD Attack
Localized attack on 32×32 patch

ε = 0.5 for perturbation limit

Generated Adversarial Set 3

🔹 Task 5: Transferability
Tested DenseNet-121 on clean and adversarial sets

Assessed cross-model attack effectiveness

📊 Results
🔸 ResNet-34 vs. DenseNet-121 Accuracy
| **Attack Type** | **ResNet-34<br>Top-1** | **ResNet-34<br>Top-5** | **DenseNet-121<br>Top-1** | **DenseNet-121<br>Top-5** |
| --------------- | ---------------------- | ---------------------- | ------------------------- | ------------------------- |
| **No Attack**   | 76.00%                 | 94.20%                 | 88.00%                    | 98.40%                    |
| **FGSM**        | 3.40%                  | 21.20%                 | 62.40%                    | 89.40%                    |
| **PGD**         | 0.00%                  | 1.20%                  | 65.60%                    | 93.80%                    |
| **Patch PGD**   | 4.40%                  | 35.60%                 | 85.20%                    | 98.00%                    |

FGSM causes catastrophic failure for ResNet-34 but is less damaging to DenseNet-121.

PGD completely disables ResNet-34 but DenseNet-121 remains fairly robust.

Patch attacks degrade ResNet modestly, but DenseNet shows high resilience.

🖼️ Example Visualizations
Example bar plots and visual comparisons (clean vs. adversarial predictions) are included in the notebook.

FGSM (ε = 0.02)

PGD (ε = 0.02)

Patch‑PGD (ε = 0.5)

🔬 Ablation Studies
Ablations on:

FGSM ε vs. Accuracy — saturation observed beyond ε = 0.01

PGD steps — top-1 hits zero quickly; top-5 declines gradually

Patch PGD — affected by step size, restarts, target class

📌 Patch PGD Hyperparameters
| **Steps** | **Step Size** | **Restarts** | **Top-1** | **Top-5** | **Target Class** |
| --------- | ------------- | ------------ | --------- | --------- | ---------------- |
| 150       | 0.015         | 8            | 2.80%     | 84.60%    | Untargeted       |
| 100       | 0.01          | -            | 11.20%    | 60.40%    | 5                |
| 100       | 0.05          | 8            | 1.00%     | 44.20%    | 10               |
| 150       | 0.004         | 10           | 0.20%     | 37.20%    | 12               |

📓 Notebook Files
Notebook	Purpose
Main_dl.ipynb	Full pipeline: loading, attacks, evaluation, visualizations
Patch_ablations.ipynb	Patch PGD parameter sweep & analysis

🧠 Author
Jeevana Bhumireddy (jb8855)

📎 Citations
PyTorch pretrained ImageNet models

Goodfellow et al., “Explaining and Harnessing Adversarial Examples,” 2015

💻 Instructions to Run
Install dependencies:
pip install torch torchvision matplotlib numpy tqdm
Place test images in:
./TestDataSet/
Run:
Main_dl.ipynb

