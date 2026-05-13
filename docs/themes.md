---
title: Theme customization
nav_order: 7
---

# Theme customization

INTERSECT supports custom `.intersectstyle` themes. On first launch it creates default `dark.intersectstyle` and `light.intersectstyle` in the user theme directory.

| OS | Theme folder |
| --- | --- |
| Windows | `%APPDATA%\Roaming\INTERSECT\themes\` |
| macOS | `~/Library/Application Support/INTERSECT/themes/` |
| Linux | `~/.config/INTERSECT/themes/` |

## Create a custom theme

1. Copy one of the starter files from [`themes/`](https://github.com/tucktuckg00se/INTERSECT/tree/main/themes) and rename it, for example `mytheme.intersectstyle`.
2. Set a unique `name:` value (used in the UI theme list).
3. Edit colors as 6-digit hex `RRGGBB`.
4. Place the file in your user theme folder.
5. Restart the plugin, then use the **SET** button in the header to select the theme.

The **SET** button popup also controls interface scale (`0.5x` to `3.0x` in `0.25` steps).
