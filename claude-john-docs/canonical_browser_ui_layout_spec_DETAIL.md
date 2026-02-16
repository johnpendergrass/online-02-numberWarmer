# canonical_browser_ui_layout_spec.md

# Canonical Browser UI Layout Specification  
### Mobile · Tablet · Desktop (HTML / CSS / JS)

**Status:** Authoritative  
**Intent:** Permanent, canonical rules for browser-based UI layout across mobile phones, tablets, and desktop browsers.  
**Scope:** Layout, sizing, scaling, viewport behavior, coordinate systems.  
**Out of Scope:** Game mechanics, art direction, genre-specific UI, asset pipelines.

---

## 1. Core Principles (Non‑Negotiable)

1. **CSS pixels (logical pixels) are the only unit of measure**  
   All dimensions are expressed in CSS pixels (`px`). Device pixels are irrelevant.

2. **The browser viewport is not the device screen**  
   Browser chrome (address bar, toolbars, OS indicators) reduces usable space and can change dynamically.

3. **No document scrolling**  
   The page itself must never scroll. Any scrolling must be internal and deliberate.

4. **One canonical mobile design size**  
   All mobile-first UI is designed against a single logical stage and scaled uniformly.

5. **Larger devices add space, not rules**  
   Tablets and desktops provide additional layout space via presentation shells.

---

## 2. Terminology

**Full Screen**  
Total device screen size in CSS pixels.

**Visible Viewport**  
The area available to the web page after browser chrome, OS UI, and safe areas. This is the only space that can be relied upon.

**Logical Design Stage**  
A fixed coordinate system used for layout, interaction, and scaling.

**Presentation Shell**  
Additional layout space on larger devices that surrounds—but does not alter—the logical stage.

---

## 3. Canonical Mobile Design Target (Primary)

### Logical Design Stage

```
WIDTH:  390 CSS px
HEIGHT: 640 CSS px
ORIGIN: top-left (0,0)
```

This represents:
- A modern phone in portrait orientation
- Guaranteed visible space with browser chrome present
- A size that scales safely down to smaller phones

This stage is the foundation of all layouts.

---

## 4. Scaling Contract (Invariant)

### Uniform Scaling

```
scale = min(
  availableWidth  / designWidth,
  availableHeight / designHeight
)
```

- Aspect ratio preserved
- No non-uniform stretching
- Extra space letterboxed or pillarboxed

### Design Invariant

> All essential UI must remain usable at ~90% scale of the canonical stage.

---

## 5. Mobile Reality (Reference Ranges)

| Category | Width (CSS px) | Guaranteed Visible Height |
|--------|----------------|---------------------------|
| Canonical target | 390 | ≥ 640 |
| Smaller phones | 375 | ~550–600 |
| Larger phones | 393–440 | 700+ |

**Rule:** If it fits in **390 × 640**, it fits on all phones.

---

## 6. Tablet Targets (Secondary)

- Logical stage remains unchanged
- Extra space provided via presentation shell

### Tablet Reference Targets (Portrait)

| Target | Width | Height | Notes |
|------|------|-------|------|
| Small tablet | 744 px | ~1000 px | iPad Mini class |
| Typical tablet | 820 px | ~1100 px | iPad / Air class |
| Large tablet | 1024+ px | 1250+ px | Pro class |

### Tablet Scaling Policy

- Scale up with a recommended cap (~1.5–1.6×)
- Use excess space for panels, HUD, margins

---

## 7. Desktop Targets (Tertiary)

| Scenario | Typical Width | Typical Visible Height |
|--------|---------------|------------------------|
| Laptop browser | 1024–1280 px | ~520–600 px |
| Desktop window | 1400–1920 px | ~600–800 px |

**Insight:** Desktop is wide but often vertically constrained.

---

## 8. Viewport Height Rules (Mandatory)

- Do not rely on `100vh`
- Prefer `100dvh` or `100svh`
- Prefer `visualViewport` for runtime measurement

---

## 9. Safe Area Policy

- Default (no `viewport-fit=cover`): browser protects content
- Full-bleed: apply `env(safe-area-inset-*)` to outer container
- Never bake safe areas into logical stage

---

## 10. Input Coordinate Mapping

```
logicalX = (clientX - stageLeft) / scale
logicalY = (clientY - stageTop)  / scale
```

Clamp to logical bounds.

---

## 11. Minimum Interaction Sizes

| Element | Minimum at Design Size |
|-------|------------------------|
| Tap targets | ≥ 48 × 48 px |
| Primary text | ≥ 16 px |
| Secondary text | ≥ 14 px |

---

## 12. Canonical Target Summary

| Platform | Basis | Strategy |
|--------|------|----------|
| Mobile | 390 × 640 stage | Scale down |
| Tablet | Same stage | Scale up (capped) |
| Desktop | Same stage | Use width, conserve height |

---

## 13. Final Rule

> There is exactly one logical design stage. Devices differ only in how much space they provide around it.


---
