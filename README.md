<div align="center">

# ComfyUI Workflows

**Production-ready ComfyUI pipelines — all tested on RTX 5080 16 GB**

[![ComfyUI](https://img.shields.io/badge/ComfyUI-compatible-blue)](https://github.com/comfyanonymous/ComfyUI)
[![VRAM](https://img.shields.io/badge/VRAM-16%20GB%20tested-green)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Civitai](https://img.shields.io/badge/Civitai-TP__AI__63-orange)](https://civitai.com/user/TP_AI_63)

</div>

## About

I share ComfyUI workflows I actually run daily for stock footage and image generation. Every pipeline is tested on RTX 5080 16GB, ships with exact HuggingFace download links, and requires zero placeholder steps.

**Signature feature:** QwenVL auto-prompting — drop an image or type a keyword, and a vision model reads it and generates a motion-aware or detailed prompt automatically. No manual description needed.

## Workflows

| | Workflow | Type | VRAM | Civitai |
|---|---|---|---|---|
| 🎬 | [LTX-2.3 Image-to-Video — QwenVL Auto-Prompt, No Drift](./ltx-2.3-i2v-auto/) | Video | ~14 GB | [156 downloads](https://civitai.com/user/TP_AI_63) |
| 🖼️ | [Krea2 Turbo — QwenVL Dual-Mode](./krea2-turbo-dual/) | Image | ~8 GB | [182 downloads](https://civitai.com/user/TP_AI_63) |
| 🖼️ | [Z-Image-Turbo — QwenVL Dual-Mode Auto-Prompt](./zimage-turbo/) | Image | ~13 GB | [View listing](https://civitai.red/models/2742740/z-image-turbo-qwenvl-dual-mode-auto-prompt) |
| ⬆️ | [SeedVR2 Batch Upscaler — Sleep On It, Wake Up 4K](./seedvr2-batch-upscale/) | Upscale | ~16 GB | [View listing](https://civitai.red/models/2750373/seedvr2-batch-upscaler-sleep-on-it-wake-up-4k?modelVersionId=3094090) |

## Quick Start

1. **Clone this repo:**
   ```bash
   git clone https://github.com/Thinni63/comfyui-workflows.git
   cd comfyui-workflows
   ```

2. **Load a workflow into ComfyUI:**
   - Drag `workflow.json` directly into the ComfyUI browser canvas, OR
   - ComfyUI menu → **Load** → select `workflow.json`

3. **Download required models** (see each workflow's README for exact links and target folders)

4. **Install custom nodes** (listed in each workflow's README)

5. **Load workflow → Queue → Done**

## Requirements

- **ComfyUI** (latest)
- **RTX 5080 16 GB** (or equivalent ≥14 GB VRAM)
- **Python 3.11+**
- **Custom nodes:** See each workflow's README for the full list

## Usage Tips

- Each workflow has detailed setup instructions in its own `README.md`
- Example outputs and preview images are in `examples/` folders (coming soon)
- Settings like CFG scale, sampler, and step counts are labeled in the ComfyUI UI
- For troubleshooting, check the Notes section in each workflow's README

## Connect

- **Civitai:** [TP_AI_63](https://civitai.com/user/TP_AI_63) — Download workflows + see all my work
- **GitHub:** [@Thinni63](https://github.com/Thinni63)

New workflows most weeks. Star this repo to stay updated.

## License

All workflow JSON files are licensed under **MIT** (see [LICENSE](./LICENSE)). Base models (Krea2, LTX-2.3, etc.) are licensed separately—refer to each workflow's README for model attribution and terms.

---

Generated for production use. Test locally first.
