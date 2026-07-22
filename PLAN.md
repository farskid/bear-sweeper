# Bear Sweeper — Minesweeper for 5-Year-Olds

## Concept

Classic Minesweeper mechanics, but all number-based logic replaced with sensory
proximity cues a preschooler can read: color glow, sound intensity, and animal
reactions. Theme: a forest meadow where sleeping bears hide under grass tiles.
Goal: clear the meadow and rescue the bunny without waking all the bears.

## Core mechanic translation

| Minesweeper        | Bear Sweeper                                                     |
| ------------------ | ---------------------------------------------------------------- |
| Mine               | Sleeping bear                                                     |
| Number tile (1–8)  | Color glow gradient: green (0 bears adjacent) → yellow (1) → orange (2) → red (3+). Plus "Zzz" snore icon that grows with count |
| Flag               | Honey pot marker ("leave honey where a bear might be") — toggle mode |
| Instant death      | Waking a bear costs 1 of 3 balloons. Bear yawns, tile locks as revealed bear. Game over only at 0 balloons |
| Win                | All safe tiles revealed → bunny rescued, confetti + dance animation |

## Game rules

- Flow: map-select screen first → pick map → game. ⬅️ back returns to maps.
- Themes (picker on map screen, persisted):
  - 🐻 Kids — bears, flowers, honey, paw directions
  - 💎 Crystal — gem tiles, facet edges point at bombs, color gets sharper closer to bombs
- Maps (last choice persisted):
  - 🌱 Meadow 5×5 / 3 bears
  - 🌿 Woods 7×7 / 6 bears
  - 🌲 Forest 9×9 / 10 bears
  - ⛰️ Mountain 11×11 / 16 bears
  - 🌙 Night 13×13 / 24 bears
- First tap always safe and flood-opens a large area (mines placed after first click).
- Flood fill on zero-adjacency tiles, same as classic.
- 3 balloon lives shown at top. Hitting bear pops one balloon (pop animation + sound).
- Honey marker button in a dock below the board frame (not in the header, never overlapping tiles).
- Lose = funny crowned-bear victory dance + silly lose jingle.
- No timer, no score. Only stars at end (3 balloons left = 3 stars).

## Sensory cue system (the whole point)

Every revealed tile communicates proximity WITHOUT numerals:

1. **Color**: tile background glows green/yellow/orange/red per adjacency count (0/1/2/3+).
2. **Sound**: tapping a revealed tile replays proximity sound — birds chirping (safe),
   soft snore (1), louder snore (2), rumbling snore (3+). Use Web Audio API,
   synthesized or tiny embedded sounds; no large assets.
3. **Icon**: 0 = small flower sprouts; 1+ = "Zzz" glyphs sized/multiplied by count
   (this doubles as counting practice without requiring numeral literacy).

## Look & feel

- Bright, chunky, rounded. Big tap targets (min 56px tiles).
- Cartoon flat style: grass-green board, sky gradient background, floating clouds.
- All CSS/SVG art — no image assets. Emoji acceptable for bear 🐻, bunny 🐰, honey 🍯,
  balloon 🎈, flower 🌼 to keep it zero-dependency.
- Font: rounded (e.g. "Fredoka" from Google Fonts, with system fallback).

## Animations (must-haves)

- Tile reveal: springy flip/pop (scale bounce).
- Flood fill: staggered ripple reveal (tiles open in waves outward, ~40ms stagger).
- Bear wake: tile shakes, bear emoji pops up with "ROAR" wiggle, screen micro-shake.
- Balloon pop: balloon scales up and bursts into particles.
- Win: confetti rain + bunny bounce + bears wave.
- Idle: clouds drift, occasional "Zzz" floats up from unrevealed board.
- All animations CSS-based where possible; JS only for confetti/particles.

## Audio

- Web Audio API synthesized: pop, chirp, snore (low sine rumble), pop-balloon,
  win jingle (3–4 ascending notes). Mute button (persist in localStorage).
- iOS/Safari: audio context must resume on first user gesture.

## UX details for age 5

- Flag mode = big toggle button with honey icon (no right-click/long-press dependence).
- First-time tutorial wizard (4 tap-through steps, icon-heavy); auto on first map entry; ❓ replay on map screen + game header. Persisted via `bearSweeperTutorialSeen`.
- Tap feedback on EVERY interaction (scale bounce even on invalid taps).
- Win overlay: huge replay (🔄) plus next-map button (next map's emoji) when not on final map; lose = replay only.
- No text-dependent UI — icons everywhere. Any text is decorative.
- No settings menus beyond level picker (two big buttons: small/large grid).

## Tech

- Single self-contained `index.html` (inline CSS + JS, vanilla, no build step,
  no dependencies). Open in browser = play.
- Mobile-first responsive (kid will play on tablet/phone). Touch events + click.
- Prevent double-tap zoom / text selection on board.
- Location: `brainstormings/bear-sweeper/index.html`.

## Acceptance checklist

- [ ] First tap never hits bear, always opens area
- [ ] Flood fill works with staggered animation
- [ ] Color + Zzz + sound proximity cues on revealed tiles
- [ ] Tapping already-revealed tile replays its sound
- [ ] Honey flag toggle mode works on touch and mouse
- [ ] 3-balloon lives, bear wake costs one, 0 = lose screen
- [ ] Win = bunny rescue + confetti + star rating
- [ ] Mute toggle persists
- [ ] First-time tutorial shows once, ❓ replay works, theme-aware icons
- [ ] Works in Safari + Chrome, mobile viewport
