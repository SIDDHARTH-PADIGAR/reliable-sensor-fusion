# What happens when satellite sensors become unreliable?

### The problem

Optical satellite imagery can be obstructed by clouds and shadows. SAR imagery has its own challenges, including speckle noise.

When combining these different sources, how does a model know which sensor to trust?

I wanted to investigate whether teaching a model to assign different weights to each sensor could improve flood detection.

### What I built

I used the public Sen1Floods11 dataset, containing Sentinel-1 SAR imagery, Sentinel-2 optical imagery and expert-labelled flood maps.

I developed two models:

- **Standard U-Net:** Combines both satellite data sources.
- **Reliability-aware U-Net:** Learns pixel-level weights to determine how much each sensor contributes.

### What I tested

I compared both models under five conditions:

- Clean satellite imagery.
- Simulated optical cloud and shadow obstruction.
- Simulated SAR speckle noise.
- Unseen test data.
- A geographically separate Bolivia dataset.

### What I found

| Evaluation | Standard U-Net | Reliability-aware |
|---|---:|---:|
| Validation IoU | 0.7258 | **0.7746** |
| Test IoU | 0.7668 | **0.8089** |
| Bolivia IoU | 0.7645 | 0.7644 |
| Optical degradation IoU | **0.4323** | 0.2105 |
| SAR degradation IoU | 0.7310 | **0.7739** |

### So, what does this tell us?

The learned weighting approach improved flood detection on standard test data and under simulated SAR noise.

However, it performed considerably worse when optical imagery was obscured by simulated clouds and shadows. It also showed no meaningful improvement on the Bolivia geographic holdout.

In other words, learning to weight sensors does not automatically make a fusion model reliable.

### Why I found this interesting

Auric's published Computer Vision Researcher requirements include working with EO and SAR imagery, multimodal data and challenging image conditions.

This project gave me an opportunity to explore one part of that problem space hands-on, build working models and investigate where the approach succeeds and fails.

**The interesting part isn't just that one model scored higher. It's understanding why that improvement disappears under certain conditions.**

### Technical details

- Dataset: Sen1Floods11 v1.1
- Framework: PyTorch
- Models: U-Net and reliability-weighted U-Net
- Hardware: NVIDIA Tesla T4
- Evaluation: IoU and F1
- Visual evidence: SAR, optical imagery, ground truth and both model predictions.
