# Outdoor Gym 3D Model — Rebuild Prompt

Use this prompt to regenerate the outdoor gym 3D model as a standalone HTML file using Three.js r128.

---

## Prompt

Build a standalone interactive 3D model of a backyard outdoor gym as a single self-contained HTML file using Three.js r128 global scripts (not ESM). The scene should be fully orbitable using OrbitControls (drag to orbit, scroll to zoom, right-click to pan). Use ACESFilmic tone mapping and PCFSoft shadow maps. Background color `#0d1117`. Camera starts at position (7.5, 4.5, 8.5) targeting (0, 1.6, 0).

---

### Ground and Paver Pad

- Large dirt ground plane, color `#5e4d38`
- A 22×16 grid of instanced concrete pavers (each 0.195 × 0.042 × 0.095m) laid in a running bond pattern over the ground. Alternate pavers use colors `#c8aa88` and `#b89474` in a checkerboard arrangement. Total pad spans approximately 4.7m × 1.6m.

---

### Structure — 6×6 Posts and Concrete Piers

Four 6×6 posts (0.14m square × 2.85m tall, wood color `#7a5c3a`) arranged as a rectangle:
- Left front: x=−2.3, z=+0.55
- Right front: x=+2.3, z=+0.55
- Left rear: x=−2.3, z=−0.55
- Right rear: x=+2.3, z=−0.55

Each post sits on a concrete pier (cylinder, 0.155m top radius, 0.17m base radius, 0.22m tall, color `#999990`) with a Simpson-style post base bracket (flat plate + two side flanges, color `#888888`) and an anchor bolt stub.

---

### Gym Bars — Steel Pipe

All bars are 2-inch steel pipe (cylinder radius 0.038–0.035m), color `#909090` for top bars and `#606060` for mid bars, running left-to-right (x-axis), bolted through posts with carriage bolt details (small horizontal cylinders at each post intersection):

- **Top pull-up bar**: y ≈ 2.79m — one bar at z=+0.55 (front), one at z=−0.55 (rear). Length 4.7m.
- **Mid bar**: y ≈ 1.82m — same front/rear pair. Length 4.7m.
- **Side connecting beams**: wood box beams (PS−0.01 × 0.14 × 1.15m) spanning front-to-rear at x=lx and x=rx at top and mid heights.

---

### Dip Station

Two parallel steel pipe bars (radius 0.028m, length 1.05m, color `#555566`) extending front-to-back, mounted on the left post assembly at x=−2.3 ± 0.24, y ≈ 1.36m. Each bar supported by two vertical pipe stubs (radius 0.022m, height 0.42m) at z=±0.42.

---

### Gymnastics Rings

Two wooden rings (TorusGeometry radius 0.14m, tube 0.018m, color `#ddbb77`) hanging from the front top bar at x=−0.65 and x=+0.65, y ≈ 2.04m (0.75m below the top bar). Each ring has a dark nylon strap (thin cylinder, color `#222222`) connecting it to the bar above.

---

### Shed Roof — Single Slope

The roof slopes in a single direction (front lower, rear higher) for proper drainage:

- **Back post extensions**: Additional 0.42m stubs on the two rear posts to raise the rear wall plate to y ≈ 3.21m.
- **Front plate**: Wood box beam (5.0 × 0.14 × PS) at y ≈ 2.72m, z=+0.55
- **Rear plate**: Same, at y ≈ 3.14m, z=−0.55
- **Roof slope angle**: `Math.atan2(0.42, 1.1)` ≈ 21° pitched back-to-front
- **Roof panel**: BoxGeometry (5.2m wide × 0.025m thick × ~1.6m slope-length), metal material color `#8a9090`, metalness 0.45. Rotated `rotation.x = SA` so the front/+z edge is lower.
- **Rafters**: Four 2×6 wood rafters (0.055 × 0.12 × SL+0.02) at x = −1.9, −0.65, +0.65, +1.9, same rotation as roof panel, offset 0.065m below panel center.
- **Side fascia boards**: Two sloped wood boards (one on each side) connecting front-top to rear-top.

---

### Solar Panels

Two solar panels mounted flush on the roof slope surface, side by side:

- Positions: x=−0.85 and x=+0.85, same rotation as roof panel
- Panel body: BoxGeometry (1.15 × 0.04 × 0.58m), dark blue `#0d2a45`, slight emissive `#081525`
- Aluminum frame: slightly larger box, color `#777777`, metalness 0.7
- Cell grid: 4 horizontal + 7 vertical thin strips (color `#2a5080`) on panel surface
- MC4 wiring: TubeGeometry CatmullRom curve running from each panel down the rear-left post to a charge controller box

---

### Charge Controller

Small box (0.18 × 0.12 × 0.06m, color `#334433`) mounted on the rear-left post at mid height, facing rearward. Has a small green emissive screen (color `#00cc66`, emissiveIntensity 1.0) on its face.

---

### LED Lighting

Three horizontal LED strip meshes (4.5m × 0.012m × 0.025m, emissive white `#fff4cc`, emissiveIntensity 2.5) mounted under the roof at y ≈ topY+0.18, at z = +0.15, −0.05, −0.25.

Four PointLights (color `#fff4cc`, intensity 0.5, distance 4.0) placed at x = −1.8, −0.6, +0.6, +1.8 just below the LED strips to cast warm downward light.

---

### Dimension Labels

Gold (`#FFD700`) dimension annotation system with dimension lines (LineBasicMaterial, depthTest:false), tick marks, faint leader lines (opacity 0.45), and canvas-texture Sprite labels (dark background, gold border, bold text):

| Label | Value | Position |
|---|---|---|
| WIDTH | 15 ft (4.6 m) | In front of structure, near ground |
| FRONT HT | 9 ft (2.8 m) | Left side, front post |
| REAR HT | 10′-6″ (3.2 m) | Left side, rear post |
| DEPTH | 3′-6″ (1.1 m) | Right side |

---

### Lighting Setup

- AmbientLight `#fff0e8`, intensity 0.5
- DirectionalLight `#fff5d0`, intensity 1.5, position (7, 12, 8), castShadow, mapSize 2048×2048
- DirectionalLight `#b0c8ff`, intensity 0.4, position (−5, 4, −3) (fill/sky light)

---

### Page Style

```css
body { background: #0d1117; overflow: hidden; }
#title { position: fixed; top: 16px; left: 20px; color: #FFD700; font-size: 15px; }
#info  { position: fixed; bottom: 14px; left: 50%; transform: translateX(-50%); color: #555; font-size: 12px; }
```

Title text: `OUTDOOR GYM` with subtitle `Highland Reserves · Pleasant View, TN`
Info text: `Drag to orbit · Scroll to zoom · Right-click to pan`

---

### CDN Scripts (r128 — do not use r150+)

```html
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
```

Use `new THREE.OrbitControls(camera, renderer.domElement)` — OrbitControls is on the THREE global in r128.

---

### Key Measurements (1 unit = 1 meter)

| Variable | Value |
|---|---|
| `lx` / `rx` | −2.3 / +2.3 (post x positions) |
| `fz` / `bz` | +0.55 / −0.55 (front/rear z) |
| `postH` | 2.85m |
| `topY` | Y0 + postH − 0.1 ≈ 2.79m |
| `midY` | Y0 + 1.78m |
| `backExt` | 0.42m (rear post extension for roof slope) |
| `SA` | `Math.atan2(0.42, 1.1)` ≈ 21° (roof pitch) |
| `POST_S` | 0.14m (6×6 actual dimension) |
| `PH` / `Y0` | 0.042m (paver height = ground offset) |
