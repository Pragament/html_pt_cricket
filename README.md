# 🏏 Cricket Match Scorekeeper

A lightweight, client‑side web application for scoring cricket matches in real time. It provides a quick‑entry interface, automatic local storage, and one‑click export of match results.

---

## ✨ Features

- **Quick scoring** – one‑tap buttons for runs (0–6), extras (wide, no‑ball, bye, leg‑bye), and wickets.
- **Live scoreboard** – shows runs, wickets, overs, batting/bowling status, and active batsmen (striker/non‑striker).
- **Bowler figures** – displays overs, runs, wickets, and economy rate for each bowler.
- **Fall of wickets** – chronological list with batsman name and score at dismissal.
- **Match setup** – configure team names, toss winner, and batting decision.
- **Persistent storage** – all data is automatically saved to `localStorage`; you can close the browser and resume later.
- **Export results** – download a plain‑text scorecard summary as a `.txt` file.
- **Responsive** – works on desktop, tablet, and mobile devices.
- **No server or database required** – runs entirely in the browser.

---

## 🖥️ How to Use (User Guide)

### Getting Started
1. Open the `index.html` file in any modern web browser (Chrome, Firefox, Edge, Safari).
2. The app loads with default team names (“India” and “Australia”). You can change them.
3. Choose the toss winner and what they elected to do (bat or bowl).
4. Click **“Start Match”** – the match begins, and the entry panel becomes active.

### Scoring a Ball
- The **entry panel** shows the current over and ball count.
- Click a run button (0–6) to record runs scored off the bat.
- Use the extra buttons:
  - **W** – Wide
  - **NB** – No‑ball
  - **B** – Bye
  - **LB** – Leg‑bye
- Click **“✕ Wkt”** to record a wicket. The current striker is marked out, and the next batsman (not out) automatically takes strike.
- The **“⏭ Over”** button forces the current over to end (useful if you need to manually close an over).
- **“↩ Undo”** is a placeholder for future implementation – it does not yet reverse actions (use the reset button to start over).

### Managing the Match
- The scoreboard updates instantly. The batting team is highlighted.
- Active batsmen are shown below the score. The striker is marked with 🏏, the non‑striker with 🔄.
- Bowling figures appear in a table below the entry panel.
- Fall of wickets are listed at the bottom.

### Resetting & Exporting
- Click **“Reset”** to clear all data and return to the setup state (confirmation required).
- Click **“Export”** to download a `cricket_scorecard_YYYY‑MM‑DD.txt` file containing a complete summary of the match.

> 💡 **Tip:** All data is automatically saved to your browser’s local storage – you can refresh the page or close the browser and your match will be restored.

---

## 📦 Installation & Setup

This is a single‑page application with no dependencies. To run it:

1. Download the `index.html` file (provided in this repository).
2. Double‑click to open it in your browser.
3. That’s it – no installation, no server, no internet required.

---

## 🛠️ Developer Guide

### Tech Stack
- **HTML5** – structure
- **CSS3** – custom styling (no frameworks)
- **Vanilla JavaScript** – all logic in a single file
- **localStorage** – persistent data storage

### File Structure
```
index.html          # Complete application (HTML + CSS + JS)
README.md           # This file
```

### Data Model
The entire match state is stored in a JavaScript object (`match`) and serialized to `localStorage`. Below is the core structure:

```javascript
match = {
  team1: {
    name: string,
    runs: number,
    wickets: number,
    overs: number,
    ballsInOver: number,
    extras: number,
    batsmen: [ { name, runs, balls, fours, sixes, out, outReason } ],
    bowlers: [ { name, overs, balls, runs, wickets, extras } ],
    fow: [ { runs, batsman, over } ]
  },
  team2: { /* same structure */ },
  batting: 'team1' | 'team2',
  bowling: 'team1' | 'team2',
  status: 'setup' | 'ongoing' | 'completed',
  currentOver: { balls, runs, wickets },
  overNumber: number,
  ballNumber: number,
  totalBalls: number,
  strikerIdx: number,
  nonStrikerIdx: number,
  bowlerIdx: number,
  toss: { winner: 'team1' | 'team2', choice: 'bat' | 'bowl' },
  innings: number,
  maxOvers: number
}
```

### Key Functions
| Function | Description |
|----------|-------------|
| `startMatch()` | Initialises the match based on setup inputs. |
| `recordBall(runs)` | Records runs off the bat; updates batsman, team, bowler, and current over. |
| `recordExtra(type)` | Handles wides, no‑balls, byes, and leg‑byes. |
| `recordWicket()` | Marks the striker out, awards wicket to bowler, and brings in the next batsman. |
| `completeOver()` | Finalises the current over, rotates strike, and changes the bowler. |
| `swapStrike()` | Swaps striker and non‑striker indices. |
| `getBattingTeam()` / `getBowlingTeam()` | Helper to get the current batting/bowling team object. |
| `renderAll()` | Updates the entire UI from the `match` object. |
| `saveState()` | Serializes `match` to `localStorage`. |
| `exportResults()` | Generates a text scorecard and triggers a download. |

### Extending the App
- **Add new scoring types** – extend the `recordExtra` function and add buttons to the entry grid.
- **Support multiple innings** – modify the data model to include innings arrays and adjust the logic for innings transitions.
- **Improve undo** – implement a proper action history stack (currently a stub).
- **Add bowling changes** – allow the user to select which bowler bowls the next over via a dropdown.

### Building / Testing
No build tools are required. Simply edit the `index.html` file and refresh the browser to test changes. For larger modifications, consider extracting CSS and JS into separate files.

---

## 🤝 Contributing
Contributions are welcome! If you find a bug or have an idea for an enhancement, please open an issue or submit a pull request.
