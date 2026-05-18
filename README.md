# Graph Visualizer

An interactive, browser-based tool for constructing and analyzing planar graphs. Built to support research in **graph realization and outerplanarity hierarchies** at the CUNY Graduate Center.

**Live tool:** [https://xchyn.github.io/graph-visualizer/](https://xchyn.github.io/graph-visualizer/)

---

## Features

- **Vertex placement** — click the canvas to add labeled vertices (v1, v2, v3, ...)
- **Edge construction** — click two vertices to connect them; duplicate edges are prevented
- **Drag-to-move** — reposition vertices freely; edges follow automatically
- **Edge bending** — curve any edge with a draggable on-curve control point (quadratic Bézier); double-click a handle to reset it straight
- **Delete mode** — remove individual vertices or edges by clicking them
- **Sequential relabeling** — vertex labels stay contiguous (v1, v2, ...) after deletions
- **Live degree sequence** — displayed in condensed notation, e.g. `(6^5, 4^2)`, updated in real time
- **Export data** — dumps vertex list, edge list, condensed degree sequence, and full adjacency list as copyable text
- **Export TikZ/LaTeX** — generates a complete, compilable LaTeX document (`\documentclass`, `\usepackage{tikz}`, `\begin{document}`, `figure` environment with caption) containing the graph as a `tikzpicture`; includes vertex coordinates, edges with bend angles for curved edges, vertex labels, and a caption with the degree sequence — save as `.tex` and compile directly with `pdflatex`
- **Export image** — saves the current drawing as a high-resolution PNG (2x scale) with vertex labels, degree annotations, and centered degree sequence header; always exported in light theme for print/paper readability
- **Grid snapping** — toggle a configurable grid overlay; vertices snap to intersections when placed or dragged
- **Select mode** — click vertices to toggle selection (highlighted in gold); use with cycle edge tool below
- **Add cycle edges** — connects consecutive selected vertices in circular order (by angle from centroid) with straight edges; existing edges are never duplicated or removed
- **System theme support** — UI follows the operating system's light/dark mode preference and updates live on theme change
- **Keyboard shortcuts** — keys 1–5 switch modes; Escape cancels in-progress actions

## Usage

Open `index.html` in any modern browser. No build step, no dependencies, no installation.

### Modes

| Key | Mode | Action |
|-----|------|--------|
| 1 | + Vertex | Click canvas to place a vertex |
| 2 | + Edge | Click two vertices to connect them |
| 3 | Move | Drag vertices to reposition |
| 4 | Bend | Drag midpoint handles to curve edges |
| 5 | Delete | Click a vertex or edge to remove it |
| 6 | Select | Click vertices to select/deselect for circle layout |

### Toolbar buttons

- **Export data** — opens a text box with the full graph description (vertices, edges, degree sequence, adjacency list)
- **Export TikZ** — generates a complete compilable LaTeX document with the graph as a TikZ figure
- **Export image** — downloads the current graph as `graph.png`
- **Clear** — removes all vertices and edges

### Grid and circle layout controls

- **Grid snap** checkbox — toggles grid overlay and snapping; vertices align to intersections when placed or dragged
- **Size** — grid spacing in pixels (default: 30)
- **Select all / Deselect** — select or deselect all vertices at once
- **Add cycle edges** — adds straight edges between consecutive selected vertices in circular order (existing edges preserved)

## Technical Details

- **Single-file architecture** — the entire application is contained in one HTML file with embedded CSS and JavaScript; no external dependencies or build tools
- **SVG rendering** — all graph elements (vertices, edges, labels, handles) are rendered as SVG for crisp, resolution-independent display
- **Quadratic Bézier curves** — edge bending stores the on-curve midpoint and computes the off-curve control point via the identity `CP = 2M − (A + B) / 2`, ensuring the drag handle always sits exactly on the visible curve
- **Pointer events** — uses the Pointer Events API for unified mouse and touch input with pointer capture for smooth dragging
- **Hit testing** — vertex hit detection uses Euclidean distance; edge hit detection samples 50 points along the Bézier curve (or uses point-to-segment projection for straight edges)
- **Image export pipeline** — constructs a standalone SVG string with hardcoded light-theme colors, renders it to an offscreen canvas at 2x resolution via `Image` + `Blob` URL, then triggers a PNG download via `canvas.toBlob()`
- **TikZ export** — converts SVG pixel coordinates to centimeters (50px = 1cm) with y-axis flip, translates Bézier bends into TikZ `bend left/right` angles using cross-product direction detection; outputs a complete compilable LaTeX document with figure environment and degree sequence caption
- **Grid snapping** — rounds vertex coordinates to nearest grid multiple; grid is drawn as SVG lines behind all graph elements
- **Cycle edges** — sorts selected vertices by angle from centroid to determine circular order; adds straight edges between consecutive pairs, closing the cycle
- **Theme system** — CSS custom properties define all colors with light defaults and a `prefers-color-scheme: dark` media query override; a `matchMedia` listener re-renders on live theme changes

## Research Context

This tool was developed as part of PhD research on the **outerplanarity hierarchy of planar graphs and planaric degree sequences** under the supervision of Prof. Amotz Bar-Noy at the CUNY Graduate Center. It is designed for interactively constructing candidate graph realizations, verifying degree sequences, and exploring planar embeddings with curved edges to visually separate edge crossings.

## Author

Developed by Xinying Chyn, PhD student at the CUNY Graduate Center, Department of Computer Science.
