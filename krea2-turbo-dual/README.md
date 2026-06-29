# Krea2 Turbo — QwenVL Dual-Mode Workflow

> Fast, high-quality image generation with optional AI-powered auto-prompting. Drop an image to get an AI description automatically, or type a subject keyword and let QwenVL expand it into a detailed visual prompt. Your choice—no rewiring needed, just a number.

[![VRAM](https://img.shields.io/badge/VRAM-~8%20GB-blue)]()
[![Civitai](https://img.shields.io/badge/Civitai-182%20downloads-orange)](https://civitai.com/user/TP_AI_63)
[![License](https://img.shields.io/badge/License-CC0%20Workflow%20%2B%20Krea--2%20Community-yellow)](https://www.krea.ai/krea-2-licensing)

## Overview

ComfyUI workflow for professional image generation using Krea2 Turbo fp8 diffusion model. Two modes: **Mode A** — type a subject, QwenVL expands it automatically into a rich, photorealistic prompt. **Mode B** — load a reference image, let QwenVL analyze and describe it, then generate inspired by its style and content. Tested on RTX 5080 16GB. Landscape (16:9) and portrait (9:16) aspect ratios built-in.

### Table of Contents

- [Two Modes](#two-modes)
- [Features](#features)
- [Required Models](#required-models)
- [Download Links](#download-links)
- [Required Custom Nodes](#required-custom-nodes)
- [How to Use](#how-to-use)
- [Settings & Parameters](#settings--parameters)
- [Quick Start Guide](#quick-start-guide)
- [Model Attribution](#model-attribution--licensing)

## Two Modes

Switch between modes by changing a single number widget. No rewiring, no bypass groups—clean and intuitive.

| Mode | Input | Process | Best For |
|------|-------|---------|----------|
| **🤖 Mode A** (keyword → prompt) | Text subject (e.g., "a lone lighthouse at golden hour") | QwenVL expands into detailed visual prompt | Creative brainstorming, fast iterations |
| **📸 Mode B** (image → description) | Reference image (PNG/JPEG) | QwenVL analyzes image → generates inspired output | Style transfer, mood matching, reference-driven |

**Switching:** Set **🔀 Mode Switch** widget index:
- `0` → Mode A (active by default)
- `1` → Mode B

Also includes **📐 Latent Switch** for aspect ratio:
- `0` → Landscape 1920×1088 (16:9)
- `1` → Portrait 1088×1920 (9:16)

## Features

- ✅ **Krea2 Turbo FP8** — Fast inference, reduced VRAM footprint (fp8 quantization)
- ✅ **QwenVL Dual-Mode** — Vision-directed or keyword-prompt, no rewiring
- ✅ **Dual Aspect Ratios** — Landscape 16:9 and Portrait 9:16, index switch
- ✅ **Tested on RTX 5080 16GB** — Reliable VRAM performance
- ✅ **Qwen3-VL Auto-Download** — Vision model loads on first use (~3 GB, local)
- ✅ **Professional Negative Prompt** — Curated quality filters (blurry, low-quality, watermark, deformed)
- ✅ **Stable Sampler Config** — Euler sampler, cfg 1.0, 10 steps, simple scheduler

## Required Models

| Model File | Size | Category | Location | Source |
|---|---|---|---|---|
| `krea2_turbo_fp8.safetensors` | ~6–7 GB | UNet (main diffusion) | `ComfyUI/models/unet/` | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) |
| `qwen3vl_4b_fp8_scaled.safetensors` | ~2–3 GB | CLIP (text/vision encoder) | `ComfyUI/models/clip/` | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) |
| `qwen_image_vae.safetensors` | ~1 GB | VAE (encode/decode) | `ComfyUI/models/vae/` | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) |
| `Qwen3-VL-2B-Instruct` | ~2.5 GB | VLM (auto-download) | Auto-downloads on first use | Qwen/Qwen3-VL-2B-Instruct |

**Total disk space:** ~11–15 GB (Krea2 weights + Qwen3-VL downloads)

## Download Links

### Krea2 Model Files

All models hosted on **[AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8)** (single source):

| File | Purpose | HF Link |
|------|---------|--------|
| `krea2_turbo_fp8.safetensors` | UNet diffusion model | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) → `krea2_turbo_fp8.safetensors` |
| `qwen3vl_4b_fp8_scaled.safetensors` | CLIP (text + vision) | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) → `qwen3vl_4b_fp8_scaled.safetensors` |
| `qwen_image_vae.safetensors` | VAE codec | [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8) → `qwen_image_vae.safetensors` |

### QwenVL Vision Model (Auto-Download)

**`Qwen3-VL-2B-Instruct`** (~2.5 GB) — Auto-downloads on first QwenVL node execution
- No manual download needed; ComfyUI-QwenVL handles it
- Cached locally in `~/.cache/` after first download
- Source: [Qwen/Qwen3-VL-2B-Instruct](https://huggingface.co/Qwen/Qwen3-VL-2B-Instruct)

### Placement

```
ComfyUI/
├── models/
│   ├── unet/
│   │   └── krea2_turbo_fp8.safetensors
│   ├── clip/
│   │   └── qwen3vl_4b_fp8_scaled.safetensors
│   └── vae/
│       └── qwen_image_vae.safetensors
```

## Required Custom Nodes

Install these in your ComfyUI `custom_nodes/` folder:

| Custom Node | Purpose | Install |
|---|---|---|
| **AILab_QwenVL_Advanced** | Vision analysis (Mode B image-to-prompt) | [1038lab/ComfyUI-QwenVL](https://github.com/1038lab/ComfyUI-QwenVL) |
| **AILab_QwenVL_PromptEnhancer** | Keyword expansion (Mode A) | Same as above |
| **easy anythingIndexSwitch** | Mode and aspect-ratio switcher | [vjumpkung/ComfyUI-Easy-Use](https://github.com/vjumpkung/ComfyUI-Easy-Use) |
| **comfy-core** (UNETLoader, CLIPLoader, VAELoader, etc.) | Standard ComfyUI nodes | Built-in; no install needed |

### Quick Install

```bash
cd ComfyUI/custom_nodes

# QwenVL nodes
git clone https://github.com/1038lab/ComfyUI-QwenVL

# Easy-Use (includes IndexSwitch)
git clone https://github.com/vjumpkung/ComfyUI-Easy-Use
```

Or via **ComfyUI Manager**:
- Search: `ComfyUI-QwenVL` → Install
- Search: `ComfyUI-Easy-Use` → Install

## How to Use

### Mode A: Keyword → Auto-Expanded Prompt

1. Load workflow into ComfyUI
2. Ensure **🔀 Mode Switch** [index] = `0` (default, Mode A active)
3. In **✍️ QwenVL Prompt Enhancer** node, edit `[prompt_text]` field:
   - Type a subject: `"a lone lighthouse at golden hour overlooking the sea"`
   - QwenVL will expand it automatically into a detailed prompt
4. Set **📐 Latent Switch** [index]:
   - `0` → Landscape 1920×1088 (16:9)
   - `1` → Portrait 1088×1920 (9:16)
5. Click **Queue Prompt**
6. Image saves to `ComfyUI/output/krea2_turbo/output/`

**Generation time (RTX 5080):** ~30–60 seconds

### Mode B: Reference Image → AI Description

1. Load workflow
2. Set **🔀 Mode Switch** [index] = `1` (Mode B active)
3. In **📸 Load Reference Image** node, select your reference image from the file picker
   - Any PNG, JPEG, or WebP
   - Any aspect ratio (workflow handles it)
4. QwenVL analyzes the image → generates inspired output
5. Set **📐 Latent Switch** as before
6. Click **Queue Prompt**
7. Output image saves to `ComfyUI/output/krea2_turbo/output/`

**Generation time (RTX 5080):** ~40–70 seconds (includes image analysis overhead)

## Settings & Parameters

| Parameter | Current Value | Notes |
|-----------|--------|-------|
| **Sampler** | euler | Standard diffusion sampler |
| **Scheduler** | simple | Simple noise scheduler |
| **Steps** | 10 | Total diffusion steps |
| **CFG Scale** | 1.0 | Classifier-free guidance (1.0 = no guidance, stable for Krea2 Turbo) |
| **Seed** | randomize | Auto-randomize on each queue |
| **Negative Prompt** | (curated list) | See below |
| **Output Folder** | `krea2_turbo/output` | Relative to ComfyUI root |

### Negative Prompt (Built-in)

```
blurry, low quality, watermark, text overlay, logo, ugly, deformed, noise, grain, 
oversaturated, flat lighting, duplicate
```

Tuned for high-quality, professional output. Edit in **❌ NEGATIVE** node if needed.

### Adjustable Widgets

- **🔀 Mode Switch** [index]:
  - `0` → Mode A (keyword expansion)
  - `1` → Mode B (image reference)

- **📐 Latent Switch** [index]:
  - `0` → Landscape 1920×1088 (16:9)
  - `1` → Portrait 1088×1920 (9:16)

- **Seed** (in ⚡ KSampler):
  - `randomize` → new seed each queue
  - Set to a number (e.g., `42`) → reproducible results

- **Steps** (in ⚡ KSampler):
  - Default: 10 (fast, good quality)
  - Increase to 15–20 for higher quality (slower)

## Quick Start Guide

```
1. Place krea2_turbo_fp8.safetensors (+ CLIP + VAE) in ComfyUI/models/
2. Install custom nodes: ComfyUI-QwenVL, ComfyUI-Easy-Use
3. Load workflow.json into ComfyUI
4. Set 🔀 Mode Switch [index] = 0 or 1
5. Type subject or load reference image
6. Queue Prompt
7. Output in ComfyUI/output/krea2_turbo/output/
```

**First run:** QwenVL downloads Qwen3-VL-2B-Instruct (~2.5 GB) → ~1–2 minutes wait on first execution.

## Performance Tips

- **Cold start:** First queue takes longer due to model loading. Subsequent queues are faster.
- **Keyword inspiration:** Start vague ("lighthouse"), let QwenVL expand it. Edit the generated prompt in the text node if needed.
- **Reference images:** Sharp, well-composed reference images yield better style transfer.
- **Batch generation:** Queue multiple times for variations (seed auto-randomizes).
- **VRAM:** ~8 GB peak usage; safe on RTX 5080 16GB alongside other apps.

## Notes & Licensing

- **Model source:** Krea2 Turbo weights from [AlperKTS/Krea2_FP8](https://huggingface.co/AlperKTS/Krea2_FP8)
- **Hardware tested:** RTX 5080 16GB VRAM; CUDA compute 9.2+
- **Output license:** You own all outputs. Commercial use OK if revenue < $1M/yr (Krea-2 Community License). See https://www.krea.ai/krea-2-licensing
- **Workflow:** CC0 (public domain)—free to use, modify, share

---

← [Back to all workflows](../README.md)

⭐ Found this useful? **Star the repo** · [Visit my Civitai page](https://civitai.com/user/TP_AI_63)

## Model Attribution & Licensing

### Krea2 Turbo (AlperKTS FP8 Quant)
- **License:** [Krea-2 Community License](https://www.krea.ai/krea-2-licensing)
- **Commercial use:** ✅ Allowed if total annual revenue < $1,000,000 USD. Enterprise license required above this threshold.
- **Architecture:** Original DiT 12.9B (NOT FLUX-derived) — Krea's own foundation model
- **Verified:** 2026-06-29
- **Attribution required:** Credit Krea-2 and AlperKTS when distributing
- **Download:** https://huggingface.co/AlperKTS/Krea2_FP8 (community FP8 quantization, permitted as Derivative)

### QwenVL Text Encoder + Image VAE
- **Files:** `qwen3vl_4b_fp8_scaled.safetensors` (Qwen3-VL-4B) + `qwen_image_vae.safetensors`
- **License:** Apache 2.0 ✅ (Alibaba Qwen team)
- **Attribution:** Include in workflow or output metadata

### QwenVL (Alibaba / Qwen)
- **License:** Apache 2.0 (Qwen models)
- **Attribution:** Include in workflow or output metadata
- **Download:** Auto-downloads via ComfyUI-QwenVL
- **Model:** Qwen3-VL-2B-Instruct (2.5 GB)

### Custom Nodes
- **ComfyUI-QwenVL** (1038lab): Apache 2.0 / BSD
- **ComfyUI-Easy-Use** (vjumpkung): Per upstream license

### Workflow JSON
- **License:** CC0 (Public Domain) — free to use, modify, redistribute
- **Attribution:** Optional but appreciated

---

**All generated images are AI-created.** Refer to Krea2 and Qwen licensing for commercial use terms. This workflow configuration is original work (CC0); model weights are third-party and covered by their respective licenses.
