# SeedVR2 Batch Upscaler — ComfyUI Workflow

**Drop a folder, come back to 4K. ComfyUI batch image upscaler powered by SeedVR2 (ByteDance).**

![Status](https://img.shields.io/badge/status-ready-green) ![License](https://img.shields.io/badge/license-MIT-blue)

## Overview

Whole-folder image upscaler for ComfyUI using SeedVR2, the SOTA diffusion-based restoration model. Load an entire directory of images, queue once per image, and let the workflow handle sequential upscaling to 4K—no manual per-image re-queuing, no repeated model loads.

**Key Differentiator:** Most upscaler workflows process single images and require manual re-queue for each image in a batch. This workflow uses an `ImageIterator` node to walk your input folder, processing images sequentially through a single persistent model instance. Perfect for stock photography, concept art, and QC'd generation batches.

## Quick Start

1. **Install Custom Nodes**
   ```bash
   cd ComfyUI/custom_nodes
   git clone https://github.com/ComfyUI-Kelin/ComfyUI_Image_Anything.git
   ```
   Then via ComfyUI Manager: install `ComfyUI-SeedVR2_VideoUpscaler` (numz) and `was-node-suite-comfyui` (ltdrdata).

2. **Download Models** (choose one variant)
   - **7B fp16** (default, best quality): ~7.5 GB
     - `seedvr2_ema_7b_fp16.safetensors` → `ComfyUI/models/diffusion_models/`
     - `ema_vae_fp16.safetensors` → `ComfyUI/models/vae/`
   - Or **7B fp8** (faster) / **3B** (fastest) — see Download Links below

3. **Use the Workflow**
   - Place images in `ComfyUI/input/`
   - Open `SeedVR2_Batch_Upscale_v1.json`
   - Queue once per image (or Queue ×N if your UI supports it)
   - Check `ComfyUI/output/[DATE]/` for results

## Download Links

**Primary (7B fp16 — recommended):**
- `seedvr2_ema_7b_fp16.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/seedvr2_ema_7b_fp16.safetensors
- `ema_vae_fp16.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/ema_vae_fp16.safetensors

**Faster variants (7B fp8):**
- `seedvr2_ema_7b_fp8_e4m3fn.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/seedvr2_ema_7b_fp8_e4m3fn.safetensors
- `ema_vae_fp16.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/ema_vae_fp16.safetensors

**3B variants:**
- `seedvr2_ema_3b_fp16.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/seedvr2_ema_3b_fp16.safetensors
- `ema_vae_fp16.safetensors` — https://huggingface.co/numz/SeedVR2_comfyUI/blob/main/ema_vae_fp16.safetensors

## Hardware Requirements

| Config | VRAM | System RAM | Notes |
|--------|------|-----------|-------|
| 7B fp16 (default) | ~16 GB | 33 GB offloaded → 64 GB total recommended | Highest quality |
| 7B fp8 | ~9 GB | 18 GB offloaded → 32 GB OK | ~30% faster (estimated, not yet benchmarked), quality ~= fp16 |
| 3B | ~16 GB | No offload needed | Fastest, fits 16 GB native |

**Tested:** RTX 5080, Windows 11, CUDA 12.1+

## Settings

- **Model:** `seedvr2_ema_7b_fp16.safetensors` (swap to fp8 or 3B as needed)
- **Upscale Resolution:** 4096 max (1024 → 4096, or smaller images scaled proportionally)
- **Color Correction:** LAB (perceptually-aware)
- **Block-Swap:** 36 blocks (default, CPU offload enabled)
- **Torch.compile:** Disabled by default (requires triton-windows to enable)

## Before/After Examples

See `examples/` folder in this repo for before/after comparisons (side-by-side + 100% crop detail):

- `hero1_portrait_skinhair.png` — portrait, skin/hair detail
- `hero2_mesh_fabric.png` — fabric/texture detail
- `hero3_foliage_detail.png` — foliage/detail recovery
- `hero4_small_input_restore.png` — small input (640×800) → 4K restoration

## Troubleshooting

**Missing nodes:** Ensure all 3 custom node packs are installed, then restart ComfyUI.

**Out of Memory:** Switch to 7B fp8 (faster) or 3B variant (smallest footprint).

**torch.compile error on Windows:** Node is disabled by default. Only enable if you have triton-windows installed.

**Slow performance:** If using block-swap (7B fp16), you may need 64 GB system RAM for smooth batching.

## Licensing

**Workflow JSON:** MIT License (original work, Thinni63/TP_AI_63)

**Model Weights:** Apache License 2.0 (ByteDance Seed Team)
- **Attribution Required:** "SeedVR2 by ByteDance Seed Team, licensed under Apache License 2.0."
- See https://huggingface.co/ByteDance-Seed/SeedVR2-7B for full terms

**Custom Nodes:**
- ComfyUI-SeedVR2_VideoUpscaler: Check repository
- was-node-suite-comfyui: MIT License
- ComfyUI_Image_Anything: MIT License

## Links

- **Civitai Listing:** https://civitai.com/models/PLACEHOLDER
- **GitHub Repo:** https://github.com/Thinni63/comfyui-workflows/tree/main/seedvr2-batch-upscale
- **SeedVR2 Paper/Model:** https://huggingface.co/ByteDance-Seed/SeedVR2-7B

## Feedback & Questions

Tested on 16 GB VRAM. If you have questions or see improvements, open an issue or comment on Civitai — I read every submission.

---

**Roadmap:** Video-batch upscaler variant coming soon.
