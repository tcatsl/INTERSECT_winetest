---
title: NRPN MIDI routing
nav_order: 6
---

# MIDI controller routing (NRPN)

INTERSECT supports NRPN-based slice editing from a hardware or software MIDI controller. Enable it via the **SET** button in the header bar — the popup contains an NRPN section with channel and consume options.

**Controller requirements:** The controller must have **endless rotary encoders** (not fixed-range knobs) and must support NRPN mode with configurable MSB/LSB address values. Crucially, it must send **relative** data bytes — CC 96 (Data Increment) and CC 97 (Data Decrement) — one step per click. Controllers that only send absolute values (CC 6 Data Entry) will not work, nor will standard fixed-range knobs. Examples include the Akai MPD32 and MPD218. Results may vary.

Select the slice to edit by enabling **FM** (Follow MIDI) and playing its MIDI note. Then use the start/end knobs to adjust the slice boundaries. Commit happens automatically ~300ms after the knob stops moving — the same feel as releasing a parameter slider. Zoom in for finer control; each knob step moves `viewWidth / 16383` samples.

## NRPN parameter table

NRPN numbers use CC 99 (MSB address) / CC 98 (LSB address) to select the parameter, then CC 96 (Data Increment) or CC 97 (Data Decrement) to send a ±1 step. No absolute-value data bytes (CC 6/38) are used.

When programming a hardware controller, set the knob to **NRPN mode** with **MSB 64** and the LSB from the table below.

| NRPN | MSB | LSB | Name | Direction | Notes |
| --- | --- | --- | --- | --- | --- |
| 8193 | 64 | 1 | Zoom | CC 96 / CC 97 | Zoom in / out |
| 8194 | 64 | 2 | Slice start | CC 96 / CC 97 | Each step = viewWidth / 16383 samples |
| 8195 | 64 | 3 | Slice end | CC 96 / CC 97 | Each step = viewWidth / 16383 samples |

## SET button — NRPN settings

Open with the **SET** button in the header bar.

| Control | Function |
| --- | --- |
| `NRPN` | Enable/disable NRPN slice editing |
| `CONSUME CCs` | Strip NRPN edit CCs from MIDI output so they don't reach downstream instruments |
| `CH −` / `CH +` | MIDI channel filter (0 = omni) |

Settings are saved to the INTERSECT settings file alongside theme and UI scale.

## DAW-specific notes

**Ableton Live:** NRPN control does not work in Ableton Live. Ableton intercepts the CC 96/97 data-increment bytes before they reach the plugin, so the plugin only ever sees the address bytes (CC 98/99) and can never fire an event. This is an Ableton routing limitation with no known workaround on the plugin side.

**REAPER / Bitwig:** Route a MIDI track to the plugin instance as normal. No additional configuration is needed; all CC messages are forwarded to the plugin.
