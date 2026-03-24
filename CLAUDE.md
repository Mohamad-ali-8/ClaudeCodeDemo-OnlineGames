# CLAUDE.md — Offline Gaming Portal

## Project Overview

A fully self-contained, offline-capable browser gaming portal. Zero backend. Zero API calls. Zero external dependencies at runtime. Pure HTML, CSS, and vanilla JavaScript. Deployed to **Azure Static Web Apps** via GitHub Actions on every push to `main`. Also runnable locally by opening `index.html` directly in a browser (no server needed).

---

## Architecture

```
/
├── index.html              # Main menu — the game hub
├── style.css               # Global styles + game card grid
├── assets/
│   └── icons/              # SVG icons (inline-embedded in index.html, also as .svg files)
├── games/
│   ├── chess/
│   │   └── index.html
│   ├── snake/
│   │   └── index.html
│   ├── tetris/
│   │   └── index.html
│   ├── minesweeper/
│   │   └── index.html
│   ├── 2048/
│   │   └── index.html
│   ├── sudoku/
│   │   └── index.html
│   ├── pong/
│   │   └── index.html
│   ├── memory-match/
│   │   └── index.html
│   ├── breakout/           # Coming Soon
│   │   └── index.html
│   ├── pacman/             # Coming Soon
│   │   └── index.html
│   ├── connect-four/       # Coming Soon
│   │   └── index.html
│   └── wordle/             # Coming Soon
│       └── index.html
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions → Azure Static Web Apps
└── CLAUDE.md
```

---

## Game Roster

| Game | Folder | Status | Notes |
|---|---|---|---|
| Chess | `games/chess/` | ✅ Built | Full piece movement, check detection |
| Snake | `games/snake/` | ✅ Built | Classic grid snake with score |
| Tetris | `games/tetris/` | ✅ Built | All 7 tetrominoes, line clearing |
| Minesweeper | `games/minesweeper/` | ✅ Built | 3 difficulty levels |
| 2048 | `games/2048/` | ✅ Built | Swipe + arrow key support |
| Sudoku | `games/sudoku/` | ✅ Built | Puzzle generator + validator |
| Pong | `games/pong/` | ✅ Built | 2-player (keyboard) + vs AI mode |
| Memory Match | `games/memory-match/` | ✅ Built | Emoji symbol pairs, 4x4 grid |
| Breakout | `games/breakout/` | 🔜 Coming Soon | Paddle + ball + brick grid |
| Pac-Man | `games/pacman/` | 🔜 Coming Soon | Maze, ghosts, pellets |
| Connect Four | `games/connect-four/` | 🔜 Coming Soon | 2-player + AI |
| Wordle Clone | `games/wordle/` | 🔜 Coming Soon | 5-letter word guessing |

---

## Main Menu (`index.html`)

### Design Brief
- **Aesthetic**: Dark arcade theme. Deep navy/black background. Neon-accent game cards. Font: `Orbitron` (Google Fonts — load via `<link>` in `<head>`, acceptable for dev; for strict offline, embed as base64 `@font-face`).
- **Layout**: CSS Grid, responsive — 4 columns on desktop, 2 on tablet, 1 on mobile.
- **Each game card** contains:
  - Inline SVG icon (see Icon Spec below)
  - Game title
  - Short one-line description
  - Click behaviour: navigates to `games/<name>/index.html` if built; shows **Coming Soon modal** if not yet built.

### Card Markup Pattern
```html
<a class="game-card" href="games/chess/index.html" data-status="ready">
  <div class="card-icon"><!-- inline SVG here --></div>
  <h2 class="card-title">Chess</h2>
  <p class="card-desc">Classic strategy, no clock</p>
</a>

<a class="game-card" href="#" data-status="coming-soon">
  <div class="card-icon coming-soon-overlay"><!-- inline SVG here --></div>
  <h2 class="card-title">Breakout</h2>
  <p class="card-desc">Bricks won't break themselves</p>
  <span class="badge-soon">Coming Soon</span>
</a>
```

### Coming Soon Behaviour
Cards with `data-status="coming-soon"` must:
1. Prevent navigation (use `href="#"` + JS `preventDefault`)
2. Show a modal or inline toast: **"🚧 This game is coming soon!"**
3. Never navigate away from the hub

---

## Icon Specification

