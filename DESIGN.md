# Tilled — Visual Design Style Guide

## Overview

Tilled is a competitive farming board game with a warm, handcrafted aesthetic. The visual identity blends **gouache botanical illustration** (card art) with **pixel-art field sprites** (board), unified by a dark, earthy UI with gold accents. The mood is cozy, rustic, and slightly nostalgic — like a well-loved farmer's almanac lit by lantern light.

---

## Art Styles

### Card Art — Gouache Botanical Illustration

The card art that players hold in hand and see on detail popups.

- **Technique**: Hand-painted gouache on textured paper. Visible brushstrokes with soft edges where colors meet. Not photorealistic, not vector, not cel-shaded.
- **Palette**: Warm, slightly desaturated earth tones. Sun-faded and organic — nothing neon. Highlights lean warm cream (`#F5E6C8`), shadows lean warm brown (`#5C4033`). Never pure black or pure white.
- **Outlines**: Thin dark brown (`#3B2716`), 1-2px apparent weight, slightly irregular hand-drawn feel. Softest where form curves away.
- **Shading**: Single-direction warm light from upper-left. Soft ambient occlusion at joins (leaf meets stem, root meets soil). No hard cast shadows. 2-3 value steps per color region.
- **Detail level**: Medium — enough to identify the specific cultivar, but stylized. Leaf veins as simple lines, textures suggested through color variation.
- **Composition**: Subject centered, filling 70-80% of canvas. Slight natural asymmetry (a leaf tilts, a root curves). Harvested/display form with minimal plant context. No ground, no pots, no scenery.
- **Scale**: Consistent apparent size regardless of real-world dimensions. A single carrot and a whole watermelon occupy similar canvas area.
- **Format**: 512x512 PNG, transparent background.

### Field Sprites — Pixel Art

The small crop sprites that appear on hex tiles when crops are planted.

- **Technique**: 32-color indexed pixel art. Clean hand-placed pixels. No anti-aliasing, no sub-pixel rendering, no dithering.
- **Palette**: Max 32 colors per sprite. Greens: 4 shades from dark forest (`#2D5A1E`) to bright leaf (`#7EC850`). Browns: 3 shades for soil (`#4A3520`, `#6B4C32`, `#8B6B4A`). Plus 2-4 accent shades per crop. Black (`#1A1A1A`) for outlines only.
- **Outlines**: 1px dark outlines (`#1A1A1A`) on exterior edges only. Interior boundaries use color contrast, not outlines.
- **Shading**: 2 value steps per region (base + shadow). Light from upper-left. Shadow pixels on lower-right edges. No highlights — base color IS the lit side.
- **Perspective**: Strict 3/4 top-down (~60 degrees). Soil patch is a flattened oval. Plants grow "up" in screen space.
- **Composition**: Circular soil patch in lower 40%. Plant grows from soil center, filling upper 60%. Must read clearly at 20x20px display size.
- **Scale**: All crops same sprite size. Stylize proportions to fit rather than showing real scale differences.
- **Format**: 512x512 source PNG (displayed at ~28px on board), transparent background.

---

## Color System

### Core UI

| Token | Hex | Usage |
|-------|-----|-------|
| `ui-bg` | `#1a1a2e` | Page/board background |
| `ui-bg-solid` | `#0f0f1e` | Opaque panel backgrounds (hand, HUD) |
| `ui-surface` | `#252540` | Cards, buttons, raised surfaces |
| `ui-border` | `#3a3a5c` | Panel borders, dividers |
| `ui-accent` | `#c8a050` | Gold — primary interactive color, VP display, highlights |
| `ui-accent-hover` | `#dab060` | Hover state for accent elements |
| `ui-text` | `#e8e0d0` | Primary text (warm parchment white) |
| `ui-text-dim` | `#8a8478` | Secondary/label text |
| `ui-danger` | `#c05050` | Errors, storage warnings, negative state |
| `ui-success` | `#50a050` | Positive state, growth indicators |

### Seasons

| Season | Hex | Character |
|--------|-----|-----------|
| Spring | `#4ade80` | Fresh green — planting & claiming |
| Summer | `#fbbf24` | Warm amber — growth & abundance |
| Autumn | `#f97316` | Burnt orange — harvest time |
| Winter | `#60a5fa` | Cool blue — dormancy & planning |

### Crop Categories

| Category | Hex | Used for |
|----------|-----|----------|
| Root Vegetables | `#c2410c` | Card stripes, badges, labels |
| Leafy Greens | `#16a34a` | |
| Fruits | `#dc2626` | |
| Herbs | `#7c3aed` | |
| Fungi | `#78716c` | |
| Flowers | `#ec4899` | |
| Legumes | `#15803d` | |
| Grains | `#d4a850` | |
| Rare Heirlooms | `#ca8a04` | |

