# NANDINI-
color mixing game 
# Color Mixer Game

A browser-based color guessing and mixing game built with vanilla HTML, CSS, and JavaScript. No frameworks, no dependencies — just open the file and play.

---

## What it is

Color Mixer is a stress-relief game where you're shown a named target color and challenged to recreate it by adjusting Red, Green, and Blue (RGB) values. You earn points based on how closely your mix matches the target. The closer you get, the higher your score.

It's designed to be calming and absorbing — the focus required to match a color is a surprisingly effective way to quiet a busy mind.

---

## How to play

1. A **target color** is displayed on the left with its name (e.g. "Sunset Orange")
2. Click a **paint pot** on the palette to start from a base color
3. Use the **R / G / B sliders** to fine-tune your mix
4. Watch the **match % bar** update in real time
5. Hit **Submit mix** when you're ready
6. Score is awarded based on accuracy — then a new color loads automatically

---

## Scoring

| Match accuracy | Points (no hint) | Points (hint used) |
|---|---|---|
| 95% and above | 100 | 80 |
| 85 – 94% | 70 | 50 |
| 70 – 84% | 40 | 25 |
| 55 – 69% | 20 | 10 |
| Below 55% | 0 | 0 |

- Score **150 × current level** to level up
- Build a **streak** by hitting 85%+ multiple times in a row
- Using the **Hint** button reduces your points for that round

---

## Features

- 18 named target colors to match
- 10 paint pot shortcuts (Red, Yellow, Blue, White, Black, Orange, Green, Purple, Pink, Cyan)
- Real-time RGB sliders with live color preview
- Match percentage bar with color-coded feedback (green / amber / red)
- Scoring system with levels and streak tracking
- Hint system that reveals a clue about one channel
- Recent attempts history showing color swatches and match %
- Fully offline — no internet or server needed

---

## Project structure

```
color_mixer_game.html   ← entire project in one file
```

Everything — HTML markup, CSS styles, and JavaScript logic — lives in a single `.html` file. This makes it easy to share, host, or modify.

---

## How it works (technical overview)

### Color representation
Colors are stored as RGB triplets `{ r, g, b }` with values 0–255. The hex string is computed with:

```js
function toHex(r, g, b) {
  return '#' + [r, g, b].map(v => v.toString(16).padStart(2, '0')).join('');
}
```

### Match calculation
Match percentage is calculated using Euclidean distance in RGB space:

```js
function matchPct(r1, g1, b1, r2, g2, b2) {
  const dr = r1 - r2, dg = g1 - g2, db = b1 - b2;
  const maxDiff = Math.sqrt(3 * 255 * 255); // max possible distance
  return Math.round((1 - Math.sqrt(dr*dr + dg*dg + db*db) / maxDiff) * 100);
}
```

The maximum possible distance between two RGB colors is `√(3 × 255²) ≈ 441.67`. Dividing your actual distance by this gives a normalized score.

### Color naming
A simplified HSL conversion is used to guess a human-readable name for the current mix:

```js
function colorName(r, g, b) {
  const [h, s] = rgbToHsl(r, g, b);
  if (s < 0.15) return r > 200 ? 'White' : r > 120 ? 'Gray' : 'Black';
  const names = ['Red','Coral','Orange','Yellow','Lime','Green',
                 'Teal','Cyan','Blue','Indigo','Violet','Magenta'];
  return names[Math.round(h / 30) % 12];
}
```

### Game loop
- `newRound()` — picks a random target color not seen in the last 3 rounds
- `onSlider()` — fires on every slider change, updates the mix preview live
- `submitMix()` — computes match %, awards points, updates streak, triggers next round

---

## Customisation

### Add more target colors
Edit the `COLORS` array at the top of the script:

```js
const COLORS = [
  { name: 'Crimson red', r: 220, g: 20, b: 60 },
  { name: 'Your color',  r: 100, g: 200, b: 50 },
  // ...
];
```

### Add more paint pots
Edit the `POTS` array:

```js
const POTS = [
  { name: 'Red',    r: 220, g: 30,  b: 30  },
  { name: 'Brown',  r: 139, g: 69,  b: 19  },
  // ...
];
```

### Change scoring thresholds
Find the `submitMix()` function and adjust the `if/else` conditions:

```js
if (pct >= 95) { pts = 100; ... }
else if (pct >= 85) { pts = 70; ... }
// etc.
```

### Change level-up threshold
```js
if (score >= level * 150) { level++; }
// change 150 to any value
```

---

## Running locally

No setup needed. Just:

```bash
# Option 1 — double-click the file in your file explorer
color_mixer_game.html

# Option 2 — open from terminal
open color_mixer_game.html        # macOS
start color_mixer_game.html       # Windows
xdg-open color_mixer_game.html    # Linux
```

---

## Possible extensions

- **Timer mode** — race to match the color in under 30 seconds
- **Multiplayer** — two players compete to match the same color
- **Color theory mode** — learn complementary, analogous, triadic color relationships
- **Paint mixing simulation** — subtractive color mixing (RYB) instead of RGB
- **Accessibility mode** — show color names for players with color vision deficiency
- **Leaderboard** — save high scores to localStorage

---

## Technologies used

| Technology | Purpose |
|---|---|
| HTML5 | Structure and layout |
| CSS3 | Styling, transitions, responsive grid |
| Vanilla JavaScript | Game logic, color math, DOM updates |

No external libraries, frameworks, or build tools required.

---

## License

Free to use, modify, and share for personal or educational purposes.
