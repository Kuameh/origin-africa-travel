# Sanctuary — Design System

> A complete reference of the visual language used in the **Sanctuary | Slow Exploration** landing page. Every token, rule, and pattern below is written to be directly reusable when composing new pages, sections, or components.

---

## 1. Design Tokens (CSS Custom Properties)

All values live on `:root` and should be consumed via `var(--token-name)` everywhere.

### 1.1 Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--c-bg-base` | `#6B655B` | Page / body background — warm earthy taupe |
| `--c-bg-overlay` | `rgba(107, 101, 91, 0.85)` | Semi-transparent version of base bg; used for overlay tinting (noise layer) |
| `--c-text-main` | `#ffffff` | Primary text — pure white for maximum contrast on dark bg |
| `--c-text-muted` | `rgba(255, 255, 255, 0.7)` | Secondary / supporting text — 70 % white |

**Inline / one-off colours used (not yet tokenised — candidates for new tokens):**

| Value | Where used | Suggested token name |
|---|---|---|
| `rgba(255, 255, 255, 0.15)` | Header bottom border | `--c-border-subtle` |
| `rgba(255, 255, 255, 0.6)` | Orb button border | `--c-border-medium` |
| `rgba(255, 255, 255, 0.2)` | Orb ring pseudo-element | `--c-border-faint` |
| `rgba(255, 255, 255, 0.9)` | Orb button hover border | `--c-border-strong` |
| `rgba(255, 255, 255, 0.4)` | Coords text, orb hover ring | `--c-text-ghost` |
| `rgba(255, 255, 255, 0.1)` | Orb hover bg, cursor active bg | `--c-fill-whisper` |
| `rgba(255, 255, 255, 0.3)` | Cursor outline border | `--c-border-cursor` |
| `white` / `#ffffff` | Cursor dot bg, orb active fill | `--c-fill-solid` |

### 1.2 Typography Tokens

| Token | Value | Role |
|---|---|---|
| `--font-serif` | `'Playfair Display', serif` | Display / headline typeface — elegant, editorial |
| `--font-sans` | `'Manrope', sans-serif` | UI / body typeface — geometric, modern |

### 1.3 Motion / Easing Tokens

| Token | Value | Notes |
|---|---|---|
| `--ease-fluid` | `cubic-bezier(0.25, 1, 0.5, 1)` | Smooth, slightly decelerating ease — used on all meaningful transitions |

---

## 2. Typography Scale & Rules

### 2.1 Loaded Font Weights

| Family | Weights loaded | Notes |
|---|---|---|
| **Playfair Display** | 400, 500 (+ italic 400) | Italic 400 is specifically used for the `<i>` tag inside hero text |
| **Manrope** | 300, 400, 500 | 500 for nav labels; 300/400 available for body copy |

### 2.2 Type Styles (Component-level)

Each style below is a reusable "text block" — copy the property set to reproduce it.

#### `text-brand` — Logo / Brand Name
```css
font-family: var(--font-serif);
font-size: 1.8rem;
font-weight: 400;
font-style: italic;
letter-spacing: -0.02em;
text-transform: none;
color: var(--c-text-main);
```

#### `text-nav` — Navigation Links
```css
font-family: var(--font-sans);
font-size: 0.75rem;
font-weight: 500;
letter-spacing: 0.2em;
text-transform: uppercase;
color: var(--c-text-main);
text-decoration: none;
```

#### `text-hero-headline` — Main Hero Heading
```css
font-family: var(--font-serif);
font-size: 5rem;            /* → 3rem on mobile */
font-weight: 400;
line-height: 1.1;
letter-spacing: -0.02em;
color: var(--c-text-main);
```
> **Note:** Individual words inside the headline use `<i>` (inherited italic from Playfair Display 400 italic) for editorial emphasis.

#### `text-hero-sub` — Hero Subtitle / Prompt
```css
font-family: var(--font-sans);
font-size: 0.85rem;
font-weight: 400;            /* inherited default */
letter-spacing: 0.3em;
text-transform: uppercase;
color: var(--c-text-muted);
```

