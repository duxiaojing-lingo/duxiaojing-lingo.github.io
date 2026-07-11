# Design Spec: Stardew Valley Portfolio Redesign
Date: 2026-07-11
Owner: Xiaojing Du (xed20@cam.ac.uk)

## Goal
Rebuild the existing "Farm of Phonetics" single-page portfolio to feel genuinely inside Stardew Valley — bright summer palette, animated Junimo hero scene, NPC-dialogue wording throughout, and unique SVG pixel-art characters for every collaborator/supervisor.

## Output
- File: `/Users/dududu/portfolio/index.html`
- Audio: copy `~/Downloads/ConcernedApe - Summer (Nature's Crescendo).mp3` → `/Users/dududu/portfolio/audio/summer.mp3`
- Portrait: embed as base64 from `/Users/dududu/Library/Containers/com.aptonic.Dropzone4/Data/Library/Application Support/Dropzone/Promised Files/DzFilePromise-053C2D6C-8B66-4A70-81A2-10376E018988/xiaojing_du.webp`

## Color Palette (Bright Stardew Summer)
Old palette was too dark/desaturated. New values:

| Token | Value | Use |
|-------|-------|-----|
| `--sky-top` | `#5bc8f5` | Hero sky top |
| `--sky-bottom` | `#c8eefb` | Hero sky bottom |
| `--grass-bright` | `#a8e063` | Foreground grass |
| `--grass` | `#6ab04c` | Mid grass |
| `--grass-dark` | `#388e3c` | Shadow/edge |
| `--gold` | `#f9ca24` | Panel borders, highlights |
| `--gold-dark` | `#e6990a` | Shadow gold |
| `--wood` | `#c8864a` | Buttons, nav |
| `--wood-dark` | `#7d4f27` | Button shadow |
| `--parchment` | `#fef9e7` | Panel backgrounds |
| `--parchment-dark` | `#f5e6c8` | Panel border inset |
| `--ink` | `#3d2b1f` | Body text |
| `--ink-soft` | `#6b5544` | Secondary text |
| `--heart` | `#ff6b9d` | Heart icons |
| `--accent-blue` | `#4fc3f7` | Skills/highlights |
| `--accent-green` | `#69f0ae` | Status badges |
| `--accent-purple` | `#ce93d8` | Section accents |

## Fonts
- `Press Start 2P` — headings, nav, labels
- `VT323` — body text, dialogue
- Both from Google Fonts (already in existing file)

## Sections (Nav order)

1. **The Farm** (home/hero)
2. **Farmer's Journal** (about)
3. **Equipped Tools** (skills)
4. **Quest Log** (publications)
5. **Treasure Chest** (grants/awards)
6. **Valley Folks** (collaborators)
7. **Adventure Log** (research/service)
8. **Send a Dove** (contact)

## Hero Section Detail

### Scene Layout (top to bottom, CSS-layered)
1. **Sky layer** — fixed, `--sky-top` → `--sky-bottom` gradient, floating pixel clouds (CSS divs with box-shadow blobs, drifting right via `driftCloud` keyframe)
2. **Mountains** — two distant pixel mountain silhouettes in muted teal/green, fixed behind grass
3. **Trees** — 4 tall pixel trees in CSS (`trunk` + 3 canopy `rect` layers stacked, bright `--grass-bright`) positioned left and right edges
4. **Grass strip** — repeating-linear-gradient, bright green, 80px tall at bottom
5. **Junimos** — 6 absolutely-positioned, animated creatures (details below)
6. **Portrait card** — center stage, the `xiaojing_du.webp` in a golden pixel frame, 280px wide max
7. **Dialogue box** — below portrait, Stardew-style speech bubble with typewriter text

### Junimo CSS Sprites
Each `.junimo` is a `<div>` with child elements:
- Body: `60px × 66px`, border-radius `50% / 55%`, one of 6 colors
- Eyes: two `::before`/`::after` dark dots (4px circles), slightly off-center
- Stem: a `3px × 8px` thin rectangle on top, same color as body but darker
- Legs: two tiny `8px × 6px` rectangles at bottom

