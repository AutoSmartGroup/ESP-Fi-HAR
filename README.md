# ESP-Fi HAR: A Low-Power WiFi CSI Dataset for Ad-Hoc IoT Human Activity Recognition
## Introduction
ESP-Fi HAR is a publicly available WiFi Channel State Information (CSI) dataset collected using low-power, commodity ESP32 modules. It targets privacy-preserving, energy-efficient Human Activity Recognition (HAR) in resource-constrained ad-hoc IoT networks.

Unlike traditional CSI datasets that rely on high-power Intel 5300 or Atheros network cards, ESP-Fi HAR demonstrates the feasibility of scalable HAR using inexpensive IoT hardware. The dataset covers four indoor environments (Corridor, Office, Meeting Room, Laboratory) and seven daily activities.

We provide a full PyTorch benchmark suite with 20+ deep learning models (CNN, ResNet variants, RNN/LSTM/GRU, ViT, Transformer hybrids, MobileNetV3, EfficientNet-Lite, etc.) optimized for the ESP-Fi CSI format (1×950×52 amplitude).

Dataset & code: [GitHub Repository](https://github.com/AutoSmartGroup/ESP-Fi-HAR)


## Requirements

1. Install `pytorch` and `torchvision` (we use `pytorch==1.12.0` and `torchvision==0.13.0`).
2. `pip install -r requirements.txt`



## Directory Structure
```
Benchmark
├── LICENSE
├── ESP_Fi_model.py          # All model definitions
├── run.py                   # Train/test entry point
├── DATA_LICENSE.txt
├── dataset.py               # ESP-Fi_HAR dataset loader
├── util.py                  # Model/data loading utils
├── latency-cpu.py           # CPU latency benchmark
├── requirements.txt
├── README.md
├── ── Data
    ├── ESP-Fi_HAR
    │   ├── test_amp
    │   ├── train_amp        # Place downloaded dataset here
└── training_logs/           # Generated logs & checkpoints
```


## Supervised Learning
To run models with supervised learning (train & test):  
Run: `python run.py --model [model name] `  

python run.py --model CNN

## Supported Models
- CNN
- ResNet18
- GRU
- LSTM
- Transformer
- MobileNetV3
- EfficientNetLite

Results (accuracy, loss, best model .pth, CSV logs) are saved in ./training_logs/.


## Training Settings

- Train/test split
- Input: CSI amplitude only
- Normalization: Z-score per sample
- Optimizer: AdamW
- Scheduler: CosineAnnealingLR
- Loss Function: CrossEntropyLoss
- Evaluation Metrics:
  - Accuracy
  - Macro-F1

Note: Best model is selected based on training accuracy (no validation split available). Test set is used only for final evaluation.

- Logs and checkpoints are saved in ./training_logs/

- Training CSV includes per-epoch:
TrainAcc, TrainLoss, TestAcc, TestF1, TestLoss

- Final Test CSV includes best model performance.

## Batch Size Recommendation (8GB GPU)

| Model Type | Batch Size |
|------------|-----------|
| GRU | 64 |
| CNN /LSTM / ResNet18 / MobileNetV3 /EfficientNetLite | 32 |
| Transformer | 4 |

---
## Measure CPU inference latency

Latency is measured on CPU only to reflect realistic IoT deployment conditions.

With trained checkpoint:

Run: ` python latency-cpu.py --checkpoint training_logs/best_CNN.pth `

If you want to measure latency for another model, replace best_CNN.pth with the corresponding checkpoint.

## Dataset
### ESP-Fi HAR
 
- **CSI size** : 1 x 950 x 52
- **number of classes** : 7
- **classes** : run, fall, walk, turn, jump, squat, arm wave
- **Indoor scenarios & samples per scene** :
  - Corridor: 560 samples (train/test split included)
  - Office: 560 samples
  - Meeting Room: 560 samples
  - Laboratory: 560 samples

#### Dataset Organization

The ESP-Fi HAR dataset is hierarchically organized across three dimensions: **scenario**, **participant**, and **activity**.  

Each sample follows the structured naming convention: **X-Y-Z-M**, where:

- **X**: Scenario ID (1–4), corresponding to four indoor scenarios:
  1. Corridor
  2. Office
  3. Meeting Room
  4. Laboratory
- **Y**: Participant ID (1–8)
- **Z**: Activity ID (1–7), mapped to predefined actions:
  - 1: run
  - 2: fall
  - 3: walk
  - 4: turn
  - 5: jump
  - 6: squat
  - 7: arm wave
- **M**: Trial number (1–10), indexing repeated trials per participant per scenario

> Example: `2-5-3-7.mat` → Scenario 2 (Office), Participant 5, Activity 3 (walk), Trial 7




## License

### Code

The source code in this repository is licensed under the MIT License.
See the LICENSE file for details.

---

### Dataset

The ESP-Fi HAR dataset is released under the Creative Commons Attribution 4.0 International (CC BY 4.0).

Users are free to use, modify, and distribute the dataset for academic or commercial purposes, provided proper attribution is given by citing the following publication:

Wen et al.,  
"ESP-Fi HAR: A low-power WiFi CSI dataset for Ad-Hoc IoT human activity recognition",  
Ad Hoc Networks, 2026.

This dataset does not contain personally identifiable information.


## Citation

If you use this dataset in your research, please cite:

```bibtex
@article{WEN2026104192,  
   author = {Wen, Zhiwei and Ruan, Yanlin and Wang, Xiaoye and Zhou, Junjie and Gao, Hongliang and Li, Tao},  
   title = {ESP-Fi HAR: A low-power WiFi CSI dataset for Ad-Hoc IoT human activity recognition},  
   journal = {Ad Hoc Networks},  
   volume = {186},  
   pages = {104192},  
   ISSN = {1570-8705},  
   DOI = {https://doi.org/10.1016/j.adhoc.2026.104192},  
   url = {https://www.sciencedirect.com/science/article/pii/S1570870526000582},  
   year = {2026},  
   type = {Journal Article}  
}