All icons are **inline SVG**, hand-crafted per game. No external images. No `<img src>` tags. Icons are embedded directly in `index.html` cards.

Each icon SVG:
- ViewBox: `0 0 120 120`
- Dark themed background matching game palette
- Contains visual elements representative of the game (see preview)
- Self-contained — no external references

### Icon Palette Reference
| Game | Background | Accent |
|---|---|---|
| Chess | `#1a1a2e` | `#534AB7` (purple) |
| Snake | `#0d2b1a` | `#1D9E75` (teal) |
| Tetris | `#0a0a1a` | Multi-colour tetrominos |
| Minesweeper | `#1a1a1a` | `#888780` (gray) |
| 2048 | `#1a0f05` | `#BA7517` (amber) |
| Sudoku | `#05051a` | `#7F77DD` (purple-light) |
| Pong | `#000000` | `#D3D1C7` (off-white) |
| Memory Match | `#0a1a1a` | `#5DCAA5` (teal-light) |
| Breakout | `#1a0505` | `#D85A30` (coral) |
| Pac-Man | `#0a0a00` | `#EF9F27` (amber) + `#2266CC` (blue walls) |
| Connect Four | `#050a1a` | `#185FA5` (blue) |
| Wordle | `#0a0a0a` | `#639922` (green) |

---

## Individual Game Pages

Each `games/<name>/index.html` is a **self-contained single file**:
- All CSS inlined in `<style>`
- All JS inlined in `<script>`
- A "← Back to Menu" button linking to `../../index.html`
- No external CDN calls at runtime (no jQuery, no lodash, no frameworks)
- Mobile-friendly: touch events where applicable

### Game Implementation Notes

**Chess** (`games/chess/`)
- Render an 8×8 board using CSS Grid or HTML table
- Unicode chess pieces: ♔♕♖♗♘♙ / ♚♛♜♝♞♟
- Implement: legal move highlighting, turn switching, check detection, checkmate detection
- Optional: castling, en passant, pawn promotion (prompt for piece)

**Snake** (`games/snake/`)
- Canvas-based, 20×20 grid cells
- Arrow keys + WASD controls
- Speed increases every 5 food items
- High score stored in `localStorage`

**Tetris** (`games/tetris/`)
- Canvas-based
- 7 standard tetrominoes with standard SRS rotation
- Ghost piece (translucent drop preview)
- Line clear animation, level/speed progression
- High score in `localStorage`

**Minesweeper** (`games/minesweeper/`)
- Three modes: Easy (9×9, 10 mines), Medium (16×16, 40 mines), Hard (30×16, 99 mines)
- First click is always safe (regenerate board if needed)
- Right-click to flag
- Timer + mine counter display

**2048** (`games/2048/`)
- 4×4 grid, arrow key + swipe support
- Tile merge animation (CSS transition)
- Best score in `localStorage`

**Sudoku** (`games/sudoku/`)
- Pre-seeded set of at least 20 puzzles (hardcoded, no generation required)
- Three difficulty levels (Easy/Medium/Hard)
- Highlight row/column/box on cell selection
- Validate button + reveal solution option

**Pong** (`games/pong/`)
- Canvas-based
- Mode select: 2-Player (W/S vs ↑/↓) or vs AI
- AI tracks ball with slight lag to be beatable
- Score to 7, game over screen

**Memory Match** (`games/memory-match/`)
- 4×4 grid = 8 pairs
- Symbols pool: use emoji (🌙 ⭐ 🔥 💎 🎵 🎯 🚀 🌈)
- Flip animation (CSS `rotateY`)
- Move counter + timer

---

## GitHub Actions Deployment — Azure Static Web Apps

File: `.github/workflows/deploy.yml`

```yaml
name: Deploy to Azure Static Web Apps

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main
  workflow_dispatch:

jobs:
  build_and_deploy:
    if: github.event_name == 'push' || github.event_name == 'workflow_dispatch' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Deploy to Azure Static Web Apps
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: upload
          app_location: "/"        # Root of the repo is the app
          api_location: ""         # No API — leave empty
          output_location: ""      # No build step — leave empty

  close_pull_request:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request
    steps:
      - name: Close PR environment
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          action: close
```

### Azure Static Web Apps Setup (one-time, manual)

