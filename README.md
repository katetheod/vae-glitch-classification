# vae-glitch-classification
Unsupervised glitch classification in LIGO gravitational wave data using Variational Autoencoders and Spectral Clustering.
# Variational Autoencoder for Gravitational-Wave Glitch Representation Learning

## Overview

This project investigates the use of **Variational Autoencoders (VAEs)** to learn compact latent representations of spectrograms from the **Gravity Spy** dataset. The goal is to determine whether an unsupervised deep learning model can capture the morphology of gravitational-wave detector glitches and produce latent representations suitable for clustering and visualization.

The model is trained solely to reconstruct spectrogram images without using class labels. After training, the learned latent space is analysed using dimensionality reduction and the clustering technique, **Spectral Clustering**.

---

## Dataset

The project uses spectrogram images from the **Gravity Spy** dataset.

For each glitch, two spectrograms are used:

* **0.5 s time window**
* **2.0 s time window**

The ten most common glitch classes are considered:

* 1080Lines
* Blip
* Extremely Loud
* Fast Scattering
* Koi Fish
* Low Frequency Burst
* Low Frequency Lines
* No Glitch
* Scattered Light
* Tomte

The spectrograms are cropped to remove axes and labels before being resized to **128 × 128 pixels**.

---

## Model Architecture

### Encoder

The encoder consists of three convolutional blocks:

* Conv2D (2 → 32)
* Conv2D (32 → 64)
* Conv2D (64 → 128)

Each convolution is followed by a ReLU activation.

The final feature map is flattened and projected into two vectors:

* latent mean (μ)
* latent log variance (log σ²)

using fully connected layers.

---

### Latent Space

* Latent dimension: **128**
* Reparameterization trick used during training
* KL divergence regularizes the latent distribution toward a unit Gaussian

---

### Decoder

The decoder mirrors the encoder using transposed convolutions:

* ConvTranspose2D (128 → 64)
* ConvTranspose2D (64 → 32)
* ConvTranspose2D (32 → 2)

The output reconstructs both spectrogram views simultaneously.

---

## Loss Function

Training minimizes

**Loss = Reconstruction Loss + β × KL Divergence**

where

* Reconstruction Loss = Mean Squared Error (MSE)
* KL Divergence = regularization term
* β is gradually increased during training (KL warm-up)

---

## Training

Typical hyperparameters:

| Parameter        | Value |
| ---------------- | ----- |
| Optimizer        | Adam  |
| Learning rate    | 5e-4  |
| Batch size       | 32    |
| Epochs           | 20    |
| Latent dimension | 128   |

---

## Latent Space Analysis

After training:

1. The encoder is frozen.
2. The latent mean vectors (μ) are extracted.
3. Features are standardized.
4. UMAP projects the latent space into two dimensions.
5. Clustering is performed using Spectral Clustering

Cluster quality is evaluated using:

* Adjusted Rand Index (ARI)
* Normalized Mutual Information (NMI)

---

## Results

The learned latent space successfully separates several glitch morphologies.

Example performance:

* **ARI ≈ 0.53**
* **NMI ≈ 0.67**

Several glitch types form distinct clusters, while visually similar classes naturally overlap.


## Installation

Clone the repository

```bash
git clone https://github.com/<username>/<repository>.git
cd <repository>
```

Install dependencies

```bash
pip install -r requirements.txt
```









