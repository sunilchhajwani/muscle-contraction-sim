# 🧬 Sarcomere Contraction Simulator

Interactive 3D visualization of the **sliding filament theory** of muscle contraction, built with Three.js. Shows real-time cross-bridge cycling, calcium dynamics, and sarcomere shortening at the molecular level.

## Live Demo

Open `index.html` in any modern browser. No server, no dependencies — just double-click.

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

## What It Shows

A single sarcomere (Z-line to Z-line) rendered in 3D with all major contractile components:

| Component | Visual | Role |
|-----------|--------|------|
| **Z-lines** | Dense vertical plates | Sarcomere boundaries, anchor thin filaments |
| **Thick filaments (myosin)** | Blue rods at center | Motor proteins, contain myosin heads |
| **Thin filaments (actin)** | Green strands | Slide toward M-line during contraction |
| **Myosin heads** | Orange spheres on myosin | Cross-bridge cycling (attach → pivot → detach) |
| **Tropomyosin** | Red ribbons on actin | Blocks binding sites; shifts on Ca²⁺ binding |
| **Calcium ions (Ca²⁺)** | Yellow glowing particles | Released from SR, bind troponin to expose sites |
| **Sarcoplasmic reticulum** | Purple torus below | Stores/releases/reuptakes Ca²⁺ |
| **M-line** | Central purple plate | Anchors thick filaments |

## Interactive Controls

| Control | What It Does |
|---------|-------------|
| **Stimulation slider** (0–100%) | Fires action potential → triggers Ca²⁺ release from SR |
| **ATP toggle** (Normal/Depleted) | Normal = cycling heads; Depleted = rigor (heads stuck) |
| **Sim speed** (0×–3×) | Slow-motion for teaching vs real-time |
| **Play/Pause/Reset** | Loop control + full reset |
| **Camera orbit** | Click-drag to rotate 3D scene |
| **Scroll zoom** | Zoom in/out |
| **Labels toggle** | Show/hide anatomical labels |

## Physiological Phases

The simulator cycles through 6 phases based on stimulation and calcium dynamics:

```
RESTING → EXCITATION → Ca²⁺ RELEASE → CROSS-BRIDGE CYCLING → CONTRACTION → RELAXATION
```

1. **Resting** — Tropomyosin blocks myosin-binding sites on actin. Myosin heads cocked (ADP+Pi). Low Ca²⁺ (~0.1 µmol/L).
2. **Excitation** — Action potential arrives at the muscle fiber, depolarization propagates.
3. **Ca²⁺ Release** — SR releases calcium ions. [Ca²⁺] rises from 0.1 → ~10 µmol/L. Ions bind troponin C, which shifts tropomyosin away from binding sites.
4. **Cross-Bridge Cycling** — Myosin heads attach to exposed actin sites → power stroke (Pi release, head pivots ~45°) → ADP release → ATP binds → heads detach → ATP hydrolysis re-cocks heads.
5. **Contraction** — Net effect of many cross-bridge cycles: thin filaments slide toward M-line. Sarcomere shortens from 2.2 µm → ~1.6 µm.
6. **Relaxation** — Stimulation stops. SERCA pump returns Ca²⁺ to SR. Tropomyosin re-blocks sites. Heads detach.

## Cross-Bridge Cycle (5 States)

```
① DETACHED (cocked, ADP+Pi)
    ↓  Ca²⁺ exposes binding site
② ATTACHED (bound to actin)
    ↓  Pi release triggers pivot
③ POWER STROKE (head pivots, pulls actin)
    ↓  ADP released, ATP binds
④ DETACHING (ATP-bound, releasing actin)
    ↓  ATP hydrolysis (ADP+Pi)
⑤ RECOCKING (head returns to cocked position)
    ↓  → back to ①
```

**Rigor state**: If ATP is depleted, heads remain stuck in the post-power-stroke position (cannot detach without ATP). This is why muscles stiffen after death (rigor mortis).

## HUD Readouts

- **Sarcomere length** (µm) — live, shortens during contraction
- **[Ca²⁺]** (µmol/L) — calcium concentration
- **Force** (mN) — proportional to active cross-bridges
- **Phase indicator** — color-coded current phase
- **Force trace** — scrolling graph of force over time
- **Cross-bridge count** — how many heads currently attached
- **ATP consumed** — running total
- **Shortening %** — sarcomere shortening percentage

## Architecture

See `schematic.html` for a full architecture diagram showing the 5-layer design:

1. **User Layer** — Start button, sidebar controls, mouse/scroll input
2. **Controls Layer** — Stimulation, ATP, speed, playback, view toggles
3. **Physiology Engine** — Ca²⁺ dynamics, cross-bridge FSM, phase machine, sarcomere math, rigor logic
4. **3D Scene Layer** — Z-lines, thick/thin filaments, myosin heads, Ca²⁺ particles, SR, tropomyosin
5. **Output Layer** — HUD panel, phase display, force trace, stats, tips, WebGL render

## Tech Stack

- **Three.js r128** (CDN-loaded) — WebGL 3D rendering
- **Web Audio API** — (not used in this version)
- **Canvas 2D** — Force trace graph
- **HTML/CSS** — HUD overlay, sidebar, start screen
- **Single file** — No build step, no npm, no server

## How to Use (Teaching)

1. Open the simulator, click **Start**
2. Drag the **Stimulation slider** to ~50% — watch Ca²⁺ ions release from SR
3. Observe tropomyosin shifting (red ribbons moving) as Ca²⁺ binds troponin
4. Watch myosin heads (orange → green → yellow → cyan) cycle through cross-bridge states
5. See the sarcomere shorten (Z-lines move inward, length drops from 2.2 → 1.6 µm)
6. Watch the force trace build up as more cross-bridges attach
7. Set stimulation to 0 — watch relaxation (Ca²⁺ returns to SR, heads detach)
8. Toggle **ATP → Depleted** to see rigor (heads stuck, sarcomere locked)

## References

- Guyton AC, Hall JE. *Textbook of Medical Physiology*, 14th ed. Chapters 6–7.
- Huxley AF, Hanson J. "Changes in the cross-striations of muscle during contraction and stretch and their structural interpretation." *Nature* 173:973–976, 1954.
- Huxley HE. "The mechanism of muscular contraction." *Science* 164:1356–1366, 1969.

## Files

| File | Description |
|------|-------------|
| `index.html` | The 3D simulator (open in browser) |
| `schematic.html` | Architecture diagram of the simulator |
| `README.md` | This file |

## License

MIT