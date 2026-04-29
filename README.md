# Superpixel Segmentation With Edge Guided Local-Global Attention Network

> **Official PyTorch implementation of the TCSVT paper "Superpixel Segmentation With Edge Guided Local-Global Attention Network".**

## Authors

**Mingzhu Xu**<sup>1</sup>, **Zhengyu Sun**<sup>1</sup>, **Yijun Hu**<sup>1</sup>, **Haoyu Tang**<sup>1</sup>, **Yupeng Hu**<sup>1</sup>\*, **Xuemeng Song**<sup>2</sup>\*, **Liqiang Nie**<sup>3</sup>

<sup>1</sup> `Shandong University`  
<sup>2</sup> `Shandong University, Qingdao`  
<sup>3</sup> `Harbin Institute of Technology (Shen Zhen)`  
\* Corresponding author

## Links

- **Paper**: [`IEEE Xplore`](https://doi.org/10.1109/TCSVT.2025.3587485)
- **Code Repository**: [`GitHub`](https://github.com/iLearn-Lab/TCSVT25-ELGANet)

---

## Table of Contents

- [Introduction](#introduction)
- [Highlights](#highlights)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Data Preparation](#data-preparation)
- [Usage](#usage)
- [Evaluation](#evaluation)
- [Demo / Visualization](#demo--visualization)
- [Citation](#citation)
- [License](#license)

---

## Introduction

This project is the official implementation of the paper **"Superpixel Segmentation With Edge Guided Local-Global Attention Network" (ELGANet)**.

Superpixel segmentation is a fundamental task in computer vision. ELGANet introduces an **Edge-Guided mechanism** and a **Local-Global Attention network**, significantly improving boundary adherence and semantic consistency.

This repository provides:
- Complete training and inference code.
- Implementation of the edge-guided local-global attention network.
- Full preprocessing and evaluation pipeline based on the BSDS500 dataset.

---

## Highlights

- **Edge-Guided Attention**: Utilizes edge information to refine superpixel boundaries.
- **Local-Global Context**: Captures both local details and global contextual information.
- **High Efficiency**: Optimized PyTorch implementation with Cython-based post-processing to ensure connectivity.
- **State-of-the-art Performance**: Achieves strong results on ASA, CO, and BR-BP metrics.

---

## Project Structure

```text
.
├── data_preprocessing/    # BSDS500 data preprocessing scripts
├── eval_spixel/           # Evaluation scripts (bash & MATLAB)
├── third_party/           # External dependencies (including Cython connectivity module)
├── main.py                # Main training script
├── run_demo.py            # Inference demo
├── requirements.txt       # Environment dependencies
└── LICENSE                # License file
```

---

## Installation

### 1. Clone the repository

```bash
git clone [https://github.com/iLearn-Lab/ELGANet.git](https://github.com/iLearn-Lab/ELGANet.git)
cd ELGANet
```

### 2. Prerequisites & Cython Compilation
To ensure superpixel connectivity, you need to compile the components in `third_party`:

```bash
cd third_party/cython/
python setup.py install --user
cd ../..
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

---

## Data Preparation 

1. Download the raw dataset from [BSDS500](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/BSR/BSR_full.tgz) and extract it to `<BSDS_DIR>`.
2. Run the preprocessing scripts to generate training and testing data:

```bash
cd data_preprocessing
python pre_process_bsd500.py --dataset=<BSDS_DIR> --dump_root=<DUMP_DIR>
python pre_process_bsd500_ori_sz.py --dataset=<BSDS_DIR> --dump_root=<DUMP_DIR>
cd ..
```
The generated directory structure includes `/train`, `/val`, and `/test` folders, along with corresponding `.txt` path files.

---

## Usage

### Training
Start training with the prepared dataset:
```bash
python main.py --data=<DUMP_DIR> --savepath=<CKPT_LOG_DIR>
```

Fine-tune or resume training using a pretrained model:
```bash
python main.py --data=<DUMP_DIR> --savepath=<CKPT_LOG_DIR> --pretrained=<PATH_TO_THE_CKPT> 
```

### Demo
Generate superpixel visualization results for images in a specific directory:
```bash
python run_demo.py --data_dir=PATH_TO_IMAGE_DIR --output=./demo 
```
The results will be saved in the `./demo/spixel_viz directory`.

---

## Evaluation

Following the evaluation protocol of [FCN](https://github.com/fuy34/superpixel_fcn), we use the [superpixel benchmark](https://github.com/davidstutz/superpixel-benchmark) for evaluation.

1. Download and compile the benchmark code according to the [instructions](https://github.com/davidstutz/superpixel-benchmark/blob/master/docs/BUILDING.md)。
2. Edit variables in `eval_spixel/my_eval.sh` (e.g., `IMG_PATH`, `GT_PATH`, `SEG_PATH`).
3. Run the evaluation script:
   ```bash
   cp eval_spixel/my_eval.sh <path/to/benchmark>/examples/bash/
   cd <path/to/benchmark>/examples/
   bash my_eval.sh
   ```
4. Configure the paths in `plot_benchmark_curve.m` in MATLAB and run it to visualize **ASA**, **CO**, and **BR-BP** curves.

---

## Results

For your convenience in comparative analysis, we have provided the final results of the ELGANet model on four datasets (BSDS, NYU, KITTI, and DRIVE) at the following link [Baidu Netdisk](https://pan.baidu.com/s/1f0vC3-5TmggA3Q5fQcSTDw?pwd=ghph), which you may download at any time for comparison.

---

## Citation

If you use this code or method in your research, please cite our work:

```bibtex
@ARTICLE{ELGANet2025TCSVT,
  author={Xu, Mingzhu and Sun, Zhengyu and Hu, Yijun and Tang, Haoyu and Hu, Yupeng and Song, Xuemeng and Nie, Liqiang},
  journal={IEEE Transactions on Circuits and Systems for Video Technology}, 
  title={Superpixel Segmentation With Edge Guided Local-Global Attention Network}, 
  year={2025},
  volume={35},
  number={12},
  pages={11922-11934},
  doi={10.1109/TCSVT.2025.3587485}}

```

---

## Acknowledgement

- Thanks to [SSN](https://github.com/NVlabs/ssn_superpixels) for providing the connectivity implementation.
- Thanks to [superpixel-benchmark](https://github.com/davidstutz/superpixel-benchmark) for providing the standard evaluation toolkit.
---

## License

This project is released under the Apache License 2.0.