### Resources

| Resource | Icon | Hex |
|----------|------|-----|
| Water | `💧` | `#3b82f6` |
| Compost | `🌱` | `#65a30d` |
| Seeds | `🌰` | `#a16207` |
| Sunlight | `☀️` | `#eab308` |
| Labor | `🔨` | `#78716c` |
| Coins | `🪙` | `#d97706` |

### Plot Types

| Plot | Hex |
|------|-----|
| Riverside | `#4a90d9` |
| Hillside | `#d4a843` |
| Forest | `#2d6b3f` |
| Marsh | `#7a6b8a` |
| Greenhouse | `#5bbfb5` |
| Rocky | `#8a7d6b` |
| Meadow | `#c4a83d` |
| Unclaimed | `#5a5a7a` |

### Player Colors

`#ef4444` / `#3b82f6` / `#22c55e` / `#a855f7` / `#f97316`

---

## Typography

| Role | Font | Weight | Size |
|------|------|--------|------|
| Display / titles | Georgia (serif) | Bold | `text-xl` (1.25rem) |
| UI body / buttons | Inter (sans-serif) | Semibold | `text-sm` (0.875rem) |
| Labels / metadata | Inter | Semibold | `text-2xs` (0.625rem), uppercase, wide tracking |
| Card names | Inter | Semibold | `text-xs` (0.75rem) |
| Victory point hero | Inter | Bold | `text-4xl` (2.25rem) |

---

## UI Components

### Panel Pattern
- Dark opaque background (`ui-bg-solid`)
- Single-pixel border (`ui-border`)
- Internal padding `p-4`
- Section titles: uppercase, `text-2xs`, `tracking-wider`, `text-ui-text-dim`, bottom border separator

### Card (Hand)
- Fixed width `130px`, rounded `lg`
- Category color stripe at top (2px, `rounded-b-full`)
- Card art image at 50% width, centered
- Crop name, category label (in category color), planting cost, growth speed, VP badge
- Hover: lifts 4px (`-translate-y-1`), border transitions to accent
- Selected: gold border, raised 6px

### Buttons (Action Bar)
- Rounded `md`, `bg-ui-surface`, `border-ui-border`
- Hover: border goes gold, slight brightness increase
- Disabled: 30% opacity, `cursor-not-allowed`
- AP cost badge: gold circle pinned to top-right corner, `text-2xs` bold

### Market Panel
- Crop price rows: dot (category color) + label + bold gold price right-aligned
- Demand card: `bg-ui-surface`, gold border at 30% opacity, rounded `lg`
- Seasonal info box: italic, dim text, `bg-ui-surface/50`

---

## Layout

```
┌──────────────────────────────────────────────┐
│                  HUD (48px)                  │
├──────────┬──────────────────────┬────────────┤
│ Player   │                      │  Market    │
│ Dashboard│    Hex Board Canvas   │  Panel     │
│ (240px)  │                      │  (240px)   │
│          │                      │            │
├──────────┴──────────────────────┴────────────┤
│              Action Bar (floating)           │
├──────────────────────────────────────────────┤
│           Hand Panel (200px, full width)     │
└──────────────────────────────────────────────┘
```

### Z-Index Layers

| Layer | Z-Index | Content |
|-------|---------|---------|
| Board | 10 | Hex tiles, PixiJS canvas |
| UI | 100 | Side panels, action bar |
| HUD | 200 | Top bar, hand panel |
| Panel | 300 | Modals in panels |
| Modal | 400 | Dialogs, confirmations |
| Overlay | 500 | Full-screen overlays |
| Tutorial | 600 | Tutorial highlights |
| Tooltip | 700 | Card detail popup |

---

## Design Principles

1. **Warm over cool** — The palette leans warm at every level. Even the "dark" backgrounds are deep indigo (`#1a1a2e`), not cold gray. Gold is the accent, not blue.
2. **Hand-made over digital** — Card art looks painted, field sprites look hand-pixeled. Visible brushstrokes and deliberate pixels over smooth gradients.
3. **Legibility at every scale** — Field sprites must read at 20px. Card art must read at 130px wide. Category colors must be distinct from each other at a glance.
4. **Consistent internal scale** — All crops drawn at similar canvas fill regardless of real-world size. A carrot is as visually important as a watermelon.
5. **Restraint** — 2-3 value steps per shading region, not 10. 32 colors per sprite, not unlimited. Medium detail, not photographic. The style is defined as much by what it leaves out.
6. **Dark UI, bright subjects** — The interface recedes into deep indigo-black so the warm crop illustrations and gold accents command attention.
