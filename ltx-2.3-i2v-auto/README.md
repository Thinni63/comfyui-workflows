# LTX-2.3 Image-to-Video Workflow — QwenVL Auto-Prompt, No Drift

> Drop an image, get professional motion. QwenVL vision model automatically analyzes your input and generates a motion-aware prompt—no manual description needed. Locked static camera, zero drift, broadcast-quality output.

[![VRAM](https://img.shields.io/badge/VRAM-~14%20GB-blue)]()
[![Civitai](https://img.shields.io/badge/Civitai-156%20downloads-orange)](https://civitai.com/user/TP_AI_63)
[![License](https://img.shields.io/badge/License-LTX--2%20Community%20%2B%20Gemma-yellow)]()

## Overview

Pure LTX 2.3 22B image-to-video pipeline for ComfyUI. Drop an image, get professional motion. QwenVL vision model automatically analyzes your input image and generates a motion-aware prompt—no manual description needed. The workflow enforces locked static camera (anti-drift), scales dynamically to any input resolution, and upscales output to broadcast-quality 1920×1088 at 24 FPS. Production-ready for stock footage, ambient loops, and commercial video generation.

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

- **QwenVL Auto Motion Director** — Vision model reads input image → auto-generates motion prompt (camera lock, object tracking hints)
- **Locked Static Camera** — Zero pan, zoom, or drift; all motion in-frame only
- **Pure LTX 2.3 22B** — No LoRA needed; GGUF quantization for 16GB VRAM
- **Dynamic Pixel Scaling** — Auto-scales any input size to optimal 0.52MP for 8-step inference
- **Dual-Stage Upscale** — 960×544 base → 2× spatial upscaler → 1920×1088 output
- **Audio + Video VAE** — Multi-modal encoding; ready for synced audio pipelines
- **24 FPS Native** — Smooth playback; 168 frames per generation
- **5-Second Clips** — 121 frames @ 24fps per generation (stock-ready duration)

## Required Models

All models below are freely downloadable from HuggingFace. File sizes and placement paths are exact.

| Model File | Size | Category | Location |
|---|---|---|---|
| `ltx-2.3-22b-distilled-Q4_K_M.gguf` | 17.8 GB | UNet (main diffusion) | `ComfyUI/models/unet/` |
| `gemma_3_12B_it_fp4_mixed.safetensors` | 9.45 GB | Text Encoder | `ComfyUI/models/text_encoders/` |
| `ltx-2.3_text_projection_bf16.safetensors` | 2.31 GB | Text-to-Latent Projection | `ComfyUI/models/text_encoders/` |
| `LTX23_video_vae_bf16.safetensors` | 1.45 GB | Video VAE (encode/decode) | `ComfyUI/models/vae/` |
| `LTX23_audio_vae_bf16.safetensors` | 365 MB | Audio VAE (dual-modal) | `ComfyUI/models/vae/` |
| `ltx-2.3-spatial-upscaler-x2-1.1.safetensors` | 996 MB | 2× Upscaler | `ComfyUI/models/upscale_models/` |

**Total disk space:** ~32 GB (compressed on disk, decompressed in VRAM)

## Download Links (verified HuggingFace sources)

### UNet
- **`ltx-2.3-22b-distilled-Q4_K_M.gguf`** (GGUF quantized, Q4_K_M compression)  
  → [QuantStack/LTX-2.3-GGUF](https://huggingface.co/QuantStack/LTX-2.3-GGUF)  
  → Place in: `ComfyUI/models/unet/`

### Text Encoders
- **`gemma_3_12B_it_fp4_mixed.safetensors`** (fp4 mixed precision)  
  → [Comfy-Org/ltx-2](https://huggingface.co/Comfy-Org/ltx-2)  
  → Place in: `ComfyUI/models/text_encoders/`

- **`ltx-2.3_text_projection_bf16.safetensors`** (text-to-latent bridge)  
  → [Kijai/LTX2.3_comfy](https://huggingface.co/Kijai/LTX2.3_comfy)  
  → Place in: `ComfyUI/models/text_encoders/`

### VAE (Video & Audio Codecs)
- **`LTX23_video_vae_bf16.safetensors`** (video encode/decode)  
  → [Kijai/LTX2.3_comfy](https://huggingface.co/Kijai/LTX2.3_comfy)  
  → Place in: `ComfyUI/models/vae/`

- **`LTX23_audio_vae_bf16.safetensors`** (audio codec, dual-modal support)  
  → [Kijai/LTX2.3_comfy](https://huggingface.co/Kijai/LTX2.3_comfy)  
  → Place in: `ComfyUI/models/vae/`

### Upscaler
- **`ltx-2.3-spatial-upscaler-x2-1.1.safetensors`** (2× spatial upscale, final quality pass)  
  → [Lightricks/LTX-2.3](https://huggingface.co/Lightricks/LTX-2.3)  
  → Place in: `ComfyUI/models/upscale_models/`

> **Note:** VAE files are NOT in the official Lightricks repo — get them from Kijai/LTX2.3_comfy. The Gemma fp4 encoder is hosted by Comfy-Org. All filenames use v1.1 (distilled-1.1, upscaler x2-1.1) — the current stable hotfix release.

## Required Custom Nodes

Install all of these in your ComfyUI `custom_nodes/` folder:

| Custom Node | Purpose | Repository |
|---|---|---|
| **LTXV** | LTX-Video sampling, encoding, projection | [Lightricks](https://github.com/Lightricks/ComfyUI-LTXV) |
| **AILab_QwenVL_Advanced** | QwenVL vision model (image-to-text) | [1038lab/ComfyUI-QwenVL](https://github.com/1038lab/ComfyUI-QwenVL) |
| **ComfyUI-GGUF** | UnetLoaderGGUF for quantized models | [ComfyUI GGUF support](https://github.com/city96/ComfyUI-GGUF) |
| **VideoHelperSuite** | VHS_VideoCombine, frame batching, MP4 export | [Kosinkadink/ComfyUI-VideoHelperSuite](https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite) |
| **rgthree-comfy** | Fast Groups Bypasser (workflow toggles) | [rgthree/ComfyUI-rgthree-comfy](https://github.com/rgthree/ComfyUI-rgthree-comfy) |
| **ImageIterator** | Batch image loader | [Kosinkadink/ComfyUI-ImageIterator](https://github.com/Kosinkadink/ComfyUI-ImageIterator) |
| **ImageScaleToTotalPixels** | Dynamic resolution scaling | [Kosinkadink/ComfyUI-ImageScaleToTotalPixels](https://github.com/Kosinkadink/ComfyUI-ImageScaleToTotalPixels) |
| **GetImageSize+** | Image dimension detection | [Custom node (see notes)](#notes) |

### Quick Install (via Manager)

If you use ComfyUI Manager, search and install:
- `LTXV`
- `ComfyUI-QwenVL` (AILab)
- `ComfyUI-GGUF`
- `ComfyUI-VideoHelperSuite`
- `rgthree-comfy`

Manual install (git clone into `custom_nodes/`):
```bash
cd ComfyUI/custom_nodes

git clone https://github.com/Lightricks/ComfyUI-LTXV
git clone https://github.com/1038lab/ComfyUI-QwenVL
git clone https://github.com/city96/ComfyUI-GGUF
git clone https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite
git clone https://github.com/rgthree/ComfyUI-rgthree-comfy
git clone https://github.com/Kosinkadink/ComfyUI-ImageIterator
git clone https://github.com/Kosinkadink/ComfyUI-ImageScaleToTotalPixels
```

## How to Use

### Step 1: Prepare Input

Place your input image(s) in the ComfyUI `./input/` directory:
- JPEG, PNG, or WebP
- Any resolution (workflow auto-scales to 0.52MP)
- Portrait or landscape (both supported)

### Step 2: Load Workflow

1. Open ComfyUI in your browser
2. Click **Load** (top menu)
3. Select `workflow.json` from this folder
4. All nodes auto-populate

### Step 3: Review QwenVL Motion Prompt (Optional)

The workflow includes a preview node showing the auto-generated motion prompt. Review it before queuing. If you want to edit it:
1. Right-click the **Motion Director** text output node
2. Select **Convert to input**
3. Edit the text directly
4. Reconnect the link if needed

### Step 4: Queue & Generate

Click **Queue Prompt** (right side of ComfyUI). Output video saves automatically to `./output/LTX23_I2V/`.

**Generation time (RTX 5080):**
- QwenVL: ~10s
- LTX Stage 1 (960×544, 8 steps): ~2–3 min
- LTX Stage 2 (1920×1088, 3 steps): ~1–2 min
- **Total: ~4–6 minutes per 5-second clip**

### Step 5: Review Output

MP4 file: `LTX23_I2V/<date>/ltx23_stock_i2v_<date>.mp4`  
Resolution: 1920×1080 (cropped from 1088)  
Duration: 5.04 seconds @ 24 FPS (121 frames)

## Settings & Parameters

| Parameter | Value | Notes |
|-----------|-------|-------|
| **FPS** | 24 | Standard frame rate; 168 total frames per generation |
| **Pixel Budget (Stage 1)** | 0.52 MP | Optimal for 8-step sampling on 16GB VRAM |
| **Output Resolution** | 1920×1088 | After 2× spatial upscale; cropped to 1920×1080 for MP4 |
| **Sampler (Stage 1)** | euler_ancestral_cfg_pp | Low-drift SDE solver |
| **Sampler (Stage 2)** | euler_cfg_pp | Refinement sampler |
| **Base Steps (Stage 1)** | 8 | Main diffusion sampling passes |
| **Refine Steps (Stage 2)** | 3 | Quality refinement after upscale |
| **CFG Scale (both)** | 1.0 | Classifier-free guidance (1.0 = no guidance, stable output for distilled model) |
| **Batch Size** | 1 image → 1 video | Sequential processing (queue multiple times for batch) |

### Custom Adjustments

- **Motion intensity:** Edit CFG scale (1.0 default). Increase to 2.0 for stronger motion adherence (adds ~2× time)
- **Upscale speed:** Disable the Spatial Upscaler bypass group to skip final upscale (output stays 960×544)
- **Audio sync:** Toggle the 🔊 audio bypass group. OFF (default) = silent video; ON = empty audio track (provide audio separately to VHS)

## Performance Tips

- **Batch multiple images:** Queue 5–10 images in one session to amortize model load time and reduce cold starts
- **Input image quality:** Sharp, well-lit images yield sharper motion; low-contrast images may produce soft motion
- **Motion prompt tuning:** Edit the QwenVL text output before queuing if you want specific motion direction (e.g., "slow pan left" → remove pan keywords to enforce static camera)
- **Upscale disabled?** The dual-stage upscale adds ~20–30 seconds. Bypass if speed is critical (output at 960×544 instead)
- **Audio VAE:** Workflow supports video+audio VAE. Provide audio separately (WAV, MP3) to VHS for lip-sync pipelines
- **VRAM spikes:** If OOM during VAEDecodeTiled, reduce tile size from 768 → 512 (or lower)

## Notes & Disclosure

- **AI-Generated Content:** All example outputs are AI-generated by LTX 2.3. Suitable for stock footage, ambient loops, and creative projects. Disclose AI generation per local regulations
- **Model Downloads:** See "Download Links" section above for exact HuggingFace repos and target folders
- **Hardware Tested:** RTX 5080 16GB VRAM; CUDA compute 9.2+
- **VRAM Usage:** ~14 GB peak during Stage 1 sampling; requires fast SSD for frame buffering
- **No Commercial Guarantees:** Use at your own discretion. Respect local AI disclosure laws and model licensing terms when publishing outputs
- **GetImageSize+ source:** This node may require manual installation from a custom-nodes repo or be part of another utility suite

---

← [Back to all workflows](../README.md)

⭐ Found this useful? **Star the repo** · [Visit my Civitai page](https://civitai.com/user/TP_AI_63)

## Model Attribution & Licensing

### LTX-Video 2.3 (Lightricks)
- **License:** LTX-2 Community License — https://huggingface.co/Lightricks/LTX-2.3/blob/main/LICENSE
- **Commercial use:** Free for entities under $10M USD annual revenue
- **AI-generated content disclosure:** Required when publishing outputs
- **Download:** https://huggingface.co/Lightricks/LTX-2.3

### Gemma 3 12B IT (Google DeepMind)
- **License:** Gemma Terms of Use — https://ai.google.dev/gemma/terms
- **Terms:** "Gemma is provided under and subject to the Gemma Terms of Use found at ai.google.dev/gemma/terms"
- **Restrictions:** Subject to Google's Prohibited Use Policy
- **Download:** https://huggingface.co/google/gemma-3-12b-it

### QwenVL (1038lab / Alibaba)
- **License:** Apache 2.0 (via Qwen models)
- **Attribution:** Include attribution in workflow or output metadata
- **Download:** Auto-downloads on first use (~2.5 GB)

### Custom Nodes
- **LTXV** (Lightricks): Per LTX-2 Community License
- **VideoHelperSuite** (Kosinkadink): MIT License
- **AILab QwenVL** (1038lab): Apache 2.0 / BSD
- **rgthree-comfy** (rgthree): MIT License
- **ComfyUI-GGUF** (city96): Per upstream license

---

**All example outputs are AI-generated.** This workflow (JSON configuration) is shared as original work; model weights must be downloaded separately from official sources per their respective terms.
