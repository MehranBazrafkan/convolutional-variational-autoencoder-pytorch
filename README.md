
# 🧠 Convolutional Variational Autoencoder (VAE) in PyTorch

A modular and customizable implementation of a **Convolutional Variational Autoencoder (VAE)** in PyTorch, designed for image reconstruction and unsupervised representation learning. Built with residual blocks, RMS normalization, and flexible architecture scaling.

## 🚀 Features

- 🔁 **Encoder–Decoder VAE** with reparameterization trick
- 🧱 **Residual blocks** with RMS normalization
- 🧩 Fully modular, easy to customize
- 🔄 **Downsampling/Upsampling** using `einops` and `nn.Conv2d`
- 🧪 **Dropout regularization** for improved generalization
- ⚡ Fast inference with `.reconstruct()` method
- 🧼 Clean, production-ready code

## 📦 Installation

```bash
pip install convolutional-variational-autoencoder-pytorch

```

## 📁 Project Structure

```bash
convolutional-variational-autoencoder-pytorch/
├── convolutional_variational_autoencoder_pytorch/
│   ├── __init__.py
│   └── module.py        # All architecture classes and logic
├── pyproject.toml
├── LICENSE
└── README.md

```

## 🚀 Quick Start

### 1. Import the package and create the model

```python
import torch
from convolutional_variational_autoencoder_pytorch import VariationalAutoEncoder

model = AutoEncoder(
    dim=64,
    dim_mults=(1, 2, 4, 8),
    dim_latent=128,
    image_channels=3
)

```

### 2. Forward pass and reconstruction

```python
x = torch.randn(8, 3, 256, 256)  # batch of images
x_recon, mu, logvar = model(x)

# Or just get the reconstruction
x_recon = model.reconstruct(x)

```

### 3. Training step (sample loop)

```python
import torch.nn.functional as F
optimizer = torch.optim.Adam(model.parameters(), lr=1e-4)

def train_step(x):
    model.train()
    optimizer.zero_grad()
    x_recon, _, _ = model(x)
    loss = F.mse_loss(x_recon, x)
    loss.backward()
    optimizer.step()
    return loss.item()
    
```

### 🧠 Model Output

- `x_recon`: Reconstructed image

- `mu`: Mean of the latent distribution

- `logvar`: Log-variance of the latent distribution

## ⚙️ Configuration Options

| Argument | Type | Default | Description |
|--|--|--|--|
| `dim` | `int` | `64` | Base number of channels |
| `dim_mults` | `tuple` | `(1, 2, 4, 8)` | `Multipliers for feature map dimensions |
| `dim_latent` | `int` | `64` | Latent space dimension |
| `image_channels` | `int` | `3` | Input/output image channels (e.g., 3) |
| `dropout` | `float` | `0.0` | Dropout probability |

## 🧪 Example: Loss Function

Here's a basic VAE loss function combining reconstruction and KL divergence:

```python
def vae_loss(x, x_recon, mu, logvar):
    recon_loss = F.mse_loss(x_recon, x, reduction='sum')
    kl_div = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
    loss = recon_loss + kl_div
    return loss

```

## 🙋‍♂️ Author

Developed by [Mehran Bazrafkan](mailto:mhrn.bzrafkn.dev@gmail.com)

> Built from scratch with inspiration from modern deep generative modeling architectures. This package reflects personal experience with VAEs and convolutional design patterns.

## ⭐️ Support & Contribute

If you find this project useful, consider:

- ⭐️ Starring the repo

- 🐛 Submitting issues

- 📦 Suggesting improvements

## 🔗 Related Projects

- [convolutional-autoencoder-pytorch · PyPI (Implemented by me)](https://pypi.org/project/convolutional-autoencoder-pytorch/)

- [PyTorch VAE Tutorial (external)](https://github.com/pytorch/examples/tree/main/vae)

## 📜 License

This project is licensed under the terms of the [`MIT LICENSE`](https://github.com/MehranBazrafkan/convolutional-variational-autoencoder-pytorch/blob/main/LICENSE).
