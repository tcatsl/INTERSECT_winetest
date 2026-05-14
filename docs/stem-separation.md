---
title: Stem separation
nav_order: 6
description: "End-to-end walkthrough for splitting a sample into stems with INTERSECT's BS-RoFormer pipeline."
---

# Stem separation

INTERSECT separates a sample into individual stems (drums, bass, vocals, guitar, piano, other) using the BS-RoFormer model running through ONNX Runtime. The separation runs locally — no audio leaves your machine.

This page is a walkthrough. For runtime setup and GPU requirements, see [Installation → Stem separation setup]({{ site.baseurl }}{% link installation.md %}#stem-separation-setup).

## Prerequisites

Before you start, you need:

1. **An ONNX Runtime bundle.** Open **SET → Stem Separation → ONNX Runtime** and pick the bundle matching your platform. Restart INTERSECT after the bundle installs.
2. **At least one stem model.** Open **SET → Stem Separation → Download Models** and pick a BS-RoFormer model. The download progresses in the background; the menu shows installed/unavailable status next to each model.
3. **A loaded sample.** Drag any audio file onto INTERSECT's waveform.

If either step is missing, the STEMS button's panel surfaces the missing piece and the START button stays disabled.

## Step 1 — Open the stem panel

In the sample lane (the strip directly above the slice lane), each sample has its own `STEMS` button. Click it to open the stem-separation panel.

While a separation is running on that sample, the button reads `CANCEL`.

## Step 2 — Choose a model

Click the `MODEL` cell to cycle through installed models. Today the only supported family is **BS-RoFormer 6-stem**, which outputs:

| Output | Source |
| --- | --- |
| drums | model output |
| bass | model output |
| other | model output |
| vocals | model output |
| guitar | model output |
| piano | model output |
| instrumental | residual (the original minus the vocal stem), computed automatically |

You don't have to export all of them — see Step 3.

## Step 3 — Pick which stems to keep

Below the option cells is a row of stem-name toggles. Click each one to enable or disable it. The `START` button is disabled until you've selected at least one.

If you only need vocals + instrumental for a remix, enable just those two and let the others stay off — the model still runs the full inference but the export step skips disabled stems, which is faster on disk and easier to manage.

## Step 4 — Choose a compute device

Click the `DEVICE` cell to toggle between `CPU` and `GPU`. The GPU option only appears if a GPU-capable ONNX Runtime bundle is installed and your hardware supports it (CUDA / MIGraphX / DirectML / CoreML depending on platform).

Rule of thumb:

- **GPU** if available. Separation is dramatically faster — often 10–30× over CPU.
- **CPU** for short clips, for laptops on battery, or when GPU memory is constrained.

If you pick a GPU device that can't load at export time, INTERSECT shows the error in the header status bar instead of silently falling back. You can switch to CPU and re-START.

## Step 5 — Choose how the stems land in your session

Click the `MODE` cell to switch between:

- `combine` — the exported stems are added to your INTERSECT session as new samples, alongside the parent sample. The new samples carry stem-role metadata (drums, bass, etc.) so the editor can show them as children of the parent.
- `separate` — the stems are written to disk as individual WAV files. Use this when you want the stems in your DAW or another tool, not inside INTERSECT.

## Step 6 — Pick the output folder (separate mode only)

Click the `OUTPUT` cell to toggle between "Beside sample" (writes next to the original file) and a custom folder. The `...` button opens a folder picker.

## Step 7 — START and watch progress

Press `START`. The job runs in the background; you can keep playing slices, edit parameters, and load other samples while it runs.

The header status bar shows the current stage:

1. **preparing** — copying audio + loading the model
2. **separating** — running BS-RoFormer inference (this is the long stage)
3. **writing** — encoding the chosen stems to WAV
4. **importing** — only in `combine` mode; adding the new stems to the session

When the job completes successfully, the panel closes (or you can dismiss it manually).

## Cancelling

Press `CANCEL` on the panel — or the `CANCEL` button that replaces `STEMS` on the sample lane — to stop the job. Any already-written files in `separate` mode are kept on disk; nothing is added to the session in `combine` mode.

## Working with the exported stems

In `combine` mode, the stems appear in the sample lane next to the parent. INTERSECT tags each exported stem with its role (drums, bass, vocals, etc.) so you can tell which is which at a glance.

The parent → stem relationship is metadata only; the stems behave like any other loaded sample. You can slice them, lock per-slice parameters, route them to separate output buses, and so on.

## Limitations & known issues

- **macOS Intel x64** does not support stem separation. ONNX Runtime 1.24 dropped x86_64 macOS, so Intel Macs see a "Not available" message in the panel.
- **Older PCs on CPU** can take several minutes per minute of audio. This is expected.
- **Large samples + GPU** may run out of VRAM on entry-level cards. Switch to CPU if you see a GPU memory error in the header status bar.

For more specific error messages, see [Troubleshooting → Stem separation]({{ site.baseurl }}{% link troubleshooting.md %}#stem-separation).
