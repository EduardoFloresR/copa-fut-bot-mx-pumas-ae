# Reproduction instructions

This document will describe how to reproduce the project results.

## 1. Environment

Recommended environment:

- Windows 11
- WSL2 Ubuntu
- NVIDIA GPU
- Python 3.12
- PyTorch with CUDA
- SAM 3

## 2. Conda environment

```bash
conda activate futbotmx-sam3
3. Data

Original videos should be placed in:

data/raw/

These files are not tracked by Git.

4. Expected workflow
video
→ clip extraction
→ frame extraction
→ SAM 3 segmentation
→ mask postprocessing
→ tracking
→ event detection
→ visualization
→ demo video
5. Reproducibility notes

All relevant parameters should be defined in:

configs/default.yaml
