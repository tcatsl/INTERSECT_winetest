---
title: Changelog
nav_order: 13
description: "Release history for INTERSECT — what's new in each version."
---

# Changelog

The full release history is maintained in [`CHANGELOG.md`](https://github.com/tucktuckg00se/INTERSECT/blob/main/CHANGELOG.md) at the root of the repository. It follows the [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.

## Quick links

- [Latest release](https://github.com/tucktuckg00se/INTERSECT/releases/latest) — download binaries
- [All releases](https://github.com/tucktuckg00se/INTERSECT/releases) — every tagged version with notes
- [Unreleased changes](https://github.com/tucktuckg00se/INTERSECT/blob/main/CHANGELOG.md#unreleased) — what's queued for the next release
- [Open issues](https://github.com/tucktuckg00se/INTERSECT/issues) — known bugs and feature requests

## Versioning

INTERSECT uses semantic-style versioning relative to the 0.x series:

- **Minor bumps (`0.X.0`)** — new features, UI redesigns, session-format changes
- **Patch bumps (`0.X.Y`)** — batched bug fixes, cosmetic tweaks, performance improvements

Releases happen roughly every 2–4 weeks for minors and at most weekly for patches.

## Session compatibility

INTERSECT writes its session state with a version number. The current loader accepts versions back several releases (today: v19 through v24) so upgrades don't strand your saved projects. If you downgrade the plugin past a session's save version, that session won't load — upgrade back to the version that saved it (or newer) to recover.
