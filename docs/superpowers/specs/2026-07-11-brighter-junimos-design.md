# Design Spec: Brighter Palette + Pixel-Art Hero Junimos

Date: 2026-07-11
Owner: Xiaojing Du

## Goal
Continue iterating on the existing Stardew Valley portfolio (`/Users/dududu/portfolio/index.html`). Two upgrades, scoped to a visual-only pass:

1. Replace the 6 hero-scene Junimo SVGs (currently smooth ellipse blobs) with the blocky pixel-art body template that's already used for the Valley Folks (NPC) cards — matching the crisper, cuter look of the user's reference photo of pixel Junimo keychains.
2. Boost the site's color saturation/contrast and switch to a bolder "flat poster" outline/shadow treatment site-wide, inspired by the user's second reference (Yuan Lum portfolio: bright pastel bg, thick outlines, bold accent star bursts).

## Non-goals (this pass)
- No changes to Valley Folks card backgrounds/layout
- No changes to pixel-tree divider shapes
- No typography/font changes
- No content, section structure, or JS logic changes
- No changes to music player, dialogue typewriter, or other existing JS behavior

## 1. Palette changes

Update `:root` custom properties in `index.html` (currently ~line 17):

| Token | Old | New |
|---|---|---|
| `--sky-top` | `#5bc8f5` | `#3ab6ff` |
| `--sky-bottom` | `#c8eefb` | `#aee4ff` |
| `--grass-bright` | `#a8e063` | `#9bef4f` |
| `--grass` | `#6ab04c` | `#58c93f` |
| `--grass-dark` | `#388e3c` | `#2e9e2e` |
| `--gold` | `#f9ca24` | `#ffd23f` |
| `--gold-dark` | `#e6990a` | `#e8a400` |
| `--wood` | `#c8864a` | `#d99456` |
| `--ink` | `#3d2b1f` | `#2b1c14` |
| `--heart` | `#ff6b9d` | `#ff5c8d` |
| `--accent-blue` | `#4fc3f7` | `#2fd1ff` |
| `--accent-purple` | `#ce93d8` | `#d68bff` |

Unchanged: `--parchment`, `--parchment-dark`, `--wood-dark`, `--ink-soft`, `--accent-green`.

New token: `--outline: #241208` — near-black warm ink, used for bold borders/shadows in section 2.

New accent tokens for burst shapes (section 4): `--burst-blue: #3f6fe0`, `--burst-pink: #ff4d8d`, `--burst-gold: #ffd23f` (reuse `--gold`).

## 2. Bold outline/shadow treatment (site-wide)

Applies to: `.pixel-btn`, `.pixel-panel`/card classes, nav bar, music player — anywhere currently using `border: 3px|4px solid var(--gold-dark)` paired with a soft `box-shadow ... rgba(0,0,0,0.2)` offset.

- Border width: 3–4px → 4–5px, color `var(--gold-dark)` → `var(--outline)`.
- Keep a thin inset gold ring as accent highlight: `box-shadow: 0 0 0 3px var(--gold) inset, ...`
- Replace the soft rgba offset shadow with a solid `var(--outline)` offset shadow, distance increased from 6–8px to 8–10px (e.g. `8px 8px 0 var(--outline)` → `10px 10px 0 var(--outline)`).
- Hover states keep existing lift/translate behavior; only color/width of border+shadow changes.

Do not touch structural properties (padding, border-radius, layout) — only border color/width and box-shadow color/offset.

## 3. Hero Junimo sprite swap

Location: hero section markup, the 6 `.junimo-wrap` divs (currently ~line 756–835), each containing an inline `<svg viewBox="0 0 24 30">` with ellipse/rect shapes.

Replace each inline SVG with a new `viewBox="0 0 16 22"` pixel-rect body, reusing the same layout already defined in the `junimoSVG()` JS template (ears at x=0/13 y=9-12, head block, cheek blush, eyes with white highlight, arms, shaded legs) — but as static inline markup (not JS-generated), since the hero markup is static HTML, and add a topper accessory in the reserved top rows (y=0–5).

Keep each Junimo's existing body/dark color pair:
- Junimo 1: body `#5de65d`, dark `#3db33d` → topper: gold pixel star
- Junimo 2: body `#f9ca24`(new gold), dark `#c8920a` → topper: red/orange tulip
- Junimo 3: body `#ff6b6b`, dark `#aa2020` → topper: teal leaf sprout
- Junimo 4: body `#c084fc`, dark `#8040c0` → topper: purple star-flower
- Junimo 5: body `#60a5fa`, dark `#1a60d0` → topper: rainbow gem (striped)
- Junimo 6: body `#fb923c`, dark `#c05a0a` → topper: golden apple

Each topper: 4–6 small `<rect>` pixels in 2–3 colors, centered around x=5–11, y=0–5, sized/styled consistent with the accessory examples already in the `NPCS` array (e.g. Prof. Post's flower crown) for visual consistency between hero and NPC sprites.

Preserve: existing `.junimo-wrap` positioning (`left`/`right` %), `--hop-dur`/`--hop-delay` custom props, `junimoHop`/`junimoWobble` animations, and `image-rendering:pixelated`. Only the inner SVG markup and its viewBox/width/height change (scale proportionally so displayed size stays close to current, e.g. width ~70–86px depending on current stagger).

## 4. Accent star/burst shapes

New reusable inline SVG: an 8-point pixel-art burst/star, two-tone fill (outer ring color + inner color), roughly 24×24 viewBox, `image-rendering:pixelated`. Three color variants using `--burst-blue`, `--burst-pink`, `--burst-gold`.

Placement (sparse, decorative, absolutely positioned, non-interactive):
- 2–3 in the hero scene, tucked near corners/around the dialogue box (not overlapping Junimos or portrait)
- 1 near a couple of section headers or tree-dividers (not all of them) — enough to echo the reference image's playful scattering without cluttering every section

No animation required beyond what's already present (static or reuse existing subtle float/wobble keyframes if convenient); this is a visual accent, not a new interactive element.

## Self-review notes
- All changes are additive/replacement within existing CSS custom properties and existing markup blocks — no new build steps, no new files, no changes to the 2.7MB portrait base64 blob or audio setup.
- Junimo topper designs are original pixel patterns inspired by (not copied from) the user's reference photo of pixel-art Junimo keychains — no game assets or external images used, consistent with the original project's non-goal of "no external game assets."
- Risk is low: this is a styling + one-markup-block swap, verifiable by opening `index.html` in a browser and visually diffing before/after.