Colors: `#5de65d` (green), `#f9ca24` (yellow), `#ff6b6b` (red), `#c084fc` (purple), `#60a5fa` (blue), `#fb923c` (orange)

Animations (CSS keyframes on each, staggered `animation-delay`):
- `junimoHop`: translateY 0 → -18px → 0, 0.8s ease-in-out infinite, different duration per junimo (0.7s–1.1s)
- `junimoWiggle`: slight rotate -5deg → 5deg → -5deg, 2s ease-in-out infinite

Positions: scattered across the grass strip, absolute within `.hero-scene`.

### Dialogue Typewriter
Text cycles through 3 lines (JS `setInterval`, 60ms per character):
1. "Oh! A visitor! Welcome to Phonetics Valley! ♪"
2. "I'm Xiaojing — I farm acoustic data and grow tones..."
3. "Make yourself at home. The valley has much to discover! ★"

### Hero Buttons
- `[ Enter the Valley ]` → scrolls to #about
- `[ Read the Quest Log ]` → scrolls to #quests

## Wording Changes (all sections)

### About / Farmer's Journal
- Section header: `📖 Farmer's Journal`
- Sub-label: `"a little about the farmer"`
- Bio text (NPC style):
  > "Hello there! I'm Xiaojing Du, a phonetician and PhD researcher at the Cambridge Phonetics Laboratory, University of Cambridge. I spend my days farming acoustic data — measuring how people's voices shimmer and shift, from Jin Chinese tones to the subtle breathiness of English vowels.
  >
  > Before the valley, I studied at Wake Forest University (B.S. Mathematical Statistics & Linguistics, 2024), where I first discovered that language and math could grow side by side.
  >
  > These days I'm tending two main plots: a frame-level voice quality classifier that teaches machines to hear what phoneticians hear, and the tones of Huojia Jin Chinese, a dialect with secrets buried deep in its mountains."
- Education road → styled as a "Farmer's Timeline" with milestone markers (pixel star icons)

### Skills / Equipped Tools
- Header: `🛠️ Equipped Tools`
- Sub: `"items in the farmer's kit"`
- Skill bars → "proficiency seeds" — same bar UI but relabeled

### Publications / Quest Log
- Header: `📜 Quest Log`
- Sub: `"completed and in-progress quests"`
- Each paper card header: `MAIN QUEST`, `SIDE QUEST`, `CROSSOVER QUEST` (keep existing tags)
- Intro blurb: "Every researcher's farm grows through the quests they complete. Here are mine — tap any scroll to read more."

### Grants / Treasure Chest
- Header: `💰 Treasure Chest`
- Sub: `"loot found along the journey"`

### Collaborators / Valley Folks
- Header: `🏘️ Valley Folks`
- Sub: `"the NPCs who helped this farm grow"`
- Group names: `🏡 PhD Supervisors` → `🏫 The Elders` (supervisors), `🌻 Mentors from the Old Valley` (WFU), `🤝 Fellow Farmers` (collaborators)

### Research/Service / Adventure Log
- Header: `🗺️ Adventure Log`
- Sub: `"expeditions and guild service"`
- Status badges: "In Progress" → `⚔️ Active Quest`, "Completed" → `✅ Quest Complete`, "Ongoing Service" → `🛡️ Guild Duty`

### Contact / Send a Dove
- Header: `🕊️ Send a Dove`
- Sub: `"dispatch a message to the farmer"`
- Body: "The mailbox is always open. Click the envelope to reveal the address, or find me at the Cambridge Phonetics Laboratory."

## NPC Sprites (SVG)

### Technical Approach
Each sprite is an inline `<svg viewBox="0 0 16 32" width="80" height="160">` — 16×32 "pixel" grid, scaled 5× for display. Each "pixel" is a `<rect>` at 1×1. `image-rendering: pixelated` on the SVG element.

