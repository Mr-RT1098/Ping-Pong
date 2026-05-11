# 🏓 Ping Pong

A sleek, browser-based Ping Pong game built entirely in a single HTML file — no frameworks, no libraries, just vanilla HTML5, CSS3, and JavaScript.

**[▶ Play it here](https://mr-rt1098.github.io/Ping-Pong/)**

---

## What It Is

A fully playable Ping Pong game that runs right in your browser. You play against a CPU opponent with three difficulty levels. First player to reach 7 points wins.

The game features a dark cyberpunk aesthetic with glowing paddles, particle burst effects on every hit, a motion trail on the ball, and real-time sound effects — all rendered on an HTML5 Canvas with zero external dependencies.

---

## Features

- **3 Difficulty Levels** — Easy, Medium, and Hard. The AI gets progressively smarter, using ball trajectory prediction on higher difficulties.
- **Smooth Controls** — Move your paddle using the mouse, touch (mobile-friendly), or keyboard (W / S / Arrow keys).
- **Sound Effects** — Web Audio API generates paddle hits, wall bounces, and score sounds on the fly — no audio files needed.
- **Particle Effects** — Each paddle hit triggers a color-coded burst of particles.
- **Ball Trail** — The ball leaves a fading cyan trail as it moves.
- **Animated Score Display** — The score pulses and scales on each point.
- **Responsive Layout** — Canvas size adapts to the screen width.

---

## How to Play

| Action | Control |
|---|---|
| Move paddle up | `W` or `↑` |
| Move paddle down | `S` or `↓` |
| Move paddle (mouse) | Move cursor over the canvas |
| Move paddle (mobile) | Touch and drag on the canvas |
| Start / Restart | Click **Play** |

First to **7 points** wins the match.

---

## Tech Stack

| Technology | Usage |
|---|---|
| HTML5 Canvas | Game rendering (paddles, ball, net, particles) |
| Vanilla JavaScript | Game loop, physics, AI logic, input handling |
| CSS3 | Layout, overlay, button styling, Orbitron font |
| Web Audio API | Procedural sound effects (no audio files) |
| Google Fonts | Orbitron — futuristic display font |

---

## Project Structure

```
Ping-Pong/
└── Yo.html    ← The entire game in a single file
```

Everything — HTML structure, CSS styling, game logic, AI, audio, and rendering — lives inside one self-contained file.

---

## How the AI Works

The CPU paddle uses a simple physics-based prediction system:

- **Easy** — The AI reacts slowly and aims at where the ball currently is.
- **Medium** — The AI predicts where the ball will land on its side of the court, accounting for wall bounces, but with a small random error margin.
- **Hard** — Same prediction, but with higher movement speed and a tighter error margin, making it very accurate.

The AI error is randomized on each rally, so even Hard mode isn't perfect — it can be beaten with angled shots.

---

## Local Setup

No installation needed. Just open the file in any modern browser:

```bash
git clone https://github.com/Mr-RT1098/Ping-Pong.git
cd Ping-Pong
# Open Yo.html in your browser
```

Or visit the live link at the top of this page.

---

## Author

**RT** — [github.com/Mr-RT1098](https://github.com/Mr-RT1098)
