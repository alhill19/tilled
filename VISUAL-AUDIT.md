# Visual Audit Report — Tilled UI Theming

Audit date: 2026-02-28

This document catalogs every remaining unthemed, generic-dark, or inconsistent visual element in the Tilled UI codebase. The game targets a warm wood/parchment/gouache board-game aesthetic with Cinzel (display), Spectral (body), and IM Fell English (flavor) fonts.

---

## 1. CSS Custom Properties (Root Cause)

**File:** `src/app.css:10-13`

The root `:root` block still defines dark-indigo fallback values:

```css
--color-ui-bg:       #1a1a2e;   /* dark navy/indigo */
--color-ui-bg-solid:  #0f0f1e;   /* darker indigo */
--color-ui-surface:   #252540;   /* medium indigo */
--color-ui-border:    #3a3a5c;   /* indigo border */
```

These propagate everywhere Tailwind utilities like `bg-ui-bg`, `bg-ui-surface`, `border-ui-border` are used. They should be updated to warm brown/dark-wood tones to match the rest of the themed UI:
- `--color-ui-bg` → something like `#1a1208` (dark walnut)
- `--color-ui-bg-solid` → `#120d06` (deep brown-black)
- `--color-ui-surface` → `#2a1e12` (warm dark brown)
- `--color-ui-border` → `#4a3420` (warm brown border)

**Impact:** Fixing these root vars would immediately improve every panel that references them.

---

## 2. Market Panel — Container Background

**File:** `src/ui/marketPanel.ts:56`

```ts
'absolute right-0 bg-ui-bg border-l border-ui-border overflow-y-auto z-ui',
```

The market panel's outer container uses `bg-ui-bg` (dark indigo) and `border-ui-border` (indigo border). While the interior content (chalkboard, stall header, rope dividers) is richly themed, the panel container itself shows generic indigo around the edges and between sections.

**Priority:** Medium (mostly hidden by themed content, but visible during scroll and at edges).

---

## 3. Trade Panel — Stepper Buttons

**File:** `src/ui/tradePanel.ts:234-235, 256-257`

```ts
'stepper-btn w-8 h-8 rounded-md bg-ui-bg border border-ui-border text-ui-text ...'
```

Both the decrement and increment stepper buttons in the resource rows use `bg-ui-bg` (dark indigo background) and `border-ui-border` (indigo border). The rest of the trade panel uses `modal-panel`, `decorateModal()`, `section-divider-wood`, and `resource-cell-wood` classes, so these stepper buttons are the only unthemed elements.

**Fix:** Replace `bg-ui-bg border border-ui-border` with warm wood styling, e.g. `background:rgba(0,0,0,0.3);border:1px solid rgba(200,160,80,0.2)` or a dedicated `.stepper-btn-wood` CSS class.

**Priority:** High — these buttons are visually prominent in the trade flow.

---

## 4. Exchange Panel — Resource Pills

**File:** `src/ui/exchangePanel.ts:126-127, 173`

```ts
'resource-pill py-2 px-3 rounded-full border border-ui-border bg-transparent ...'
'resource-pill py-2 px-3 rounded-full border border-ui-border/30 bg-transparent ...'
```

The exchange panel's resource selection pills use `border-ui-border` (indigo). The `resource-pill` CSS class itself is partially themed, but these Tailwind border overrides reintroduce the indigo border color. The rest of the exchange panel uses `modal-panel` and `decorateModal()`.

**Priority:** Medium — the border color is subtle but inconsistent.

---

## 5. Tutorial — Tooltip and Controls

**File:** `src/ui/tutorial.ts`

The tutorial overlay is the most visually unthemed component:

