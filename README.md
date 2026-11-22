# FrameRevive: Diffusion-Based Video Restoration

FrameRevive is an experimental video denoising pipeline that combines pretrained diffusion-based image restoration with optical-flow-guided frame propagation.

## Overview

Running diffusion restoration independently on every video frame is computationally expensive. FrameRevive explores restoring selected keyframes using DDRM and propagating the restored content to intermediate frames using RAFT optical flow.

## Approach

1. Load video frames from the DAVIS dataset.
2. Add synthetic Gaussian noise.
3. Restore every fourth frame using pretrained DDRM with 20 sampling steps.
4. Estimate bidirectional optical flow using RAFT-Small.
5. Propagate restored keyframes to intermediate frames.
6. Apply motion-aware blending to reduce ghosting.
7. Reconstruct the video and evaluate restoration quality.

## Preliminary Results

Evaluated on a 30-frame, 256×256 crop from the DAVIS blackswan sequence with Gaussian noise (σ = 0.2).

| Method | PSNR (dB) ↑ | SSIM ↑ | Temporal Error ↓ |
|---|---:|---:|---:|
| Noisy input | 14.66 | 0.1588 | 0.2075 |
| DDRM + RAFT | 24.44 | 0.6767 | 0.0494 |
| DDRM + RAFT + Motion-Aware Blending | **26.22** | **0.7031** | **0.0433** |

## Current Status

This project is a work in progress. The initial video restoration pipeline is functional, but optical-flow propagation can introduce ghosting around moving objects.

Future work includes occlusion-aware propagation, keyframe interval experiments, and comparisons against video denoising baselines.

## Technologies

Python, PyTorch, DDRM, RAFT, OpenCV, NumPy, Google Colab

## References

- [DDRM](https://github.com/bahjat-kawar/ddrm)
- [Torchvision RAFT](https://docs.pytorch.org/vision/stable/models/raft.html)
- [DAVIS Dataset](https://davischallenge.org/)
