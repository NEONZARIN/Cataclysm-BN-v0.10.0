# Tileset zoom persistence variants

This document outlines four alternative implementation variants for persisting tileset zoom
settings across sessions. Each variant is designed to be split into a separate PR if needed.

## Variant A: Global options (config-based)

**Scope:** Entire install (shared across all worlds and characters).

**Approach:**
- Store `TILESET_ZOOM` and `OVERMAP_TILESET_ZOOM` in the global options file.
- Read values on startup and apply to tileset scale.
- Write back when the player zooms.

**Pros:**
- Simple implementation.
- Consistent zoom for all worlds/characters.

**Cons:**
- No per-world or per-character customization.

## Variant B: Per-world UI state

**Scope:** World-specific (shared across characters in the same world).

**Approach:**
- Add `tileset_zoom` and `overmap_tileset_zoom` fields to `uistate`.
- Serialize them in `uistate.json`.
- Apply after `uistate` loads on save load and on new world start.

**Pros:**
- Keeps zoom scoped to the active world.
- Reuses existing `uistate` persistence.

**Cons:**
- Shared across characters in the same world.

## Variant C: Per-character save

**Scope:** Character-specific.

**Approach:**
- Add zoom fields to `avatar` (or `player`) serialization.
- Apply after character data is loaded.
- Write back on zoom changes.

**Pros:**
- True per-character preference.

**Cons:**
- Requires extending the player save format.

## Variant D: Session-only with optional manual save

**Scope:** Current session unless the player explicitly saves it.

**Approach:**
- Track zoom in runtime state only.
- Add an action or menu option to copy current zoom into global options
  (or another persistence target) on demand.

**Pros:**
- Minimal persistence side effects.
- Player-controlled when to save.

**Cons:**
- Not persistent by default.
