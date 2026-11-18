# ACIT4030 – Machine Learning for 3D Computer Vision

## Final Group Project

This project, developed by a group of three students, improves the Neural Radiance Field (NeRF) implementation from the course book, built on PyTorch3D. It evaluates two scenes:

- **Poster:** A real, static 360° room capture. The improved model combines a hash grid with Mip-NeRF 360 techniques: a proposal network, scene contraction, and disparity sampling.
- **Lego:** A synthetic bulldozer with a bucket that moves over time. The improved model uses D-NeRF, with a deformation network and a hash-grid canonical scene.

## Project Structure

```text
ACIT4030-NeRF/
├── ML3DProject_final.ipynb  # Data loading, baseline, improved models, and evaluation
└── README.md
```

The course data (`lego/`, `poster/`, and `load_nerf_data.py`) is not included in this repository. The notebook installs its dependencies in Google Colab, so a local `requirements.txt` is not needed to run it as described below.

## Results

| Scene  | Model                    |    PSNR ↑ |    SSIM ↑ |   LPIPS ↓ |
| ------ | ------------------------ | --------: | --------: | --------: |
| Poster | Baseline NeRF            |     18.72 |     0.609 |     0.666 |
| Poster | Hash grid + Mip-NeRF 360 | **30.18** | **0.911** | **0.117** |
| Lego   | Baseline NeRF            |     21.62 |     0.862 |     0.147 |
| Lego   | D-NeRF                   | **23.30** | **0.867** | **0.108** |

Evaluation uses 29 held-out Poster views and 20 held-out Lego views.

## How to Run

The notebook is designed for **Google Colab with a GPU**. It reads data from Google Drive and saves results there, so no local installation is needed.

### 1. Upload the data to Google Drive

Put the course data in:

```text
My Drive/acit4030-ass2/
```

This folder must contain `lego/`, `poster/`, and `load_nerf_data.py`. A zip file is also supported.

### 2. Open the notebook in Colab

Open [`ML3DProject_final.ipynb`](ML3DProject_final.ipynb) in Google Colab and select a GPU:

```text
Runtime > Change runtime type > GPU (A100, L4 or T4)
```

### 3. Run the notebook

Run the cells from the top:

- **Cell 1a** mounts Google Drive.
- **Cell 1b** installs PyTorch 2.11 and PyTorch3D. If the runtime restarts, run cells 1a and 1b again.
- **Section 2** contains the `RETRAIN` switch:
  - `RETRAIN = True`: train all models from scratch.
  - `RETRAIN = False`: load finished models or resume after a disconnect.

Training takes about 2 hours in total on an A100.

### 4. Find the results

Results are saved in Google Drive:

```text
My Drive/ACIT4030/outputs/
├── poster/     # Models, metrics, curves, rendered images, and 360° videos
├── lego/       # The same outputs for Lego
└── summary/    # Results table for all models
```

## Important Details

- **Runs in the browser:** Everything runs in Google Colab, so no IDE or local setup is required.
- **GPU required:** Training on a CPU is far too slow.
- **Resumable:** Checkpoints are saved in Google Drive so training can continue after a disconnect.
- **Reproducibility:** Cell 1b installs fixed versions (PyTorch 2.11.0 and PyTorch3D 0.7.8), and all runs use a fixed random seed.
