---
title: Installation
nav_order: 2
---

# Installation

Download the latest release zip from [Releases](https://github.com/tucktuckg00se/INTERSECT/releases), then place plugin files in your system plugin folders.

## Release package contents

| Platform | Minimum OS | Stem separation | Included binaries |
| --- | --- | --- | --- |
| Windows x64 | Windows 10 | On-demand ONNX Runtime bundle (CPU or DirectML GPU) | `INTERSECT.vst3`, `INTERSECT.exe` |
| Linux x64 | Ubuntu 22.04+ (glibc 2.35+) | On-demand ONNX Runtime bundle (CPU / NVIDIA CUDA / AMD MIGraphX) | `INTERSECT.vst3`, `INTERSECT` (standalone) |
| macOS arm64 | macOS 10.13 (High Sierra) | On-demand ONNX Runtime bundle (CoreML) | `INTERSECT.vst3`, `INTERSECT.component`, `INTERSECT.app` |
| macOS x64 | macOS 10.13 (High Sierra) | Not available | `INTERSECT.vst3`, `INTERSECT.component`, `INTERSECT.app` |

Plugin zips are small (single-digit MB); the ONNX Runtime required for stem separation is downloaded on demand from inside the plugin — see [Stem separation setup](#stem-separation-setup).

## Plugin install paths

| Format | Windows | macOS | Linux |
| --- | --- | --- | --- |
| VST3 | `C:\Program Files\Common Files\VST3\` | `~/Library/Audio/Plug-Ins/VST3/` | `~/.vst3/` |
| AU | n/a | `~/Library/Audio/Plug-Ins/Components/` | n/a |

After copying files, rescan plugins in your DAW.

## macOS — clear quarantine on first launch

INTERSECT release builds are unsigned, so macOS may report on first launch that INTERSECT is "damaged" or "cannot be opened". Clear the quarantine flags from a Terminal:

```bash
xattr -cr ~/Library/Audio/Plug-Ins/VST3/INTERSECT.vst3
xattr -cr ~/Library/Audio/Plug-Ins/Components/INTERSECT.component
xattr -cr /Applications/INTERSECT.app
```

Then re-launch your DAW (or the standalone app) and rescan plugins.

## Stem separation setup

Stem separation requires two on-demand downloads performed from inside the plugin. Both live under **SET → Stem Separation** in the header bar.

1. **ONNX Runtime bundle** — open **SET → Stem Separation → ONNX Runtime** and pick the bundle that matches your platform and (optionally) your GPU. Only one bundle is active at a time; installing a different one switches which runtime the plugin loads. Restart INTERSECT after the install completes so the new runtime is picked up.
2. **Stem model** — open **SET → Stem Separation → Download Models** and pick a BS-RoFormer model.

After both are installed, open a sample's `STEMS` button to export.

## GPU runtime requirements (per ONNX Runtime bundle)

- **Windows — DirectML** — no extra installs required. Works with any GPU vendor (NVIDIA, AMD, Intel) on Windows 10+ with a recent DirectX 12-capable driver.
- **macOS arm64 — CoreML** — no extra installs required. Uses the built-in Apple Neural Engine / GPU.
- **macOS x64 (Intel Mac)** — stem separation is not available. ONNX Runtime 1.24 dropped x86_64 macOS support.
- **Linux — NVIDIA CUDA** — requires:
  - NVIDIA proprietary driver
  - CUDA runtime 12.x or 13.x (`libcudart.so.12` or `libcudart.so.13`) — pick the CUDA 12 or CUDA 13 bundle accordingly
  - cuDNN 9 (`libcudnn.so.9`)

  Install on Arch: `sudo pacman -S cuda cudnn`
  Install on Ubuntu: `sudo apt install nvidia-cuda-toolkit libcudnn9-cuda-12` (adjust the cuDNN package name to match your CUDA version)

  Flatpak DAW hosts, such as the Flatpak build of Bitwig Studio, may need extra library-path configuration even when GPU access is enabled in Flatseal. If the standalone app can use CUDA but the Flatpak-hosted plugin reports `OrtSessionOptionsAppendExecutionProvider_Cuda: Failed to load shared library`, add these environment variables to the DAW in Flatseal:

  ```text
  LD_LIBRARY_PATH=/opt/cuda/lib64:/usr/lib/x86_64-linux-gnu/GL/lib
  LD_PRELOAD=/run/host/usr/lib/libcudnn.so.9
  ```

- **Linux — AMD MIGraphX** — requires ROCm installed. See [AMD's ROCm install guide](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/). Experimental.
- **Linux — CPU** — no extra installs required.

If a GPU runtime is missing or fails to load at export time, INTERSECT shows the error in the header status bar instead of silently failing; you can then switch `DEVICE` to CPU or install the missing runtime.
