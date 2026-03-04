# Tilled QA Progress

## Team Status

| Agent | Area | Status | Bugs Found | Bugs Fixed |
|-------|------|--------|------------|------------|
| setup-qa | Setup screen, player config, farmers, tutorial | **DONE** | 2 | 2 |
| panels-qa | HUD, dashboard, market panel layout | **DONE** | 9 | 9 |
| cards-qa | Card hand, hover detail, actions, planting | **DONE** | 4 | 4 |
| visual-qa | Board rendering, hex interaction, camera, polish | **DONE** | 2 | 2 |
| endgame-qa | Trading, market buy/sell, seasons, endgame | In Progress | 0 | 0 |
| invariants-qa | State invariants, action preconditions | In Progress | 0 | 0 |
| turnflow-qa | Turn flow, seasons, endgame triggers | In Progress | 0 | 0 |
| market-qa | Scoring, market prices, trading | In Progress | 0 | 0 |
| data-qa | Data integrity, all cards/plots/infra | In Progress | 0 | 0 |
| fullgame-qa | Full game playthrough, every feature | In Progress | 0 | 0 |

## Bug Log

### setup-qa (2 bugs)
1. **Tutorial tooltip not centered** — `src/ui/tutorial.ts`: `animate-slide-up` transform overrode centering `translate(-50%, -50%)`. Fixed: changed to `animate-fade-in`, added `bottom: auto` reset.
2. **Tutorial dead-end** — `src/ui/setupScreen.ts` + `tutorial.ts`: "How to Play" hid setup screen, but tutorial end had no way to reopen it. Fixed: added `onEnd` callback to `startTutorial()` that reopens setup screen.

### panels-qa (6 bugs)
2. **Dashboard overlapped HUD** — `src/ui/playerDashboard.ts`: Dashboard had `top: 0`, hidden behind 48px HUD. Fixed: set `top: 48px`, `bottom: 150px`.
3. **Market panel overlapped HUD** — `src/ui/marketPanel.ts`: same issue. Fixed: `top: 48px`, `bottom: 150px`, z-index `z-ui` not `z-panel`.
4. **Hand panel overlapped sidebars** — `src/ui/handPanel.ts`: used `left: 0; right: 0` spanning full width. Fixed: `left: 240px; right: 240px`.
5. **Storage included Coins** — `src/ui/helpers.ts`: `getStorageUsed()` counted Coins, but Coins are unlimited per rules. Fixed: skip Coins. Also fixed `getStorageMax()` base value.
6. **Player tabs truncated to "Player"** — `src/ui/playerDashboard.ts`: `slice(0, 6)` truncated "Player 1"→"Player". Fixed: `slice(0, 10)`.
7. **ClaimPlot enabled outside Spring** — `src/ui/actionBar.ts`: missing Spring season check. Fixed: added `state.season === Season.Spring`.
8. **First-season market price inflation** — `src/state/turnManager.ts`: `startSeason()` applied price recovery (+2) at game start before any selling. Fixed: skip `applySeasonalModifiers()` when `round === 1`.
9. **Market prices not clamped to ceiling** — `src/state/gameState.ts` + `src/ui/marketPanel.ts`: starting prices exceeded player-count ceiling (e.g., RareHeirlooms 7 > 2P ceiling 6). Fixed: clamp during init and in UI display.
10. **Action bar "End Turn" invisible on macOS** — `src/ui/actionBar.ts`: `overflow-x-auto` hides scrollbar on macOS, making last button invisible. Fixed: `flex-wrap justify-center` so buttons wrap to 2 rows when needed.

### cards-qa (4 bugs)
8. **Starting hand draws 2 instead of 3** — `src/state/gameState.ts`: `drawFromDeck(2)`. Fixed: changed to 3.
9. **Card detail shows raw enum values** — `src/ui/cardDetail.ts` + `helpers.ts`: plot requirements showed "riversideFlats" instead of "Riverside Flats", triggers showed raw enums. Fixed: added PLOT_LABELS, TRIGGER_LABELS lookup maps.
10. **Claim Plot restricted to Spring (rule conflict)** — `src/ui/actionBar.ts` + `src/state/rules.ts`: cards-qa removed Spring check, but season data says Spring-only. **Resolved**: panels-qa's fix is correct (Spring-only per seasons.ts data).
11. **Harvest Festival disabled without mature crops** — `src/ui/actionBar.ts`: required mature crops. Per rulebook, Festival fires all triggers + optionally harvests. Fixed: always enabled with sufficient AP.

### visual-qa (2 bugs)
12. **Action bar wrapping at 1280x720** — `src/ui/actionBar.ts`: `flex-wrap` caused 12 buttons to wrap to 2 rows. Fixed: `overflow-x-auto`, `shrink-0` on buttons.
13. **Weather overlay animation broken** — `src/render/app.ts`: `weather.update(0, ...)` passed dt=0 and only called on state changes, not per-frame. Weather events appeared/disappeared instantly instead of fading. Fixed: moved update into ticker loop with actual delta time.

## Completed Tests

### setup-qa (9 tests, all PASS)
- Setup screen loads with title, subtitle, buttons
- Player count 2-5 buttons work
- Name inputs accept text
- Farmer selection opens grid, grays out taken farmers
- AI/Human toggle works, difficulty cycles
- Tutorial opens (after fix)
- Start 2-player game — full UI renders
- Start 4-player game — all tabs, larger board

### panels-qa (all PASS after fixes)
- HUD: season, round, phase, weather, player name, AP dots
- Dashboard: player tabs, VP, name, farmer, resources, storage bar, plots, fair entries
- Market: 8 category prices, seasonal info, sell/buy sections
- Hand: cards inset between sidebars
- Action bar: 12 buttons with AP badges, correct enabled/disabled
- Z-layering correct, no overlap

### cards-qa (11 tests, all PASS)
- Cards in hand (3+), hover detail popup, card selection/deselection
- Action buttons correct enabled/disabled
- Browse Seeds draws card
- Plant flow activates hex selection
- End Turn passes to AI, round advances
- AP dots update correctly

### visual-qa (12 checks, all PASS)
- Hex board visible, 7 terrain colors distinct
- Player ownership borders render
- Camera pan (drag) and zoom (scroll) work
- Hex click shows selection border
- Z-indexing correct (board behind panels)
- Responsive at 1920x1080, 1440x900, 1280x720
- Text readable, colors match design system
- No console errors

## Notes

- All agents using Playwright interactively (headless) with screenshot-based visual analysis
- Reference docs: ~/spriteline/game-design/ (rulebook, card system, economy, world, data)
- Dev server: http://localhost:5173
- Stack: Vite 7 + PixiJS v8 + TypeScript + Tailwind CSS v3
- ClaimPlot is Spring-only per seasons.ts data and majority of design docs (rulebook line 248 has a contradictory note about "any season in standard game" but all other sources + data say Spring-only)
