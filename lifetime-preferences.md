# lifetime — Repo-Local Living Preferences
> Repo-local preferences governed by the Living Documentation Law.
> Universal constitution: `gist.md` is the compressed law list; `PREFERENCES.md` is its rationale.

## 0. Constitution Link (supreme)
- [gist.md](https://gist.github.com/vex-graph/4132a6c45cb6d3797c3e8eff2e94035a#file-gist-md) — the compressed law list, mirrored in this workspace at `../../../metadata/gists/preferences/gist.md`. It is the only normative law text; `../../../metadata/gists/preferences/PREFERENCES.md` carries the rationale and is never ingested whole. Numbers are slice addresses inside one file, never law identity: cite by Title.
- All universal laws bind this repository. Read one on demand from the workspace root: `./tools/prefs law "<title>"`, `./tools/prefs section "<section>"`, `./tools/prefs repo <name>`.
- Exclusive `lifetime` (R0 Kernel Host) preferences. R0 owns lifetimes, native OS windowing, process/application lifecycle, capability gating, and memory substrate attestation. R1 comprises Relational Engine memory/storage/cache and Hotcwap dynamic module reloading. R2 comprises Vexspoke CPU computation/behavior. R0 owns residency and hardware ground truth.

## 1. Repo-Local Law Index (Binding Matrix)

Universal laws are inherited from the compressed constitution (`../../../metadata/gists/preferences/gist.md`); this table indexes the additional laws specific to this repository.

| Law Title | Scope | Enforcement |
| :--- | :--- | :--- |
| **One-Seam Canvas Window Architecture Law** | R0 Kernel Host | Mandatory for `lifetime` |
| **Continuous Real-Time Live Resize & Presentation Law (Abolishing "Freeze-Exact")** | R0 Kernel Host | Mandatory for `lifetime` |
| **Present-On-Demand Law (composite ≠ render)** | R0 Kernel Host | Mandatory for `lifetime` |

## 2. Exclusive Repo-Local Laws (FULL PROSE RESTATEMENT)

### One-Seam Canvas Window Architecture Law

#### Definition:
Exactly ONE on-screen Metal layer belongs to the host composite: seam canvas
(`CAMetalLayer`), beneath optional transparent `NSVisualEffectView` blur.
Vulkan boards (scene backdrop, content UI) are retained OFFSCREEN targets
composited into the seam image by the render repo; ordering is fixed by this
seam canvas, not by per-scene layers — no per-scene/widget `CALayer` or Vulkan
window presentation path.

#### The Why:
One on-screen Metal layer gives the window one frame cadence. Its drawable
tracks live window-content backing size, not a fixed monitor buffer; board
rectangles resolve against that size.

#### The Rule:
1. **Bottom Layer:** window blur substrate; UI widgets own no display layer.
2. **Top Layer:** sole on-screen seam `CAMetalLayer` (`geometryFlipped = YES`,
   TopLeft-pinned, native-pixel `drawableSize`); renderer composites retained
   board targets into it; lifetime owns window and event routing.

---

### Continuous Real-Time Live Resize & Presentation Law (Abolishing "Freeze-Exact")

Freezing canvas extent or early-returning from layout during mouse drags
(`Window_isLiveResizing`) is **strictly abolished**; deferring until "settle"
freezes windows/animations and tears. Render continuously during every drag:

1. **Dynamic Extent:** `CAMetalLayer.drawableSize` tracks the live
   window-content backing bounds on every resize event; resize retained GPU
   targets without creating a Vulkan presentation object.
2. **Live Layout Recalculation = Real-Time Anchor Feel:** Recalculate layout and
   anchors against live bounds each drag frame. Pinned elements (e.g.,
   right-/bottom-anchored) update offsets and re-present, staying at their edges.
   One vsync (60/120Hz) is native granularity: CoreAnimation commits each frame,
   so no content is stale beyond one frame. The sub-frame gap is covered by the
   instantly-moved layer frame (Single-Transaction Live Coordination Law) and
   `gravity = kCAGravityTopLeft` anchor pin.
3. **No Settle Deferral:** Live resize must not wait for mouse-up to resize the
   canvas, clear backbuffers, or reposition anchors. Doing so makes the window
   feel frozen and unresponsive.

---

### Present-On-Demand Law (composite ≠ render)

Windows present when the scene is dirty, not on a timer or fixed poll:

1. **Dirty Bit Gating:** `Window_present` is a no-op unless marked dirty.
2. **Event Wake:** Window and input events wake the presentation seam.
3. **Compositor Decoupling:** Rendering a surface is decoupled from blitting
   to the window surface; the host orchestrates presentation only when
   new frames are ready.
