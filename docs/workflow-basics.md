---
title: Workflow basics
nav_order: 3
---

# Workflow basics

1. **Multi-sample session:** INTERSECT can load and concatenate multiple audio files per instance (`.wav`, `.ogg`, `.aiff`, `.flac`, `.mp3`). The `FILES` button in the header toggles a built-in sample browser side panel; loading from the browser (or drag-and-drop) into an empty session replaces it, and loading while a sample is already loaded appends.
2. **Current editor layout:** header bar, sample lane, slice lane, waveform, time/zoom bar, action bar, and bottom signal-chain editor.
3. **Slice creation:** draw slices manually, chop live with **LAZY**, or split a selected slice via **AUTO**.
4. **Inheritance model:** `GLOBAL` in the Signal Chain edits sample defaults. `SLICE` edits the selected slice and locks fields that diverge from the global value.
5. **Playback model:** MIDI triggers slices by note mapping. A slice belongs to one loaded sample, but playback and editing happen on the concatenated session timeline. A slice can respond to one note or a `LOW`-to-`HIGH` range, with `ROOT` defining the transposition center for that slice. Mute groups can choke voices in the same group.
6. **Algorithms:**
   - `Repitch`: pitch and speed are linked. `MODE` switches the playback interpolation between `Linear` and `Cubic`.
   - `Signalsmith`: independent time/pitch via Signalsmith Stretch (`TONAL`, `FMNT`, `FMNT C`).
   - `Bungee`: granular stretch mode with `GRAIN` choices (`Fast`, `Normal`, `Smooth`).
7. **Repitch + Stretch interaction:** when `ALGO=Repitch` and `STRETCH=ON`, `PITCH` and `TUNE` become BPM-driven read-only displays.
8. **SET BPM:** available in the Time/Pitch module for both `GLOBAL` and `SLICE`; it calculates BPM from musical duration.
9. **Filter model:** the filter is per-voice and resolves its settings at note-on. Cutoff changes affect newly triggered notes immediately, but do not retarget voices that are already playing.
10. **Filter envelope amount:** `AMT` is measured in semitones, so `+12 st` means the envelope can push cutoff up by one octave and `-12 st` means one octave down. This stays consistent across low and high base cutoff values.
11. **Key tracking:** `KEY` is a percentage of note tracking. `0%` ignores note pitch, `100%` makes cutoff follow pitch at full keyboard scaling, and intermediate values blend between them. In slice range mode, tracking follows the slice `ROOT` note.
12. **Drive:** `DRIVE` is pre-filter saturation. It adds harmonics before the filter rather than simply turning the signal up.
13. **Drive asymmetry:** `ASYM` biases the drive waveshaper to produce even-harmonic saturation, adding a warmer, tube-like character. A DC blocker engages automatically when asymmetry is above zero.
14. **Loop crossfade:** `FADE` smooths loop and ping-pong seams with equal-power crossfading. It is active only when `LOOP` is not `OFF`.
15. **Load behavior:** file decoding/loading is asynchronous (off the audio thread).
16. **Undo/redo:** snapshot-based history for slice and parameter edits.
17. **MIDI host stop handling:** responds to `All Notes Off (CC 123)` and `All Sound Off (CC 120)`.
