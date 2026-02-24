# Lathe G-code Converter

A single-file HTML tool that converts G-code by adding **D** (cumulative degrees) and **rotQty** comments to each line. Designed for embedding in Notion (or use in the browser); no server required.

---

## What it does

- **Input:** Lines with position axes — **U/W** or **V/W** (planar) or **X, Y, Z, A** (e.g. lathe).
- **Output:** The same lines with **D** and a **;rotQty=** (or **:rotQty-** ) comment appended.  
  Formula: segment distance → **rotQty** = segment ÷ passWidth → **ΔD** = rotQty × 360 (D is cumulative from your starting lathe position).
- **Toolpath:** A 2×2 view (Top, Front, Right, Isometric) of the path, updated after each Convert.

---

## How to use it

1. **Program details (optional)**  
   Fill in `programID`, `operationID`, `translation`, `lineNumberType`, `passWidth`, `programName`, `description` if you use them for reference. **passWidth** is required for the conversion math (default 0.5).

2. **Conversion parameters**  
   - **partDiameter**, **desiredSurfaceSpeed** — used to compute **travelSpeedF** (read-only).  
   - **startingLathePosition** — D values start from this and accumulate (default 0).  
   - **finalLathePosition** — filled in after Convert (read-only).

3. **Paste your G-code** into the **Input** box (e.g. `U-21.672 W26.607 F200.` or `X777.367 Z12.7`).

4. **Convert**  
   Click **Convert**. The **Output** box shows the same lines with **D** and **;rotQty=** added. The **Toolpath** panel updates to show Top, Front, Right, and Isometric views.

5. **Copy or download**  
   Use **Copy output** to paste into another app, or **Download .nc** to save a file.

---

## Input formats

| Format   | Axes        | Use case              |
|----------|-------------|------------------------|
| Planar   | **U** and **W** or **V** and **W** | 2D planar paths        |
| Lathe    | **X**, **Z** (and optionally **Y**, **A**) | Lathe / XYZ toolpaths  |

- Missing axes on a line are treated as **0** for segment distance. That can create "spikes" in D/rotQty when an axis appears or disappears between lines.
- **A** is kept on the line but not used in the distance calculation.

---

## Key parameters

- **passWidth** — Used in rotQty = segmentDistance ÷ passWidth.
- **startingLathePosition** — First D value = this + ΔD for the first segment.
- **travelSpeedF** — Computed from passWidth × desiredSurfaceSpeed ÷ (partDiameter × π). Read-only; for reference when setting feed (F).

---

## Toolpath views

After **Convert**, the Toolpath card shows four views of the path:

- **Top** — Looking down the Z axis (X vs Y).
- **Front** — X vs Z.
- **Right** — Y vs Z.
- **Iso** — Isometric (X, Y, Z).

For **U,W** or **V,W** input, the path is drawn as a 2D shape in the Top view and in isometric.

---

## Embedding in Notion

Host the HTML file (e.g. on Cloudflare Pages, Vercel, or Netlify) and use the public URL in Notion's **/embed** block. The layout is kept compact so it fits inside the embed without scrolling.

---

## File

- **lathe_gcode_converter.html** — Single file: HTML, CSS, and JavaScript. No dependencies.
