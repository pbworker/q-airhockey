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
| **⚛ Quantum** | Full quantum mode with gate cards, energy system, circuit display, Q-BOT, and gate stacking |

---

## Quantum Gates

| Button | Gate | First-time effect | Energy cost |
|---|---|---|---|
| **H** | Hadamard | **SPLIT** — puck becomes two identical copies; only one is physically real (50/50, hidden). Ghost bounces off paddles. Auto-collapses after 6 s or on goal. | 20 ⚡ |
| **X** | Pauli-X | **REVERSE** — flips the puck's velocity direction | 10 ⚡ |
| **Z** | Pauli-Z | **PHASE** — adds a lateral kick, curving the trajectory | 10 ⚡ |
| **CNOT** | Controlled-NOT | **ENTANGLED** — two pucks travel perpendicular paths, both equally likely real. Auto-collapses after 9 s. | 35 ⚡ |
| **H → CNOT** | Bell state Φ⁺ | **BELL** — ghost teleports to the opposite half heading toward the other goal. Two independent scoring threats. Collapses after 12 s or on goal. | 55 ⚡ combined |
| **CCX** | Toffoli (3-qubit) | **GHZ** — requires H → CNOT → CCX sequence. Zeros opponent score + locks their gates 10 s. | 30 ⚡ each |

### Bell State Φ⁺

Applying **H** on one qubit followed by **CNOT** produces the maximally entangled Bell state:

```
|Φ⁺⟩ = (|00⟩ + |11⟩) / √2
```

Both pucks move independently across the full table and can each score. When either puck enters a goal, the entanglement collapses — Q-BOT narrates the result.

### GHZ State (3-qubit)

The most powerful quantum state in the game. Requires a 3-step sequence:

```
H → CNOT → CCX  =  (|000⟩ + |111⟩) / √2
```

**CCX (Toffoli)** costs 30 ⚡ from **each** player. Either player can press the CCX button, but both must have ≥ 30 ⚡. The gate has two effects that always apply:

#### Toffoli mirror-flip (always active after CCX)

CCX implements the real quantum gate: flip target Q2 when both controls Q0 and Q1 are |1⟩. In-game that means:

| Event | Quantum interpretation | Physical effect |
|---|---|---|
| Paddle hits **real puck** | Q0/Q1 measured \|1⟩ → CCX fires → Q2 flips | **Ghost's vx reverses** instantly — gold burst at ghost |
| Paddle hits **ghost puck** | Q2 observed \|1⟩ → feeds back | **Real puck's vx reverses** — gold burst at real puck |

#### GHZ bonus (requires H → CNOT → CCX sequence)

When fired after a valid H → CNOT in the recent gate log **and** puck is in a quantum state:

- Opponent's score is **reset to 0**
- Opponent's gate buttons are **locked for 10 seconds**
- Gold particle burst + GHZ overlay + Q-BOT announcement

**CCX without the full sequence** still activates the Toffoli mirror-flip — useful on its own.

**Counter-play:** Fire **X** to flip your qubit back to |0⟩, breaking the entangled state before the opponent completes the sequence.

---

## Gate Stacking

Firing a gate while the puck is already in a quantum state **evolves** that state rather than resetting it. Both players can stack gates — this creates a full quantum circuit on the puck in real time.

### Upgrade paths (state gets stronger)

| Current state | Gate fired | Result |
|---|---|---|
| SPLIT | CNOT | → **BELL** — ghost teleports to opposite half |

### Modify paths (state changes behaviour, stays same level)

| Current state | Gate fired | Effect |
|---|---|---|
| SPLIT | X | Both copies reverse direction |
| SPLIT | Z | Both copies curve simultaneously |
| SPLIT | H | **Quantum erasure** — which is real re-randomised, ghost angles re-spread |
| ENTANGLED | X | Both perpendicular pucks reverse |
| ENTANGLED | Z | Both curved |
| ENTANGLED | CNOT | Hidden variable re-rolled — was-real may no longer be |
| BELL | X | Both Bell partners reverse |
| BELL | Z | Both Bell partners curve |
| BELL | CNOT | Correlated pair re-measured, realIs re-rolled |

### Downgrade paths (opponent disruption)

| Current state | Gate fired | Result |
|---|---|---|
| ENTANGLED | H | → **SPLIT** — decohere to simpler state, ghost direction re-randomised |
| BELL | H | → **ENTANGLED** — entanglement weakened, ghost snaps back nearby |

---

## Controls

| Player | Move paddle | Gate shortcuts |
|---|---|---|
| P1 | WASD or left stick (Gamepad 1) | Q=H · E=X · R=Z · F=CNOT · G=CCX |
| P2 | Arrow keys or left stick (Gamepad 2) | U=H · O=X · P=Z · [=CNOT · ]=CCX |
| Both | Touch (each player's half of screen) | Tap gate buttons on the bar |

Gamepad face buttons (A/B/X/Y) fire H/X/Z/CNOT. Either player's gamepad can fire CCX.

**Win screen:** Space / Enter / any gamepad button = instant rematch.

---

## Energy System

Both players start with **100 ⚡** per match. Energy also controls **paddle speed** in quantum mode — a depleted player moves at 40% speed.

### Gaining energy

| Event | Energy gained |
|---|---|
| Paddle hit | +10 ⚡ to hitting player |
| Wall bounce | +2.5 ⚡ each player |
| Long rally (every 5 hits) | +15 ⚡ each player |
| Goal scored | +25 ⚡ to scorer |
| Goal conceded | +15 ⚡ to loser (comeback mechanic) |
| Passive regen | +1 ⚡/s to both players |

### Paddle speed vs energy

| Energy | Paddle speed |
|---|---|
| 100 ⚡ | 100% (full speed) |
| 50 ⚡ | 70% |
| 0 ⚡ | 40% (minimum) |

The energy bar colour signals speed: **cyan** (full) → **orange** (low) → **red** (critical).

---

## Collapse Rules

Quantum states collapse (revealing which puck was real) only on:

1. **Goal scored** — the puck enters a goal mouth
2. **Timer expiry** — SPLIT 6 s · ENTANGLED 9 s · BELL 12 s

Paddle hits do **not** collapse the state — the ghost bounces physically off paddles without being measured.

---

## Physics

- **120 Hz** fixed-step physics loop, decoupled from 60 FPS rendering
- Puck interpolated between ticks for smooth visuals at any frame rate
- **Vector-impulse collision** — moving paddle slams add real speed to the puck
- **Swept collision** goal detection — no missed goals at high speed
- Max puck speed: 7 m/s · Paddle speed scales 0.4–1.0× with energy
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
index.html          — entire game (single file, ~80 KB, zero dependencies)
README.md
Quantum Air Hockey Concept Board.png   — original design mockup
```

---

## Roadmap

- [x] Phase 1 — Core air hockey (120 Hz physics, touch/gamepad/keyboard)
- [x] Phase 2 — 2-qubit quantum engine (H/X/Z/CNOT, SPLIT/ENTANGLED/BELL, energy system)
- [x] Phase 3 — Gate stacking, GHZ state, CCX gate, energy healing, paddle speed scaling, how-to screen, quick rematch
- [ ] Phase 4 — CHSH/Bell experiment score display
- [ ] Phase 5 — Decoherence timer bar (visible quantum noise)
- [ ] Phase 6 — Attract/idle loop for unattended kiosk

---

*Gra. Nauka. Przyszłość.* — Play. Learn. Future.
