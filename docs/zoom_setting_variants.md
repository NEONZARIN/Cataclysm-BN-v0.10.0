# Tileset Zoom Setting Variants (Draft)

This document outlines four alternative implementations for persisting default tileset zoom levels.
Each variant is intentionally scoped so it can be split into its own PR.

## Variant 1: Global options (config-driven)
* **Storage**: `options.json` (global).
* **UI**: New graphics options for main tileset and overmap tileset zoom defaults.
* **Behavior**:
  * Apply saved defaults during game initialization.
  * Update option values when zooming in/out so the setting persists between sessions.
* **Pros**: Simple, user-visible settings; works across worlds.
* **Cons**: Not world-specific; overwrites personal per-world preferences.

## Variant 2: World-scoped options (world defaults)
* **Storage**: World options (`WORLD_OPTIONS`).
* **UI**: New entries under world-default options for tileset and overmap zoom.
* **Behavior**:
  * Apply per-world zoom values when loading a world/save.
  * Update world options when zooming in/out.
* **Pros**: Allows per-world defaults; aligns with other world configuration.
* **Cons**: More involved plumbing; requires careful handling for existing saves.

## Variant 3: UI state persistence (uistate.json)
* **Storage**: `uistate.json` per world/save.
* **UI**: Optional (could be a hidden setting, or no dedicated UI entry).
* **Behavior**:
  * Store zoom values in `uistatedata` when zoom changes.
  * Restore zoom values after `uistate` is loaded.
* **Pros**: Per-save persistence with minimal user-facing complexity.
* **Cons**: Not easily discoverable in options; requires clear documentation if no UI.

## Variant 4: Tileset metadata override (tileset config)
* **Storage**: Tileset config (e.g., `tileset.txt`) optional zoom defaults.
* **UI**: Optional; could be overridden by global/world settings if present.
* **Behavior**:
  * If tileset config defines default zoom, apply it on tileset load.
  * Allow a user setting to override the tileset’s suggested default.
* **Pros**: Allows tilesets to ship with recommended zoom defaults.
* **Cons**: Requires tileset metadata schema change; precedence rules must be clear.