### Character Grid (colors chosen per person)
| Person | Role | Hair | Outfit | Skin |
|--------|------|------|--------|------|
| Prof. Brechtje Post | PhD Supervisor | Blonde/light brown | Deep purple robe | #f5cba7 |
| Prof. Francis Nolan | PhD Supervisor | Grey | Dark navy coat | #f5cba7 |
| Prof. Kenneth Berenhaut | Stats mentor | Brown | Teal jacket | #f5cba7 |
| Prof. Jeff Mielke | Phonetics mentor | Dark brown | Forest green | #d4a574 |
| Dr. César Gutiérrez | Research mentor | Black | Orange vest | #d4a574 |
| Dr. Sneha Jadhav | Research mentor | Black | Pink sari accent | #d4a574 |
| Dr. Tiffany Judy | Research mentor | Auburn | Yellow blouse | #f5cba7 |
| C. McGhee | VQ collaborator | Red | Blue shirt | #f5cba7 |
| M. Qian | VQ collaborator | Black | Purple jacket | #d4a574 |
| L. Shahidi | VQ collaborator | Dark brown | Coral top | #c8a07a |
| C. Xu | VQ collaborator | Black | Green hoodie | #d4a574 |
| Q. Duan & JWST team | Galaxy collab | Black | Starfield navy | #d4a574 |

Each sprite follows the same template pixel layout:
- Rows 0-1: hair top
- Rows 2-7: head (skin tone fill, white eyes at rows 4-5 cols 5-6 and 9-10)
- Rows 7-8: neck
- Rows 8-20: body/outfit (includes arms rows 9-16, hands rows 16-17)
- Rows 20-32: legs and feet (two-toned trousers + brown shoes)

### Card layout (each NPC)
```
┌─────────────────┐
│   [SVG sprite]  │  ← 80px wide, centered
│  Name Here      │  ← Press Start 2P, 9px
│  Role text      │  ← VT323, 16px
│ ♥♥♥♥♥♥♥♥♥♥    │  ← heart bar
└─────────────────┘
```
Card has: parchment bg, gold border, 4px solid, hover lifts 4px with box-shadow glow.

## Music Player
- Fixed bottom-right, same design as existing but brighter (gold border, parchment bg)
- Wired to `audio/summer.mp3` autoplay (muted until user clicks)
- Shows "♪ Summer - ConcernedApe" as track label
- Volume slider included

## Pixel Tree Dividers
Between every section: a `<div class="tree-divider">` containing a row of 3-5 pixel trees (CSS), alternating heights, on a bright grass strip. Provides visual breathing room and reinforces the "walking through the valley" feel.

## Pixel Tree CSS Spec
```
.pixel-tree:
  position: relative
  display: inline-block
  
  trunk: 10px wide × 16px tall, background: #7d4f27
  
  canopy layers (3 stacked divs, top to bottom):
    layer-1: 20px × 10px, margin-left: -5px, bg: #6ab04c
    layer-2: 30px × 12px, margin-left: -10px, bg: #5a9e3c  
    layer-3: 40px × 14px, margin-left: -15px, bg: #4a8e2c
    
  all layers: image-rendering: pixelated
```

## Non-Goals
- No page routing / multiple HTML files — stays single file
- No external game assets (no actual Stardew sprites)
- No backend / form submission
- No GitHub/Scholar links (not provided)
- No mobile-specific breakpoints beyond `max-width` fluid layout

## Self-Review Notes
- Portrait path is absolute on Xiaojing's machine — will base64-encode into the HTML so it's self-contained
- Music path needs a copy step: `cp ~/Downloads/ConcernedApe...mp3 ~/portfolio/audio/summer.mp3`
- All SVG sprites are inline (no external files needed)
- The existing site's content (publications, grants, bio facts) is preserved verbatim; only wording/framing changes
