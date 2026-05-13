---
title: Build from source
nav_order: 8
---

# Build from source

## Prerequisites

- CMake `3.22+`
- C++20 compiler
- Git

## Platform setup

Pick your platform below to install the required toolchain and libraries, then follow the shared **Clone and build** steps.

<details markdown="block">
<summary><strong>Windows</strong></summary>

1. Install [Visual Studio 2022](https://visualstudio.microsoft.com/vs/community/) (Community edition is free).
   During installation, select the **"Desktop development with C++"** workload.
2. Install [CMake](https://cmake.org/download/) (add to PATH during install) and [Git](https://git-scm.com/download/win).

</details>

<details markdown="block">
<summary><strong>macOS</strong></summary>

1. Install Xcode Command Line Tools:
   ```bash
   xcode-select --install
   ```
2. Install CMake via [Homebrew](https://brew.sh/):
   ```bash
   brew install cmake
   ```

</details>

<details markdown="block">
<summary><strong>Linux — Debian / Ubuntu</strong></summary>

```bash
sudo apt update
sudo apt install -y build-essential cmake git libasound2-dev libfreetype-dev \
  libx11-dev libxrandr-dev libxcursor-dev libxinerama-dev \
  libwebkit2gtk-4.1-dev libcurl4-openssl-dev
```

</details>

<details markdown="block">
<summary><strong>Linux — Fedora</strong></summary>

```bash
sudo dnf install -y gcc-c++ cmake git alsa-lib-devel freetype-devel \
  libX11-devel libXrandr-devel libXcursor-devel libXinerama-devel \
  webkit2gtk4.1-devel libcurl-devel
```

</details>

<details markdown="block">
<summary><strong>Linux — Arch</strong></summary>

```bash
sudo pacman -S --needed base-devel cmake git alsa-lib freetype2 \
  libx11 libxrandr libxcursor libxinerama webkit2gtk-4.1 curl
```

</details>

## Clone and build

```bash
git clone --recursive https://github.com/tucktuckg00se/INTERSECT.git
cd INTERSECT
cmake -B build
cmake --build build --config Release
```

## ONNX Runtime

The build does not bundle any ONNX Runtime shared library. Only the ORT C++ headers are pulled in at configure time (via CMake `FetchContent`) so the plugin compiles against the published ORT API. The shared library itself is downloaded at runtime via **SET → Stem Separation → ONNX Runtime** — see [Stem separation setup]({% link installation.md %}#stem-separation-setup). This keeps each plugin zip small and lets users pick their GPU (CPU / CUDA / MIGraphX / DirectML / CoreML) without needing a separate build.

## Build outputs

- VST3: `build/Intersect_artefacts/Release/VST3/INTERSECT.vst3`
- Standalone:
  - Windows: `build/Intersect_artefacts/Release/Standalone/INTERSECT.exe`
  - Linux: `build/Intersect_artefacts/Release/Standalone/INTERSECT`
  - macOS: `build/Intersect_artefacts/Release/Standalone/INTERSECT.app`
- AU (macOS): `build/Intersect_artefacts/Release/AU/INTERSECT.component`

## Release workflow (repo maintainers)

Pushing a tag matching `v*` triggers the GitHub Actions release workflow, which builds and packages four small plugin zips:

- Windows x64
- Linux x64
- macOS arm64
- macOS x64 (no stem separation — ONNX Runtime 1.24 dropped x86_64 macOS)

ONNX Runtime bundles are published separately from the [intersect-ort-providers](https://github.com/tucktuckg00se/intersect-ort-providers) repo and downloaded on demand from the plugin.

## Dependencies

- [JUCE](https://github.com/juce-framework/JUCE) (git submodule)
- [Signalsmith Stretch](https://github.com/Signalsmith-Audio/signalsmith-stretch) (MIT)
- [Signalsmith Linear](https://github.com/Signalsmith-Audio/linear) (dependency of Signalsmith Stretch)
- [Bungee](https://github.com/bungee-audio-stretch/bungee) (MPL-2.0)
- [ONNX Runtime](https://onnxruntime.ai/) (MIT) — headers only at build time; shared library downloaded on demand