| Line | Element | Issue |
|------|---------|-------|
| 195 | Highlight ring | `border-2 border-ui-accent` — accent is fine, but no warm glow or texture |
| 202 | Tooltip container | `bg-ui-surface border-2 border-ui-accent` — uses `bg-ui-surface` (#252540 indigo) |
| 248 | Skip button | `border border-ui-border` — indigo border |
| 273 | Progress bar track | `bg-white/10` — generic, no warm tint |
| 290 | Back button | `border border-ui-border` — indigo border |
| 305 | Next button | `bg-ui-accent/20` — semi-transparent accent over indigo surface |

**Fix:** The tooltip should use a parchment/field-guide-page background like other modals, with warm brown borders instead of `bg-ui-surface`. The progress bar should use a warm brown track. Buttons should use `btn-gold` / `btn-cancel` patterns.

**Priority:** High — the tutorial is the first thing new players see, and it looks noticeably different from the rest of the themed game.

---

## 6. Setup Screen — Background Gradient

**File:** `src/ui/setupScreen.ts:47-52`

```ts
overlayEl.style.cssText = `
  background:
    url('/ui/field-guide-page.png') center / cover no-repeat,
    radial-gradient(ellipse at 50% 30%, rgba(200,160,80,0.06) 0%, transparent 60%),
    linear-gradient(180deg, #0f0f1e 0%, #1a1a2e 100%);
`;
```

The fallback gradient behind the parchment texture uses `#0f0f1e` to `#1a1a2e` — both dark indigo/navy. If the parchment image fails to load or is partially transparent, the indigo bleeds through. All other similar backgrounds in the app use warm dark browns.

**Fix:** Change to warm tones like `linear-gradient(180deg, #0c0806 0%, #1a1208 100%)`.

**Priority:** Low (parchment covers most of it), but easy fix.

---

## 7. Endgame Screen — Background Overlay

**File:** `src/ui/endgameScreen.ts:26-29`

```ts
background:
  radial-gradient(ellipse at 50% 20%, rgba(200,160,80,0.08) 0%, transparent 60%),
  linear-gradient(180deg, rgba(15,15,30,0.97) 0%, rgba(10,10,20,0.99) 100%);
```

The endgame screen overlay uses `rgba(15,15,30,...)` and `rgba(10,10,20,...)` — both dark navy/indigo with blue undertones. The content inside (score table, winner announcement, buttons) is fully themed with gold/parchment, but the overall background tone clashes.

**Fix:** Change to warm tones like `rgba(15,12,8,0.97)` and `rgba(10,8,5,0.99)`.

**Priority:** Medium — the endgame screen is important, and the dark navy tone is noticeable behind the themed content.

---

## 8. Dead Code — Unthemed Helper Functions

**File:** `src/ui/helpers.ts:382-401`

Three exported helper functions are defined but never used anywhere in the codebase:

```ts
// Line 389: secondary variant
return `${base} bg-ui-surface text-ui-text border border-ui-border hover:bg-ui-border`;

// Line 396:
return 'bg-ui-surface border border-ui-border rounded-lg p-3';

// Line 400:
return 'text-xs uppercase tracking-wider text-ui-text-dim font-bold mb-2';
```

- `btnClasses('secondary')` returns generic indigo styling
- `panelClasses()` returns generic indigo styling
- `sectionTitleClasses()` is fine but unused

**Fix:** Remove all three functions (dead code). If ever reintroduced, they should use warm wood/parchment styling.

**Priority:** Low — dead code, no visual impact, but should be cleaned up.

---

## Summary Table

| Component | File | Severity | Issue |
|-----------|------|----------|-------|
| CSS root vars | `app.css:10-13` | **Critical** | Indigo fallbacks propagate everywhere |
| Tutorial tooltip | `tutorial.ts:202` | **High** | `bg-ui-surface` — first thing new players see |
| Tutorial buttons | `tutorial.ts:248,290,305` | **High** | Indigo borders and backgrounds |
| Tutorial progress bar | `tutorial.ts:273` | **High** | Generic `bg-white/10` |
| Trade stepper buttons | `tradePanel.ts:235,257` | **High** | `bg-ui-bg border-ui-border` — prominent |
| Market panel container | `marketPanel.ts:56` | **Medium** | `bg-ui-bg border-ui-border` outer shell |
| Exchange panel pills | `exchangePanel.ts:126-127,173` | **Medium** | `border-ui-border` on resource pills |
| Endgame background | `endgameScreen.ts:26-29` | **Medium** | Dark navy rgba tones |
| Setup screen fallback | `setupScreen.ts:51` | **Low** | Indigo gradient behind parchment |
| Dead helper functions | `helpers.ts:382-401` | **Low** | Unused, should be removed |

---

## Fully Themed Components (No Issues)

The following panels are fully themed and require no changes:

- **HUD bar** (`hud.ts`) — wood plank with AP tokens, resource bar, season badge
- **Action bar** (`actionBar.ts`) — carved wood buttons with gouache icons
- **Hand panel** (`handPanel.ts`) — parchment cards with painted spines and stripes
- **Player dashboard** (`playerDashboard.ts`) — parchment almanac with section dividers, VP seal
- **Build panel** (`buildPanel.ts`) — modal-panel + infra-card themed elements
- **Harvest panel** (`harvestPanel.ts`) — modal-panel with crop art and warm dividers
- **Card detail** (`cardDetail.ts`) — field-guide-page parchment background
- **Seed panel** (`seedPanel.ts`) — field-guide-page + botanical corner ornaments
- **Season banner** (`seasonBanner.ts`) — full-screen dramatic seasonal reveal
- **Toast notifications** (`toast.ts`) — warm wood gradient with thematic borders
- **Game notifications** (`notifications.ts`) — wood-toned notification cards
- **Plot detail** (`plotDetail.ts`) — dark warm brown (#1e1208) tooltip

---

## Recommendations

1. **Start with CSS root vars** (`app.css:10-13`). Changing these four values from indigo to warm brown will fix *most* remaining issues across the entire app in one change.

2. **Retheme the tutorial** (`tutorial.ts`) — this is the first impression for new players and should match the rest of the game.

3. **Fix trade stepper buttons** (`tradePanel.ts:235,257`) — replace inline `bg-ui-bg` with warm wood styling.

4. **Update endgame background** (`endgameScreen.ts:26-29`) — swap navy rgba to warm brown rgba.

5. **Remove dead helpers** (`helpers.ts:382-401`) — clean up unused code.
