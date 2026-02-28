# Hero 3 Full Polish + Motion Pass (Moderate Refresh, Existing DA Schema)

## Summary
- Deliver a full visual + motion polish for Hero 3 in the selected repo: [hero-3.js](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/hero-3.js), [hero-3.css](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/hero-3.css), and [README.md](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/README.md).
- Keep authoring schema unchanged in [_hero-3.json](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/_hero-3.json).
- Port the known-good polish/motion fixes from the reference implementation in [hero-3.js](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple-main/blocks/hero-3/hero-3.js) and [hero-3.css](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple-main/blocks/hero-3/hero-3.css), then validate accessibility/performance behavior.

## Implementation Plan
1. Update motion engine in [hero-3.js](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/hero-3.js) to normalized, clamped, eased parallax.
2. Add `clamp()` and `lerp()` helpers and replace direct `translateY(progress * speed)` logic with target/current interpolation.
3. Set parallax ranges to `bg: 28`, `media: 56`, `decor: 92`, easing `0.14`, settle threshold `0.08`.
4. Compute parallax based on section center vs viewport center and clamp normalized value to `[-1, 1]`.
5. Write transforms through CSS custom property `--hero-3-parallax-y` (not direct `style.transform`) for all three layers.
6. Gate parallax off when any are true: `prefers-reduced-motion`, `(max-width: 900px)`, `(pointer: coarse)`.
7. Add `resize` handling and `IntersectionObserver` (`rootMargin: '25% 0px 25% 0px'`) so RAF work only runs while section is near/in view.
8. Refactor media DOM structure to insert `hero-3-media-parallax` wrapper and move `.hero-3-media-main` into it so entrance and parallax transforms do not conflict.
9. Keep existing content parsing and row behavior unchanged (rows 1–9 contract remains intact).

10. Apply visual polish pass in [hero-3.css](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/hero-3.css) with moderate refresh.
11. Upgrade section shell background to layered radial + linear gradients and add subtle depth (`isolation`, softened section shadow).
12. Convert `.hero-3-bg-layer`, `.hero-3-media-parallax`, and `.hero-3-decor-layer` to use `transform: translate3d(0, var(--hero-3-parallax-y), 0)`.
13. Add soft blur glow blobs on `.hero-3-bg-layer::before/::after` for ambient depth.
14. Refine typography contrast and headline/subcopy presentation (headline line-height/shadow and subcopy opacity tuning).
15. Improve accent card depth (softer shadow), slab size/blur tuning, and media image intro scale behavior.
16. Keep CTA semantics and interactions; only tune timing/easing consistency to match the new motion rhythm.

17. Fix entrance/floating motion choreography to remove transform conflicts.
18. Remove always-on floating animations from base `.hero-3-accent-card:nth-child(...)`.
19. Start float keyframes only after `hero-3-entered` state with explicit delays (`1.05s` / `1.3s`) so entrance and float never fight.
20. Adjust entrance transitions to `translate3d(...)` versions and tuned delays (`560ms` / `700ms`) for smoother staging.
21. Keep reduced-motion overrides as hard stop (`animation: none`, `transition: none`, no entrance transforms).

22. Update block documentation in [README.md](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/README.md).
23. Replace outdated parallax-speed and timing descriptions with normalized/clamped/eased model and new gating rules.
24. Keep DA table authoring instructions aligned to current 9-row schema and examples you provided.

## Public Interfaces / Types
- DA schema in [_hero-3.json](/Users/jeniferhanki/Library/Mobile%20Documents/com~apple~CloudDocs/GitHub%20Repositories%20M1/jenhankib2bapple/blocks/hero-3/_hero-3.json): **no changes**.
- Author-facing table contract: **unchanged** (rows 1–9 and field meanings preserved).
- DOM/CSS contract change: new wrapper class `.hero-3-media-parallax` becomes part of Hero 3 internal structure.
- Motion variable contract: parallax now depends on `--hero-3-parallax-y` on BG/media/decor layers.

## Test Cases and Scenarios
1. Desktop fine pointer (`>=1024px`): parallax is smooth, bounded, and no jump when entering/leaving viewport.
2. Tablet/phone (`<=900px`): parallax is disabled; layout remains stable and visually consistent.
3. Coarse pointer desktop/touch laptop: parallax disabled regardless of width.
4. `prefers-reduced-motion: reduce`: no parallax, no entrance animation, no floating keyframes.
5. Entrance sequence: staggered text, then media, then accent cards; no transform snapping or fighting.
6. One or both accent images missing: no console errors; layout still balanced.
7. Missing optional rows (urgency, eyebrow, trust, badges): component renders without broken spacing.
8. CTA links: internal links unchanged; external links still get `target="_blank"` + `rel="noopener noreferrer"`.
9. Mobile layout (`<=768px`): vertical rail hidden, second accent card hidden, media above text.
10. Visual QA in DA Live using provided row guidance: content maps to expected UI regions exactly.
11. Lint checks pass: `npm run lint:js` and `npm run lint:css`.

## Acceptance Criteria
1. Hero 3 looks visibly more polished (depth, gradients, shadows, contrast) without changing authored content format.
2. Scroll motion feels smooth and controlled with no jank from competing transforms.
3. Motion accessibility gates are fully respected for reduced-motion, coarse pointers, and compact viewports.
4. No schema changes required for existing DA-authored Hero 3 content.
5. Documentation reflects actual runtime behavior and authoring expectations.

## Assumptions and Defaults
- Target repo is `/Users/jeniferhanki/Library/Mobile Documents/com~apple~CloudDocs/GitHub Repositories M1/jenhankib2bapple`.
- Visual direction is moderate refresh, not a structural redesign.
- `_hero-3.json` remains unchanged unless a later request explicitly asks for author controls.
- No global token changes are required; Hero 3 uses existing design tokens from site styles.
- No new dependencies or build tooling changes are introduced.
