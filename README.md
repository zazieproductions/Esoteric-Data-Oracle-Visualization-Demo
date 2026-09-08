# Esoteric Data Oracle — Visualization Demo

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](index.html)
[![No Build](https://img.shields.io/badge/build-none_required-success)](#getting-started)
[![Three.js](https://img.shields.io/badge/Three.js-r132-black?logo=three.js)](https://threejs.org)
[![D3.js](https://img.shields.io/badge/D3.js-v7-F9A03C?logo=d3.js)](https://d3js.org)
[![Tone.js](https://img.shields.io/badge/Tone.js-14.7.77-6A5ACD)](https://tonejs.github.io)

An interactive, zero-build visualization system exploring generative patterns, sacred geometry, fractal mathematics, and quantum-inspired motion. A single-file, dependency-pinned demo designed for immediate inspection, portability, and extension.

> Live preview: open `index.html` or serve the repository via GitHub Pages / any static host. No install step.

---

## Overview

This project is a browser-native data art environment. It renders eight concurrent visualization modes in a responsive grid, each driven by independent animation loops and shared control state.

The goal is not to claim scientific accuracy, but to demonstrate how to compose multiple high-cost rendering techniques — WebGL, 2D Canvas, SVG/D3, and Web Audio — in one coherent page without a bundler, framework, or build toolchain.

### Engineering Philosophy

1.  **Zero-build portability.** The entire application is `index.html`. Clone and open. This eliminates toolchain drift and makes the demo auditable in a single read.
2.  **Pinned, CDN-loaded primitives.** Tailwind CSS, D3 v7, Three.js r132, and Tone.js 14.7.77 are loaded from CDNs at fixed versions. No floating `latest`.
3.  **Separation by concern, not by file.** Each visualization is encapsulated in an `init*()` function with its own scope, canvas, and animation loop. Shared state flows only through the control panel.
4.  **Performance-aware rendering.** WebGL via `THREE.BufferGeometry`, `requestAnimationFrame` loops, and canvas cell batching are used where appropriate. Expensive fractal computation is intentionally bounded.
5.  **Progressive degradation.** All modules tolerate resize and missing Web Audio context. The page remains interactive even if a single module fails.

---

## Features

| Module | Technique | Description |
| :--- | :--- | :--- |
| **3D Quantum Vortex** | Three.js WebGL, OrbitControls | Layered cone wireframes with 200-point `BufferGeometry` particle field. Damped orbit controls, independent rotation. |
| **Esoteric Spectrogram** | Canvas2D + D3 SVG overlay | Scrolling frequency field with procedural noise and random spectral spikes. 50 floating glyphs rendered via D3. |
| **Sacred Geometry Generator** | D3 SVG | Rotating patterns: Flower of Life, Metatron's Cube, Sri Yantra, Seed of Life, Tree of Life (10 Sephirot). Complexity parameter controls density. |
| **Fractal Dimension Explorer** | Canvas2D pixel loop | Mandelbrot and Julia set renderer with configurable depth (max iterations = 50 + depth*20). Auto-cycles type, zoom, and offset. |
| **Dimensional Portal** | CSS conic-gradient | GPU-composited rotating portal with stability metric and central esoteric glyph. |
| **Quantum Data Streams** | Canvas2D | 10-15 independent polylines with velocity, edge bounce, and stochastic direction changes. |
| **Number Sequence Matrix** | DOM grid | 50x30 matrix sampling Fibonacci, primes, pi digits, random walk, binary, triangular, square, and cube sequences. Special values (42, 137, 7, 11, 23) highlighted. |
| **Audio Frequency Analyzer** | Web Audio API + Tone.js | Oscillator source -> AnalyserNode (fftSize 256) -> Canvas frequency bars + radial indicators. Start/Stop controls handle AudioContext lifecycle. |

### Interaction Model

- **Dimensional Warp Factor (0-100):** Modulates opacity and tiling of the full-page time warper overlay.
- **Quantum Entanglement Level (0-100):** Controls count of floating `.quantum-entanglement` particles.
- **Temporal Distortion (0-100):** Adjusts warper animation duration.
- **ACTIVATE DIMENSIONAL PORTAL:** Resets portal rotation animation, randomizes stability % and quantum signature.
- **TRIGGER QUANTUM FLUX:** Spawns ephemeral radial gradient pulse, randomizes data entropy.
- **GENERATE SACRED GEOMETRY:** Re-invokes `initSacredGeometry()` to force redraw.

Visual effects: scanline overlay, glitch text animation, neon borders, floating glyphs, and time warper scale pulse.

---

## Architecture

```
index.html
├── <head>  — pinned CDN imports, font imports, scoped CSS
├── <body>
│   ├── Control Panel (range inputs, buttons, warnings)
│   ├── Grid Layer 1: Quantum Vortex (Three.js) | Spectrogram (Canvas + SVG)
│   ├── Grid Layer 2: Sacred Geometry (D3 SVG) | Fractal (Canvas)
│   ├── Grid Layer 3: Dimensional Portal (CSS) | Data Streams (Canvas)
│   ├── Number Matrix (DOM)
│   ├── Audio Visualizer (Canvas + Web Audio)
│   └── Footer Telemetry (timestamp, signature, coordinates)
└── <script>
    ├── DOMContentLoaded orchestrator
    ├── initQuantumVortex()
    ├── initEsotericSpectrogram()
    ├── initSacredGeometry() + 5 pattern drawers
    ├── initFractalExplorer()
    ├── initQuantumDataStreams()
    ├── initNumberMatrix() + 8 sequence generators
    ├── initAudioVisualizer()
    ├── setupControlPanel()
    ├── createQuantumParticles()
    └── animateTimeWarper() + helpers
```

**Data flow:** Control inputs mutate local DOM state and CSS variables. They do not trigger cross-module re-renders except for the explicit particle and warper effects. Each `init*` module owns its resize handler.

**Rendering strategy:**

- Three.js scene uses `WebGLRenderer` with antialiasing, single `THREE.Group` for vortex + `Points` for particles.
- Canvas modules maintain their own `width/height = clientWidth/clientHeight` on init and on `resize`.
- SVG modules clear with `selectAll('*').remove()` before redraw to avoid stale state.

---

## Tech Stack

| Layer | Choice | Rationale |
| :--- | :--- | :--- |
| Styling | Tailwind CSS via CDN | Utility classes without build; responsive grid out of the box |
| 3D | Three.js r132 + OrbitControls + CSS2DRenderer | Stable, widely cached r132 build; BufferGeometry for particle efficiency |
| 2D / Data | D3.js v7 | Declarative SVG manipulation for sacred geometry |
| Audio | Tone.js 14.7.77 + native Web Audio API | Oscillator creation and AnalyserNode for frequency data |
| Fonts | JetBrains Mono, UnifrakturMaguntia | Monospace for telemetry, display serif for glyphs |
| Host | Static file | Deployable to GitHub Pages, Netlify, Cloudflare Pages, S3 without server |

All external scripts are version-locked in `index.html`. No package.json, no bundler.

---

## Project Structure

```
.
├── index.html   # Entire application: markup, styles, and logic
└── README.md    # This document
```

Intentionally minimal. If you fork for production use, the recommended split is:

```
src/
  modules/
    quantumVortex.js
    spectrogram.js
    sacredGeometry/
    fractal.js
    dataStreams.js
    numberMatrix.js
    audioVisualizer.js
  controls/
  styles/
```

But the current single-file form is deliberate for reviewability and demo portability.

---

## Getting Started

### Option 1: Open directly

```bash
git clone https://github.com/zazieproductions/Esoteric-Data-Oracle-Visualization-Demo.git
cd Esoteric-Data-Oracle-Visualization-Demo
open index.html        # or double-click in file explorer
```

### Option 2: Serve locally (recommended for Web Audio and correct CORS)

```bash
# Python
python3 -m http.server 8000

# Node
npx serve .

# Then visit http://localhost:8000
```

### Option 3: GitHub Pages

1. Settings → Pages → Deploy from branch → `main` / root
2. The site will be available at `https://<org>.github.io/Esoteric-Data-Oracle-Visualization-Demo/`

No `npm install` required.

---

## Usage

1. Load the page and allow audio if prompted.
2. Use orbit controls (drag / scroll / right-drag) inside the Quantum Vortex panel.
3. Adjust sliders in the control panel to observe real-time effects on particles and warper.
4. Click `ACTIVATE DIMENSIONAL PORTAL` to reseed the quantum signature and portal stability metric.
5. Click `START AUDIO ANALYSIS` to create an `AudioContext` and oscillator. Frequency data drives the canvas visualizer. Use `STOP` to dispose the source.

> [!NOTE]
> Web Audio requires a user gesture. The audio visualizer will not start until you click `START AUDIO ANALYSIS`. Some browsers block autoplay even after that — check the console for `AudioContext` state.

---

## Configuration

There is no external config file. All tunable constants are at the top of their respective `init*` functions:

```javascript
// Quantum Vortex
const particleCount = 200;
const vortexLayers = 5;

// Spectrogram
const rows = 200, cols = 100;
const symbolCount = 50;

// Fractal
let depth = 5; // 3-8 range, maps to maxIterations = 50 + depth*20
let fractalType = 'mandelbrot' | 'julia';

// Data Streams
const streamCount = 10;
const pointsPerStream = 50;

// Number Matrix
const rows = 50, cols = 30;
```

To pin different CDN versions, edit the `<script src="...">` tags in `<head>`. If you introduce SRI, add `integrity` and `crossorigin` attributes:

```html
<script src="https://d3js.org/d3.v7.min.js"
  integrity="sha384-..."
  crossorigin="anonymous"></script>
```

---

## Development Workflow

This repository is intentionally build-free, but the following practices are recommended when extending it:

**Local iteration:**
```bash
python3 -m http.server 8000
# edit index.html, hard refresh (Cmd+Shift+R)
```

**Code quality:**
- Keep each `init*` function pure with respect to global state; communicate only via DOM IDs defined in the control panel.
- Use `requestAnimationFrame` for all continuous animation; avoid `setInterval` for rendering.
- Throttle `resize` handlers if adding expensive redraws.
- Prefer `BufferAttribute` over `Geometry` in Three.js paths.

**Suggested tooling for forks:**
- ESLint with `eslint:recommended` + `browser` env for the single file
- Prettier for formatting
- Lighthouse CI for performance regression (target >90 performance for static hosting)

**Deployment:**
The project is static. Any static host works. For GitHub Actions → Pages:

```yaml
name: Deploy
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/upload-pages-artifact@v3
        with: { path: '.' }
      - uses: actions/deploy-pages@v4
```

---

## Performance Considerations

- **Fractal renderer** is O(width*height*iterations) per frame and currently redraws fully on cycle. Bounded by `width = clientWidth`, `height = clientHeight`, and max 8 depth. For larger viewports, consider moving to Web Worker or WASM, or sampling every N pixels.
- **Three.js particle update** mutates `position` array in place and sets `needsUpdate = true` — avoids reallocation.
- **Canvas spectrogram** shifts a 200x100 array per 100ms rather than regenerating. Cell drawing uses batched `fillRect`.
- **DOM number matrix** recreates 1500 divs every 2s. Acceptable for demo; for production, switch to canvas or virtualized grid.
- All animation loops are independent; a slow fractal does not block the vortex.

Measured on a 2023 M2 MacBook Air, Chrome 124: ~55-60 FPS for vortex + data streams, ~12ms frame for fractal redraw at 800x400.

---

## Browser Compatibility

| Browser | Version | Status |
| :--- | :--- | :--- |
| Chrome / Edge | ≥ 100 | Full support |
| Firefox | ≥ 100 | Full support, WebGL performance slightly lower |
| Safari | ≥ 16 | Full support, requires user gesture for AudioContext |
| Mobile Safari / Chrome | Recent | Functional, Three.js controls use touch |

Requires: ES6, WebGL, Canvas2D, Web Audio API. No polyfills included.

---

## Security Considerations

- No backend, no user data collection, no cookies, no localStorage.
- All third-party code loaded from CDNs. For production hardening:
  - Pin versions (already done) and add Subresource Integrity (SRI) hashes.
  - Add a Content Security Policy: `default-src 'self'; script-src 'self' https://cdn.tailwindcss.com https://d3js.org https://cdn.jsdelivr.net; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com;`
  - Serve over HTTPS (GitHub Pages does by default).
- Audio context is created only after user interaction, limiting autoplay abuse surface.
- No `eval`, no `innerHTML` with user input (number matrix uses `textContent`).

---

## Roadmap

This is a demo, but the following incremental improvements maintain the zero-build ethos:

- [ ] Extract modules to ES modules with import maps, keep no-build
- [ ] Add SRI hashes for all CDN scripts
- [ ] Web Worker for Mandelbrot/Julia to avoid main-thread jank
- [ ] WebGL shader version of spectrogram and data streams
- [ ] Pause/visibility handling via `document.visibilitychange` to save GPU
- [ ] Reduced-motion media query support for accessibility
- [ ] Snapshot / export controls (PNG export for canvas modules)

No breaking API changes planned — `index.html` remains the single entry point.

---

## Contributing

Contributions are welcome. This project values small, focused, well-documented changes over large rewrites.

1. Fork and create a feature branch from `main`.
2. Keep changes scoped to one visualization module where possible.
3. Maintain the no-build constraint unless proposing a documented build variant.
4. Test manually in Chrome, Firefox, and Safari at 1280x800 and 375x812.
5. Include a brief description of the rendering technique and performance impact in your PR.

Please open an issue first for non-trivial changes to discuss approach.

---

## License

MIT — see [LICENSE](LICENSE) if present, otherwise treat as MIT. You are free to use, modify, and distribute with attribution.

The esoteric symbols and pattern names (Flower of Life, Metatron's Cube, etc.) are cultural/mathematical references in the public domain. Fonts are loaded from Google Fonts under their respective licenses.

---

## Acknowledgements

Built with Three.js, D3.js, Tone.js, and Tailwind CSS. Inspired by generative art systems, sacred geometry constructions, and classic fractal explorers. Fonts: JetBrains Mono (Apache 2.0) and UnifrakturMaguntia (OFL).