#### `text-label` — Small Action Labels
```css
font-family: var(--font-sans);
font-size: 0.7rem;
letter-spacing: 0.15em;
text-transform: uppercase;
color: var(--c-text-muted);
opacity: 0.8;
```

#### `text-coords` — Coordinate / Data Readout
```css
font-family: var(--font-sans);
font-size: 0.7rem;
font-variant-numeric: tabular-nums;
letter-spacing: 0.1em;
color: rgba(255, 255, 255, 0.4);   /* → should use --c-text-ghost */
```

### 2.3 General Typography Rules

| Rule | Value |
|---|---|
| Base font smoothing | `-webkit-font-smoothing: antialiased` |
| Default body font | `var(--font-sans)` |
| Default body color | `var(--c-text-main)` |
| Sans-serif role | UI chrome, labels, coordinates, subtitles |
| Serif role | Brand name, headlines — always display-scale |
| Letter-spacing pattern | Negative for large serif text; positive (wide) for small uppercase sans text |
| Case pattern | Uppercase for all sans labels; sentence/natural case for serif headlines |

---

## 3. Component Blocks

### 3.1 `HeroBackground` — Immersive Blur-Spotlight Backdrop

The hero background is a **four-layer** composite stack with a cursor-following spotlight reveal effect:

| Layer | Class | z-index | Purpose |
|---|---|---|---|
| Sharp image | `.hero-bg` | 1 | Full-bleed `<img>` with brightness/sepia filter; slow scale transition on hover |
| Blur spotlight | `.hero-blur` | 1 | Same image via `background-image`, heavily blurred; radial-gradient mask follows cursor to reveal the sharp image underneath |
| Noise overlay | `.hero-noise` | 2 | SVG fractal-noise texture at 6 % opacity with `overlay` blend mode |
| Gradient | `.hero-gradient` | 3 | Multi-stop vertical gradient fading into `--c-bg-base` at the bottom |

#### Sharp Layer (`.hero-bg img`)
```css
width: 100%;
height: 100%;
object-fit: cover;
filter: brightness(0.5) sepia(0.15);
transform: scale(1.05);
transition: transform 8s ease;
```
On `.hero:hover`, transform eases to `scale(1)` for subtle parallax.

#### Blur Spotlight Layer (`.hero-blur`)
```css
position: absolute;
inset: 0;
background-image: url('images/hero-bg.png');   /* same source as sharp */
background-size: cover;
background-position: center;
filter: blur(30px) brightness(0.45) sepia(0.25);
transform: scale(1.1);                         /* prevents blur edge bleed */
pointer-events: none;
mask-image: radial-gradient(
    circle 300px at var(--blur-x, 50%) var(--blur-y, 50%),
    transparent 0%, black 100%
);
-webkit-mask-image: radial-gradient(
    circle 300px at var(--blur-x, 50%) var(--blur-y, 50%),
    transparent 0%, black 100%
);
```

**JavaScript (inside `animateCursor` loop):**
```js
const heroRect = heroSection.getBoundingClientRect();
if (heroRect.bottom > 0 && heroRect.top < window.innerHeight) {
    const xPercent = (cursorX / window.innerWidth) * 100;
    const yPercent = ((cursorY - heroRect.top) / heroRect.height) * 100;
    heroBlur.style.setProperty('--blur-x', `${xPercent}%`);
    heroBlur.style.setProperty('--blur-y', `${yPercent}%`);
}
```
> `--blur-x` / `--blur-y` are updated via JS at 60fps with eased cursor position. The blur layer is only active while the hero is in the viewport.

#### Noise Overlay (`.hero-noise`)
```css
position: absolute; inset: 0;
opacity: 0.06;
pointer-events: none;
mix-blend-mode: overlay;
/* SVG inline noise via data URI */
```

#### Gradient Overlay (`.hero-gradient`)
```css
position: absolute; inset: 0;
background: linear-gradient(
    180deg,
    rgba(26, 22, 18, 0.3) 0%,
    rgba(26, 22, 18, 0.1) 40%,
    rgba(26, 22, 18, 0.5) 80%,
    rgba(26, 22, 18, 1) 100%
);
```

