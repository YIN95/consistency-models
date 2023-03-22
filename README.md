# **Consistency Models** 🌃

**Single-step** image generation with [Consistency Models](https://arxiv.org/abs/2303.01469).

<br />

![image](./assets/consistency_models.png)

**Consistency Models** are a new family of generative models that achieve high sample quality without adversarial training. They support _fast one-step generation_ by design, while still allowing for few-step sampling to trade compute for sample quality.

## Installation

```sh
$ pip install consistency
```

## Usage

```python
import torch
from diffusers import UNet2DModel
from consistency import Consistency

consistency = Consistency(
    model=UNet2DModel(sample_size=224),
    learning_rate=1e-4,
    data_std=0.5,
    time_min=0.002,
    time_max=80.0,
    bins_min=2,
    bins_max=150,
    bins_rho=7,
    initial_ema_decay=0.9,
)

samples = consistency.sample(16)

# multi-step sampling, sample from the ema model
samples = consistency.sample(16, steps=5, use_ema=True)
```

`Consistency` is self-contained with the training logic and all necessary schedules. It subclasses `LightningModule`, so it's supposed to be used with `Trainer`.

```python
from pytorch_lightning import Trainer

trainer = Trainer(max_epochs=8000, accelerator="auto")
trainer.fit(consistency, some_dataloader)
```

A complete example can be found in [this script](https://github.com/junhsss/consistency-models/blob/main/examples/train.py) or [in the colab notebook](https://colab.research.google.com/github/junhsss/consistency-models/blob/main/examples/consistency_models.ipynb).

[**Example wandb workspace**](https://wandb.ai/junhsss/consistency?workspace=user-junhsss) (still experimenting... 🏗)

## Reference

```bibtex
@misc{https://doi.org/10.48550/arxiv.2303.01469,
  doi       = {10.48550/ARXIV.2303.01469},
  url       = {https://arxiv.org/abs/2303.01469},
  author    = {Song, Yang and Dhariwal, Prafulla and Chen, Mark and Sutskever, Ilya},
  keywords  = {Machine Learning (cs.LG), Computer Vision and Pattern Recognition (cs.CV), Machine Learning (stat.ML), FOS: Computer and information sciences, FOS: Computer and information sciences},
  title     = {Consistency Models},
  publisher = {arXiv},
  year      = {2023},
  copyright = {arXiv.org perpetual, non-exclusive license}
}
```
