# Offline Game Portal

A fully self-contained browser gaming portal with 8 playable classic games. Zero backend. Zero API calls. Pure HTML, CSS, and vanilla JavaScript.

## Live Demo

🌐 **[Play now →](https://your-portal.azurestaticapps.net)** *(replace with your Azure Static Web Apps URL)*

## Games

| Game | Status |
|------|--------|
| ♟️ Chess | ✅ Playable |
| 🐍 Snake | ✅ Playable |
| 🟦 Tetris | ✅ Playable |
| 💣 Minesweeper | ✅ Playable |
| 🔢 2048 | ✅ Playable |
| 🔢 Sudoku | ✅ Playable |
| 🏓 Pong | ✅ Playable |
| 🃏 Memory Match | ✅ Playable |
| 🧱 Breakout | 🔜 Coming Soon |
| 👾 Pac-Man | 🔜 Coming Soon |
| 🔵 Connect Four | 🔜 Coming Soon |
| 📝 Wordle Clone | 🔜 Coming Soon |

## Running Locally

Just open `index.html` in any modern browser. No server, no build step, no dependencies.

```
# Clone and open
git clone <your-repo-url>
# Then open index.html in your browser
```

## Deployment

Deploys automatically to Azure Static Web Apps via GitHub Actions on every push to `main`.

### Setup (one-time)

1. Create an **Azure Static Web Apps** resource in the Azure portal
2. Copy the **deployment token** from the resource
3. Add it as a GitHub secret: `AZURE_STATIC_WEB_APPS_API_TOKEN`
4. Push to `main` — GitHub Actions handles the rest

See [CLAUDE.md](CLAUDE.md) for detailed setup instructions.

## Tech Stack

- Pure HTML5, CSS3, Vanilla JavaScript (ES6+)
- No frameworks, no build tools, no npm
- `localStorage` for persistent high scores
- Responsive design — works on desktop and mobile
- Runs fully offline after initial load
