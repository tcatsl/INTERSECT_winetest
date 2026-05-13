---
title: Interface
nav_order: 4
---

# Interface layout

## Header bar

| Area | Function | Notes |
| --- | --- | --- |
| Status text | Shows warnings, errors, and missing-file notices | Click warning/error text to copy the message |
| `UNDO` / `REDO` | History navigation | Same as `Ctrl/Cmd + Z` and `Ctrl/Cmd + Shift + Z` |
| `PANIC` | Kills active voices immediately | Also stops lazy chop |
| `FILES` | Toggle the built-in sample browser side panel | Visibility persists across sessions |
| `SET` | Popup for theme, UI scale, and NRPN settings | Also shows current plugin version |

## Sample lane, slice lane, and waveform

| Area | Function | Notes |
| --- | --- | --- |
| Sample lane | Compact session-sample overview above the slice lane | Reflects selection and zoom; drag to reorder samples; includes per-sample `STEMS` / `CANCEL` and delete buttons |
| Slice lane | Compact slice-region overview above the waveform | Reflects selection and zoom |
| Waveform | Main editing surface | Drag-and-drop loading/appending, slice selection, boundary editing, move/duplicate, preview |
| Overlay hints | Contextual help and action prompts | Used by `ADD`, `AUTO`, and other actions |
| Playback cursors | Voice-position display | Shows active playheads |
| Transient preview markers | Auto Chop preview | Dashed markers shown before applying transient split |

## Sample browser

Toggled by the `FILES` button in the header. The browser docks to the side of the editor and its visibility persists with your user settings.

| Area | Function | Notes |
| --- | --- | --- |
| `←` / `→` | Back / forward through visited folders | Disabled at ends of history |
| `↑` | Go up one directory | Same as pressing `Backspace` while the browser has focus |
| `↻` | Refresh the current directory listing | Re-reads the folder from disk |
| Path display | Click to edit the current path inline | Press `Return` to navigate |
| File list | Double-click a folder to enter it; double-click an audio file (or select files and press `Return`) to load | Multi-select supported when appending |
| Bookmarks | Pinned shortcuts to favorite folders | Right-click a folder row to **Add Bookmark**; right-click a bookmark entry to **Remove Bookmark** — bookmarks persist with user settings |
| Drag-and-drop | Drag selected files from the browser onto the waveform | Follows the same replace/append rule as double-click loads |

Loading behavior matches the rest of INTERSECT: into an empty session, the first load replaces and resets zoom/scroll; when a sample is already loaded, further loads append to the session.

## Stem separation

1. Click a sample's `STEMS` button in the sample lane.
2. Choose the model, output folder, device, and which stems to export.
3. Start the export from the overlay panel.

Notes:
- Stem separation runs on the selected session sample and writes the exported stems to the chosen folder.
- While a stem export is running, that sample's `STEMS` button changes to `CANCEL`.
- `DEVICE` defaults to CPU. GPU can be selected when the build and local runtime support it.
- If INTERSECT cannot use the selected GPU path, it warns before export so you can switch devices instead of silently exporting on CPU.
- Stem separation is compute-heavy. On older PCs, especially on CPU exports, startup and processing can be noticeably slower.
- macOS x64 (Intel Mac) builds do not include stem separation due to a platform limitation in the underlying inference runtime. The stem panel on these builds shows a "Not available" state.

See [Installation → Stem separation setup]({% link installation.md %}#stem-separation-setup) for runtime + model download steps.

## Time / zoom bar

| Area | Function | Notes |
| --- | --- | --- |
| Time ruler | Shows time markings for the current view | Updates with zoom level |
| Drag horizontally | Scroll | Uses the current zoom level |
| Drag vertically | Zoom | Anchored to the drag start position |

## Action bar

| Button | Function | Notes |
| --- | --- | --- |
| `ADD` | Toggle draw-slice mode | Drag on the waveform to create a slice |
| `LAZY` / `STOP` | Start/stop real-time lazy chopping | Label changes while active |
| `AUTO` | Open/close Auto Chop panel | Requires a selected slice |
| `COPY` | Duplicate selected slice | Equivalent to duplicate command |
| `DEL` | Delete selected slice or selected sample | Uses the last lane you interacted with |
| `ZX` | Snap edits to nearest zero crossing | Toggle |
| `FM` | Follow MIDI note selection | Auto-selects the played slice |
| `RESEQ` | Resequence slice MIDI notes | Opens overlay with `BY POSITION` and `AS CREATED` modes; requires 2+ slices |

## Signal chain bar

The bottom bar is the main parameter editor. It has four modules: `TIME/PITCH`, `FILTER`, `AMP`, and `PLAYBACK`.

**Collapsed mode** (default): `GLOBAL` and `SLICE` tabs switch between scopes, with one parameter strip visible at a time.

**Expanded mode**: shows both strips simultaneously — slice on top, global below — with no tabs. Click the chevron toggle on the right edge of the context bar to switch between modes.

**Context bar** (bottom edge):
- `SLICES` count and the global `ROOT` note are always visible on the right. The global `ROOT` is editable only when no slices exist.
- When a slice is selected: slice sample range, length, a `NOTE`/`RANGE` toggle, numeric note controls, read-only note names, and override count.

General behavior:
- Drag up/down on a value to edit it.
- Double-click a value to type it directly.
- In `SLICE` mode, editing a field locks that field for the selected slice when it differs from the global value.
- In `SLICE` mode, clicking a locked field label or right-clicking the field clears that override.

For each module's individual controls, see the [Controls and shortcuts reference]({% link controls-reference.md %}).