1. **Create the Azure resource**
   - Go to [portal.azure.com](https://portal.azure.com)
   - Search for **Static Web Apps** → **Create**
   - Subscription + Resource Group: use existing or create new
   - Name: e.g. `offline-game-portal`
   - Plan: **Free** (sufficient for this project)
   - Region: pick closest to your users (e.g. `East Asia`, `Australia East`)
   - Deployment source: **GitHub** → authorise → select your repo + `main` branch
   - Build preset: **Custom**
   - App location: `/`
   - Api location: *(leave blank)*
   - Output location: *(leave blank)*
   - Click **Review + Create**

2. **Copy the deployment token**
   - Once created, go to the resource → **Manage deployment token**
   - Copy the token

3. **Add token to GitHub repo secrets**
   - Go to your GitHub repo → **Settings → Secrets and variables → Actions**
   - New secret: name = `AZURE_STATIC_WEB_APPS_API_TOKEN`, value = paste token

4. **Push to `main`** → GitHub Actions auto-deploys → live URL appears in the Azure portal under **URL**

### Notes
- Pull requests automatically get a **staging preview URL** — great for testing new games before merging
- The free tier supports custom domains (add via Azure portal → **Custom domains**)
- No `staticwebapp.config.json` needed for this project since there are no routes to configure beyond the root

---

## Offline Capability

The site must work with zero network after initial load:

- ✅ No CDN-loaded JS libraries (everything inline or local)
- ✅ No external API calls
- ✅ No web fonts via CDN at runtime (either load on first visit and cache, or embed as base64)
- ✅ Add a minimal `service-worker.js` + `manifest.json` for PWA installability (optional stretch goal)
- ✅ `localStorage` for persistent scores — no database

### Optional PWA (`manifest.json`)
```json
{
  "name": "Offline Game Portal",
  "short_name": "Games",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#0a0a1a",
  "theme_color": "#1D9E75",
  "icons": [
    { "src": "assets/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "assets/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

---

## Development Standards

### HTML
- Semantic HTML5
- `lang="en"` on `<html>`
- `<meta charset="UTF-8">` and `<meta name="viewport" content="width=device-width, initial-scale=1">`
- No frameworks — vanilla only

### CSS
- CSS custom properties for theme tokens:
  ```css
  :root {
    --bg-primary: #0a0a1a;
    --bg-card: #12122a;
    --accent-green: #1D9E75;
    --accent-amber: #EF9F27;
    --accent-purple: #534AB7;
    --text-primary: #e8e6f0;
    --text-secondary: #888780;
    --border-card: rgba(255,255,255,0.08);
    --radius-card: 16px;
  }
  ```
- Mobile-first responsive grid
- No CSS frameworks (no Bootstrap, no Tailwind)

### JavaScript
- Vanilla ES6+
- No build step, no bundler, no `node_modules`
- `'use strict'` where appropriate
- Each game is fully self-contained in its own `index.html`

### File Naming
- Lowercase, hyphen-separated
- No spaces in any path

---

## What Claude Code Should Build (in order)

1. **`index.html`** — Main menu with all 12 game cards, inline SVG icons, CSS grid layout, Coming Soon modal logic
2. **`style.css`** — Global arcade theme, card grid, responsive breakpoints, modal styles
3. **`games/chess/index.html`** — Full chess implementation
4. **`games/snake/index.html`** — Snake game
5. **`games/tetris/index.html`** — Tetris
6. **`games/minesweeper/index.html`** — Minesweeper
7. **`games/2048/index.html`** — 2048
8. **`games/sudoku/index.html`** — Sudoku
9. **`games/pong/index.html`** — Pong
10. **`games/memory-match/index.html`** — Memory Match
11. **Coming Soon stubs** — `breakout`, `pacman`, `connect-four`, `wordle` each get a placeholder `index.html` that just redirects back to main menu with a "coming soon" message
12. **`.github/workflows/deploy.yml`** — GitHub Actions deployment workflow
13. **`README.md`** — Brief project description + live URL placeholder

---

## Constraints (Hard Rules)

- ❌ No `npm install`, no `package.json`, no build tools
- ❌ No React, Vue, Angular, or any JS framework
- ❌ No backend, no server, no API calls of any kind
- ❌ No external image files — icons are inline SVG only
- ❌ No `iframe`-based embedding of third-party games
- ✅ Every game must run from `file://` locally AND from Azure Static Web Apps
- ✅ Each game page must have a "← Back to Menu" navigation link
- ✅ Scores/state persist via `localStorage` where applicable
