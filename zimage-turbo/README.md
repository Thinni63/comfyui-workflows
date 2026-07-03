# Z-Image-Turbo — QwenVL Dual-Mode Auto-Prompt

> Fast text-to-image S3-DiT diffusion model by Tongyi-MAI (Alibaba). QwenVL vision model auto-enhances prompts or reads reference image styles — choose your mode. Dual-orientation, 12-step turbo inference, Apache-2.0 licensed.

[![VRAM](https://img.shields.io/badge/VRAM-~13%20GB-blue)]()
[![License](https://img.shields.io/badge/License-Apache--2.0-green)]()
[![Civitai](https://img.shields.io/badge/Civitai-TP__AI__63-orange)](https://civitai.red/models/2742740/z-image-turbo-qwenvl-dual-mode-auto-prompt)

## Overview

Fast text-to-image S3-DiT diffusion model by Tongyi-MAI (Alibaba) for ComfyUI. QwenVL vision model auto-enhances prompts or reads image styles — choose your mode. Dual-orientation support: landscape 1920×1088 (16:9) or portrait 1088×1920 (9:16) with zero rewiring. 12-step turbo inference runs smooth on 16 GB VRAM. Apache-2.0 licensed — commercial use OK.

### Table of Contents

- [Features](#features)
- [Required Models](#required-models)
- [Download Links](#download-links)
- [Required Custom Nodes](#required-custom-nodes)
- [How to Use](#how-to-use)
- [Settings & Parameters](#settings--parameters)
- [Performance Tips](#performance-tips)
- [Model Attribution](#model-attribution--licensing)

## Features

- **Mode A: Keyword → Auto-Expand** — Type a subject (e.g., "mountain landscape") → QwenVL PromptEnhancer expands it to a rich visual prompt → generate
- **Mode B: Reference Image → Style Capture** — Drop any reference image → QwenVL describes it → generates a new image inspired by its style & composition
- **Dual Orientation** — One number switch: 1920×1088 landscape (16:9) or 1088×1920 portrait (9:16); workflow auto-reconfigures, no node rewiring
- **Pure Z-Image-Turbo** — Apache-2.0 S3-DiT (Alibaba); no FLUX dev components, no licensing gatekeeping
- **Turbo 12-Step Sampling** — euler sampler + beta scheduler + ModelSamplingAuraFlow (shift=3) + FluxGuidance for balanced quality/speed
- **16 GB Blackwell Ready** — Tested on RTX 5080 (NVFP4 UNet); ~44 seconds per 1920×1088 image at 12 steps
- **Flexible Quality/Speed** — 6 steps ≈ 25s; 9 steps ≈ 33s; default 12 steps ≈ 44s; raise to 15 for max detail

## Required Models

All models below are freely downloadable from HuggingFace (~13 GB total).

| Model File | Size | Category | Location |
|---|---|---|---|
| `z_image_turbo_nvfp4.safetensors` | 4.5 GB | UNet — RTX 50 / Blackwell ⭐ recommended for 16 GB | `ComfyUI/models/diffusion_models/` (or `unet/`) |
| `z_image_turbo_fp8.safetensors` *(any name)* | ~6 GB | UNet — any GPU, 16 GB (community quant, Civitai) | `ComfyUI/models/diffusion_models/` (or `unet/`) |
| `z_image_turbo_bf16.safetensors` | 12.3 GB | UNet — any GPU, needs 24 GB+ VRAM | `ComfyUI/models/diffusion_models/` (or `unet/`) |
| `qwen_3_4b.safetensors` | 7.5 GB (BF16) | Text Encoder | `ComfyUI/models/text_encoders/` |
| `qwen_3_4b_fp8_mixed.safetensors` | 5.6 GB (FP8) | Text Encoder — lower VRAM alternative | `ComfyUI/models/text_encoders/` |
| `ae.safetensors` | ~600 MB | Autoencoder VAE (encode/decode latents) | `ComfyUI/models/vae/` |
| Qwen3-VL-2B-Instruct | ~3 GB (auto-downloaded) | Vision model — image read & prompt enhancement | Handled by AILab node on first queue |

Pick **one** UNet variant based on your GPU. Only one of `qwen_3_4b.safetensors` / `qwen_3_4b_fp8_mixed.safetensors` is needed.

## Download Links (verified HuggingFace sources)

All primary files from [Comfy-Org/z_image_turbo](https://huggingface.co/Comfy-Org/z_image_turbo).

### UNet (pick one)
- **`z_image_turbo_nvfp4.safetensors`** (NVFP4, RTX 50/Blackwell) → `split_files/diffusion_models/`
- **`z_image_turbo_fp8.safetensors`** *(community quant)* → search "Z-Image-Turbo FP8" on Civitai
- **`z_image_turbo_bf16.safetensors`** (full precision, 24 GB+ VRAM) → `split_files/diffusion_models/`

> Workflow file loads `z_image_turbo_nvfp4.safetensors` by default — change the filename in the UNETLoader node to match whatever file you downloaded.

### Text Encoder
- **`qwen_3_4b.safetensors`** (BF16, default) → `split_files/text_encoders/`
- **`qwen_3_4b_fp8_mixed.safetensors`** (FP8, saves VRAM) → `split_files/text_encoders/`

### VAE
- **`ae.safetensors`** → `split_files/vae/`

> Workflow file uses `z_image_turbo_nvfp4.safetensors` + `qwen_3_4b.safetensors` by default. Swap filenames in the UNETLoader / CLIPLoader nodes if you use a different variant. QwenVL auto-downloads from the AILab node on first use.

## Required Custom Nodes

Install both of these in your ComfyUI `custom_nodes/` folder (or via ComfyUI Manager):

| Custom Node | Purpose |
|---|---|
| **ComfyUI-QwenVL** (AILab / 1038lab) | PromptEnhancer node (expand keywords) + VL image-to-text (read reference images) |
| **ComfyUI-Easy-Use** (vjumpkung) | `anythingIndexSwitch` for mode toggle (keyword vs. image) + orientation picker (landscape/portrait) |

### Quick Install (via Manager)

If you use ComfyUI Manager, search and install:
- `ComfyUI-QwenVL` (AILab)
- `ComfyUI-Easy-Use` (vjumpkung)

Manual install (git clone into `custom_nodes/`):
```bash
cd ComfyUI/custom_nodes

git clone https://github.com/1038lab/ComfyUI-QwenVL
git clone https://github.com/yolain/ComfyUI-Easy-Use
```

## How to Use

### Step 1: Download & Install
1. Download & place models in `ComfyUI/models/diffusion_models/`, `/text_encoders/`, `/vae/`
2. Install the 2 custom node packs via ComfyUI Manager

### Step 2: Load Workflow
1. Open ComfyUI in your browser
2. Click **Load** (top menu)
3. Select `ZIMG_Turbo_AUTO_dual_v1.json` from this folder

### Step 3: Choose Mode
- **Mode 0 (top-left switch):** Keyword input → auto-expand via QwenVL
- **Mode 1:** Reference image → auto-describe via QwenVL

**Mode A (Keyword):**
- Type a prompt seed → QwenVL expands it to 30–50 words → sampler enhances details
- Example subjects (copy-paste ready):
  - `elegant woman, golden hour rooftop, cinematic editorial` ← workflow default
  - `mountain lake at golden hour`
  - `cozy coffee shop morning`
  - `futuristic rainy city night`

**Mode B (Reference Image):**
- Drag a reference image to the LoadImage node → QwenVL analyzes composition, color, mood → generates a new image inspired by that style
- Best for: style transfer, "I want something like this but different subject"

### Step 4: Set Orientation
- Orientation = 0: 1920×1088 (16:9 landscape)
- Orientation = 1: 1088×1920 (9:16 portrait)
- Toggle updates resolution, aspect ratio, and sampler dynamically — no manual node changes needed

### Step 5: Queue & Generate

Click **Queue Prompt**. Output saves as PNG with a live preview in ComfyUI.

**Generation time (RTX 5080, NVFP4 UNet):**
- 6 steps ≈ 25s
- 9 steps ≈ 33s
- 12 steps (default) ≈ 44s

## Settings & Parameters

| Parameter | Value | Notes |
|-----------|-------|-------|
| **Sampler** | euler + beta scheduler | Built-in ComfyUI, no extra nodes required |
| **Base Steps** | 12 | Default; adjustable 6–15 via steps slider |
| **CFG Scale** | 5.0 | Guidance strength; range 3.0–7.0 |
| **ModelSamplingAuraFlow** | shift = 3 | Distillation-optimized noise scheduler |
| **FluxGuidance** | ON | Stabilizes output diversity |
| **Seed** | Randomize or fix | Fix for reproducibility |
| **Output Format** | PNG | With ComfyUI preview |

## Performance Tips

- **Keyword Mode Tricks** — Use descriptors: "portrait of woman, soft studio lighting, sharp focus" yields better results than bare subject names
- **Reference Mode Tips** — Clear, well-composed images work best; abstract/blurry references may confuse the QwenVL reader
- **Speed Tuning** — 6 steps ≈ 25s; 9 steps ≈ 33s; 12 steps ≈ 44s. Quality gain plateaus after 15 steps
- **VRAM Efficiency** — Workflow stays under 12 GB active; safe for concurrent desktop use
- **Batch Generation** — Queue 5–10 images in one session; model stays loaded between generations
- **CFG Sensitivity** — Z-Image responds well to 5.0–6.0; above 7.0 may degrade details

## Notes & Disclosure

- **AI-Generated Content:** All example outputs are AI-generated by Z-Image-Turbo. Suitable for creative projects, design exploration, and stock footage. Disclose AI generation per local regulations
- **Hardware Tested:** RTX 5080 16GB VRAM (NVFP4 UNet + BF16 CLIP), CUDA 12.6, Blackwell SM120
- **VRAM Usage:** NVFP4 + BF16 CLIP ≈ 13 GB peak (16 GB Blackwell OK); BF16 + BF16 CLIP ≈ 20 GB (needs 24 GB)
- **Model Downloads:** Exact links verified 2026-06-29; check HuggingFace if the repo changes
- **No Commercial Restrictions:** Apache-2.0; free for personal & commercial use (see licensing section below)
- **Workflow Reuse:** Feel free to modify, share, fork — the workflow itself is CC0 public domain

---

← [Back to all workflows](../README.md)

⭐ Found this useful? **Star the repo** · [Visit my Civitai page](https://civitai.red/models/2742740/z-image-turbo-qwenvl-dual-mode-auto-prompt)

## Model Attribution & Licensing

### Z-Image-Turbo (Tongyi-MAI / Alibaba)
- **License:** Apache 2.0 — https://huggingface.co/Tongyi-MAI/Z-Image-Turbo
- **Commercial use:** Permitted; derivative models OK
- **Attribution:** Appreciated (link to official repo)

### Qwen3-VL-2B-Instruct (Alibaba DAMO Academy)
- **License:** Apache 2.0 + Alibaba Qwen Community License
- **Usage:** Vision-based prompt enhancement
- **Commercial use:** OK under community license

### Qwen3-4B (Text Encoder)
- **License:** Apache 2.0
- **Commercial use:** Permitted

### Custom Nodes
- **ComfyUI-QwenVL** (AILab/1038lab): MIT / Apache 2.0
- **ComfyUI-Easy-Use** (vjumpkung): MIT License

### Workflow License
CC0 Public Domain. This JSON workflow is original work, free to use, modify, and redistribute without attribution (though credit is always appreciated).

---

**All example outputs are AI-generated.** Model weights remain the property of Tongyi-MAI (Alibaba); weights must be downloaded separately from official sources.
