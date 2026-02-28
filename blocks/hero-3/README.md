# Hero 3 Block

## Overview

Hero 3 is a split-layout parallax hero with a glassmorphic design system. It features a text column with staggered entrance animations and a media column with a main image, floating accent cards, and scroll-driven parallax layers.

## Integration

### Block Configuration

Content is authored as up to 9 rows (all optional except row 1):

| Row | Column 1 | Column 2 |
|-----|----------|----------|
| 1 | Urgency chip text | Eyebrow text |
| 2 | Headline (paragraphs; `<em>` = italic, `<strong>` = indent) | — |
| 3 | Subcopy text | — |
| 4 | CTA buttons (links) | — |
| 5 | Trust items (list with leading icon character) | — |
| 6 | Badge items (list with leading icon character) | — |
| 7 | Main image | Vertical rail text |
| 8 | Accent card 1 image | Accent card 1 label |
| 9 | Accent card 2 image | Accent card 2 label |

### Dependencies

- `../../scripts/aem.js` — `createOptimizedPicture`

## Behavior Patterns

### Layout Behavior

- **Desktop**: 58/42 grid split between text and media columns, max-width 1280px
- **Mobile (768px-)**: Single column; media stacks above text, second accent card hidden, vertical rail hidden
- Full-bleed section: Container and wrapper max-width constraints are removed

### Parallax System

Parallax is driven by a normalized section-to-viewport center distance, then clamped and eased in `requestAnimationFrame`.

- **Background layer** (`.hero-3-bg-layer`) uses `--hero-3-parallax-y` with max range ~28px
- **Media layer** (`.hero-3-media-parallax`) uses `--hero-3-parallax-y` with max range ~56px
- **Decor layer** (`.hero-3-decor-layer`) uses `--hero-3-parallax-y` with max range ~92px
- Rendering uses lerp easing (`0.14`) with settle threshold (`0.08`) to avoid jumpy movement
- `IntersectionObserver` limits animation work when the block is out of view

Parallax is disabled when any of the following are true:
- `prefers-reduced-motion: reduce`
- coarse pointer input (`(pointer: coarse)`)
- compact viewport (`(max-width: 900px)`)

### Entrance Animations

- `.hero-3-stagger` elements fade in and slide up using `translate3d(...)` for smoother composition
- Main image enters with translate + scale, then image scale settles from `1.035` to `1`
- Accent cards enter first, then floating keyframes begin after entrance delays
- `hero-3-entered` is applied after a short `requestAnimationFrame` + timeout trigger
- Card float starts after entrance (`1.05s` and `1.3s`) so transforms do not conflict

### Visual Details

- **Urgency chip**: Pill-shaped with pulsing dot animation and accent border
- **Headline**: Gradient text fill (accent-1 to accent-2 to neutral-900)
- **CTAs**: Primary uses gradient background with arrow animation; secondary uses glassmorphic border style
- **Accent cards**: Floating glassmorphic cards with delayed bobbing animations (8s/9s cycles)
- **Background depth**: Additional soft glow blobs and refined section shadows
- **Color slab**: Larger blurred gradient slab for ambient media-side depth

### Error Handling

- Missing rows: Each element is conditionally rendered only if its data exists
- No content rows: Logs a warning and returns early
- `prefers-reduced-motion`: Entrance transitions and floating animations are disabled and content is shown statically