---

### 3.2 `NavBar` — Top Header Bar

**Structure:**
```
header.header-top
├── a.nav-text.brand          → Brand logo (text-brand style)
└── div (flex row, gap: 3rem)
    ├── a.nav-text             → "Journal"
    └── a.nav-text             → "Sound On"
```

**Container:**
```css
display: flex;
justify-content: space-between;
align-items: center;
width: 100%;
border-bottom: 1px solid rgba(255, 255, 255, 0.15);
padding-bottom: 1.5rem;
```

**Link interactivity:**
```css
pointer-events: auto;          /* re-enabled inside pointer-events: none parent */
transition: opacity 0.3s ease;
```
```css
/* :hover */
opacity: 0.6;
```

---

### 3.3 `HeroText` — Centred Hero Block

**Structure:**
```
main.hero-text
├── h1.prompt-main   → "silence is a luxury"
└── p.prompt-sub     → "Where would you like to go?"
```

**Container positioning:**
```css
position: absolute;
top: 48%;
left: 50%;
transform: translate(-50%, -50%);
text-align: center;
width: 100%;
pointer-events: none;
```

**Entry animation:** Both children use the `fadeInSlow` keyframe with staggered delays:
- `.prompt-main` → delay `0.5s`, duration `3s`
- `.prompt-sub`  → delay `1.5s`, duration `3s`

---

### 3.4 `OrbButton` — Primary CTA

A minimal, circular "hold to enter" orb with expanding ring feedback.

**Structure:**
```
div.interaction-anchor
├── button.orb-btn          → the orb itself
└── span.label-enter        → "Hold to enter" label
```

**Anchor positioning:**
```css
position: absolute;
bottom: 4rem;
left: 50%;
transform: translateX(-50%);
display: flex;
flex-direction: column;
align-items: center;
gap: 2rem;
pointer-events: auto;
```

**Button base:**
```css
width: 12px;
height: 12px;
background: transparent;
border-radius: 50%;
border: 1px solid rgba(255, 255, 255, 0.6);
transition: all 0.6s var(--ease-fluid);
```

**Pseudo-elements:**

| Pseudo | Purpose | Key styles |
|---|---|---|
| `::before` | Outer ring indicator | 40 px circle, 1 px border at 20 % white; expands to 50 px on hover |
| `::after` | Fill animation on press | Full-size white circle, `transform: scale(0)` → `scale(1)` on `:active` over 1.5 s |

**States:**

| State | Visual change |
|---|---|
| Default | Transparent 12 px dot, faint outer ring |
| `:hover` | Background → `rgba(255,255,255,0.1)`, border brightens to 90 %, ring grows to 50 px |
| `:active` (hold) | Inner white fill scales from 0 → 1 over 1.5 s |

**JavaScript interaction:**
- `mousedown` → cursor shrinks, hero text fades out, starts 1.5 s hold timer
- `mouseup` → cancels timer, restores cursor and text
- After 1.5 s hold → triggers page transition (`window.location.reload()` as placeholder)

---

### 3.5 `CustomCursor` — Dual-layer Cursor

**Structure:**
```
div.cursor-dot          → small 4 px solid white dot (follows mouse instantly)
div.cursor-outline      → 32 px ring (follows with eased lag)
```

**Cursor dot:**
```css
width: 4px;
height: 4px;
background-color: white;
border-radius: 50%;
position: fixed;
z-index: 9999;
pointer-events: none;
```

**Cursor outline:**
```css
width: 32px;
height: 32px;
border: 1px solid rgba(255, 255, 255, 0.3);
border-radius: 50%;
position: fixed;
z-index: 9999;
pointer-events: none;
transition: width 0.2s, height 0.2s, background-color 0.2s;
```

**Active state (click):**
```css
width: 20px;
height: 20px;
background-color: rgba(255, 255, 255, 0.1);
border-color: rgba(255, 255, 255, 0.6);
```

**JS animation:**
- Dot follows mouse position immediately (`mousemove`)
- Outline follows with lerp factor `0.1` via `requestAnimationFrame`

