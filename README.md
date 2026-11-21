# ⚛ Quantum Air Hockey

> Two-player educational arcade game combining classic air hockey with real 2-qubit quantum mechanics simulation — playable in any browser, no install required.

**[Play now →](https://pbworker.github.io/q-airhockey/)** · Single `index.html` file · Touch + Gamepad + Keyboard

---

## What is it?

Quantum Air Hockey is a conference-ready arcade game built on a 43" touch table (or any browser). Players compete in classic air hockey while applying quantum gates that change the puck's physical behaviour. The quantum engine runs a real 2-qubit state vector simulation — superposition, entanglement, and Bell states aren't just visual effects, they drive the gameplay.

---

## Modes

| Mode | Description |
|---|---|
| **Classic** | Pure air hockey — no quantum mechanics |
| **⚛ Quantum** | Full quantum mode with gate cards, energy system, circuit display, and Q-BOT |

---

## Quantum Gates

| Button | Gate | In-game effect | Energy cost |
|---|---|---|---|
| **H** | Hadamard | **SPLIT** — two identical pucks appear; only one is physically real (50/50, hidden). Ghost bounces off paddles. After 6 s or on goal the fake dissolves. | 20 ⚡ |
| **X** | Pauli-X | **REVERSE** — flips the puck's velocity direction | 10 ⚡ |
| **Z** | Pauli-Z | **PHASE** — adds a lateral kick, curving the trajectory | 10 ⚡ |
| **CNOT** | Controlled-NOT | **ENTANGLED** — two pucks travel perpendicular paths, both equally likely to be real | 35 ⚡ |
| **H → CNOT** | Bell state Φ⁺ | **BELL STATE** — creates `(|00⟩+|11⟩)/√2`. Both pucks are independent physical threats heading toward opposite goals. They pulse in-phase (correlated). Measuring (scoring) collapses both instantly. | 55 ⚡ combined |

### Bell State Φ⁺

Applying **H** on one qubit followed by **CNOT** produces the maximally entangled Bell state:

```
|Φ⁺⟩ = (|00⟩ + |11⟩) / √2
```

In-game: both pucks move independently across the full table and can each score a goal. When either puck is measured (crosses a goal line), the entanglement collapses — Q-BOT narrates the result.

---

## Controls

| Player | Move paddle | Gate shortcuts |
|---|---|---|
| P1 | WASD or left stick (Gamepad 1) | Q=H · E=X · R=Z · F=CNOT |
| P2 | Arrow keys or left stick (Gamepad 2) | U=H · O=X · P=Z · [=CNOT |
| Both | Touch (each player's half of screen) | Tap gate buttons on the bar |

Gamepad face buttons (A/B/X/Y) also fire H/X/Z/CNOT.

---

## Energy System

Both players start with **100 ⚡** per match.

| Event | Energy gained |
|---|---|
| Paddle hit | +10 ⚡ to hitting player |
| Wall bounce | +2.5 ⚡ each player |
| Long rally (every 5 hits) | +15 ⚡ each player |

---

## Physics

- **120 Hz** fixed-step physics loop, decoupled from 60 FPS rendering
- Puck interpolated between ticks for smooth visuals at any frame rate
- **Vector-impulse collision** — moving paddle slams add real speed to the puck
- **Swept collision** goal detection — no missed goals at high speed
- Max puck speed: 7 m/s · Max paddle speed: 1.8 m/s
- Both paddles constrained to their own half

---

## Quantum Engine

Pure JavaScript 2-qubit simulator inside `index.html`:

- State vector `[re₀₀, im₀₀, re₀₁, im₀₁, re₁₀, im₁₀, re₁₁, im₁₁]`
- Unitary matrix application via column-based tensor product
- Exact CNOT: swaps amplitudes |10⟩ ↔ |11⟩
- Born-rule measurement with wavefunction collapse
- Physics and quantum engines communicate only through game events — fully decoupled

---

## Running

**Browser (no server)**
```
open index.html
```

**Kiosk mode (43" touch table, conference)**
```
chrome.exe --kiosk file:///path/to/index.html
```

**GitHub Pages**
Enable Pages on the `main` branch root — the game is immediately playable at `https://pbworker.github.io/q-airhockey/`.

---

## Hardware Spec (conference table)

| Component | Spec |
|---|---|
| Display | 43" 16:9 PCAP touchscreen, 10-point, 1080p/4K |
| Computer | Mini PC / Intel NUC, i5+, 16 GB RAM |
| Controls | 2× USB HID gamepads (XInput) + touch |
| Audio | Stereo 2×5W (Web Audio API, no files needed) |
| Power | Power bank 8h+ or mains 230V |

---

## Project Structure

```
index.html          — entire game (single file, ~65 KB, zero dependencies)
README.md
Quantum Air Hockey Concept Board.png   — original design mockup
```

---

## Roadmap

- [ ] Phase 3 — Bell Experiment mode with CHSH score display
- [ ] Phase 4 — Decoherence model, noise simulation
- [ ] Phase 5 — 3-qubit extension, Toffoli gate
- [ ] Difficulty levels (Beginner → Quantum Master)
- [ ] Leaderboard / QR code score sharing

---

*Gra. Nauka. Przyszłość.* — Play. Learn. Future.