> **Global rule:** `cursor: none` set on `*` — native cursor is hidden everywhere.

---

### 3.6 `Coords` — Location Data Display

```css
position: absolute;
bottom: 4rem;
left: 5rem;          /* → 2rem on mobile */
```
Uses `text-coords` typography style (see §2.2).

---

## 4. Layout System

### 4.1 `UILayer` — Content Wrapper

All interactive / visible UI sits inside a single `.ui-layer` that floats above the background stack:

```css
position: relative;
z-index: 10;
width: 100%;
height: 100%;
display: flex;
flex-direction: column;
justify-content: space-between;
padding: 4rem 5rem;            /* → 2rem on mobile */
pointer-events: none;          /* children opt-in with pointer-events: auto */
```

### 4.2 Pointer-event Strategy

The UI layer disables pointer events globally (`pointer-events: none`) and only specific interactive children re-enable them:
- `.nav-text` (links)
- `.interaction-anchor` (orb button area)

This prevents the UI layer from blocking the background blur interaction driven by mouse movement.

---

## 5. Animations & Transitions

### 5.1 Keyframes

#### `fadeInSlow`
```css
@keyframes fadeInSlow {
    from { opacity: 0; transform: translateY(15px); }
    to   { opacity: 1; transform: translateY(0); }
}
```
**Usage:** Hero headline (delay 0.5 s) and subtitle (delay 1.5 s), both 3 s duration, eased with `var(--ease-fluid)`.

### 5.2 Transition Index

| Element | Property | Duration | Easing |
|---|---|---|---|
| `.nav-text` | `opacity` | 0.3 s | `ease` |
| `.orb-btn` | `all` | 0.6 s | `var(--ease-fluid)` |
| `.orb-btn::before` | `width`, `height`, `border-color` | 0.4 s | `var(--ease-fluid)` |
| `.orb-btn::after` | `transform` | 1.5 s | `var(--ease-fluid)` |
| `.cursor-outline` | `width`, `height`, `background-color` | 0.2 s | default |
| `.label-enter` | `opacity` | 0.3 s | default |

---

## 6. Responsive Breakpoints

| Breakpoint | Changes |
|---|---|
| `≤ 768px` | `.prompt-main` font-size → 3 rem; `.ui-layer` padding → 2 rem; `.coords` repositioned to `left: 2rem; bottom: 2rem` |

---

## 7. Global Resets & Defaults

```css
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    cursor: none;            /* custom cursor replaces native */
}

body {
    background-color: var(--c-bg-base);
    font-family: var(--font-sans);
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    color: var(--c-text-main);
    -webkit-font-smoothing: antialiased;
}
```

---

## 8. External Dependencies

| Dependency | Source | Purpose |
|---|---|---|
| Playfair Display | Google Fonts | Serif display typeface |
| Manrope | Google Fonts | Sans-serif UI typeface |
| Unsplash image | `photo-1586023492125-27b2c045efd7` | Hero background photograph |

---

## 9. Componentisation Summary

Below is a quick-reference table of every identified reusable block:

| Block Name | HTML root | Key CSS class(es) | Reusable? | Notes |
|---|---|---|---|---|
| **BackgroundLayer** | `div × 3` | `.layer-sharp`, `.layer-blur`, `.noise-overlay` | ✅ | Swap image URL; cursor-reactive |
| **NavBar** | `header` | `.header-top`, `.nav-text`, `.brand` | ✅ | Add more links as needed |
| **HeroText** | `main` | `.hero-text`, `.prompt-main`, `.prompt-sub` | ✅ | Change copy; animation auto-runs |
| **OrbButton** | `div` | `.interaction-anchor`, `.orb-btn`, `.label-enter` | ✅ | Wire up hold action via JS |
| **CustomCursor** | `div × 2` | `.cursor-dot`, `.cursor-outline` | ✅ | Requires JS; global `cursor: none` |
| **Coords** | `div` | `.coords` | ✅ | Static or dynamic data |
| **UILayer** | `div` | `.ui-layer` | ✅ | Wrapper for all visible UI |
