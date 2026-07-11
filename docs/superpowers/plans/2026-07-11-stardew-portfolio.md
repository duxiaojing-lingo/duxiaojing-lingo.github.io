# Stardew Valley Portfolio — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single self-contained `index.html` Stardew Valley–styled academic portfolio for Xiaojing Du with animated Junimo hero, SVG NPC sprites, bright summer palette, and full game-speak wording.

**Architecture:** Single `index.html` with all CSS/JS/SVG inline, portrait embedded as base64 WebP `data:` URI, 12 NPC sprites rendered via a JS template function, background music via HTML5 `<audio>` element pointing to `audio/summer.mp3`.

**Tech Stack:** Vanilla HTML5 / CSS3 / JS (ES6), Google Fonts (Press Start 2P + VT323), inline SVG, base64 image, HTML5 Audio API.

## Global Constraints
- Output file: `/Users/dududu/portfolio/index.html`
- Audio file: `/Users/dududu/portfolio/audio/summer.mp3`
- Portrait source: `/Users/dududu/Library/Containers/com.aptonic.Dropzone4/Data/Library/Application Support/Dropzone/Promised Files/DzFilePromise-053C2D6C-8B66-4A70-81A2-10376E018988/xiaojing_du.webp`
- No external assets except Google Fonts CDN
- `image-rendering: pixelated` on all SVG/pixel elements
- Bright Stardew palette — NO dark desaturated colors
- Section IDs (for nav anchors): `#home`, `#about`, `#skills`, `#quests`, `#treasure`, `#folks`, `#adventure`, `#contact`

---

### Task 1: Setup

**Files:**
- Create: `/Users/dududu/portfolio/audio/` (directory)
- Create: `/private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64`

- [ ] **Step 1: Create directories**
```bash
mkdir -p /Users/dududu/portfolio/audio
```
Expected: exits 0, no output.

- [ ] **Step 2: Copy audio**
```bash
cp "/Users/dududu/Downloads/ConcernedApe - Summer (Nature's Crescendo).mp3" "/Users/dududu/portfolio/audio/summer.mp3"
ls -lh /Users/dududu/portfolio/audio/
```
Expected: `summer.mp3` listed, size ~4–8 MB.

- [ ] **Step 3: Base64-encode portrait**
```bash
base64 -i "/Users/dududu/Library/Containers/com.aptonic.Dropzone4/Data/Library/Application Support/Dropzone/Promised Files/DzFilePromise-053C2D6C-8B66-4A70-81A2-10376E018988/xiaojing_du.webp" > /private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64
wc -c /private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64
```
Expected: a large byte count (500 KB–2 MB range). Keep this file — Task 3 embeds it.

- [ ] **Step 4: Verify portrait decodes**
```bash
base64 -d /private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64 | file -
```
Expected: `standard input: RIFF ... WebP image data` or similar.

---

### Task 2: HTML Skeleton + Complete CSS

**Files:**
- Create: `/Users/dududu/portfolio/index.html`

Write the full file skeleton. All section content is placeholder comments; later tasks fill them in via Edit.

- [ ] **Step 1: Write the complete skeleton**

Write `/Users/dududu/portfolio/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Xiaojing Du ✦ Farm of Phonetics</title>
<meta name="description" content="Xiaojing Du — PhD researcher in voice quality and tone, Cambridge Phonetics Laboratory">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323:wght@400&display=swap" rel="stylesheet">
<style>
/* =========================================================
   XIAOJING DU — FARM OF PHONETICS
   Stardew Valley–inspired academic portfolio
   ========================================================= */

:root {
  --sky-top: #5bc8f5;
  --sky-bottom: #c8eefb;
  --grass-bright: #a8e063;
  --grass: #6ab04c;
  --grass-dark: #388e3c;
  --gold: #f9ca24;
  --gold-dark: #e6990a;
  --wood: #c8864a;
  --wood-dark: #7d4f27;
  --parchment: #fef9e7;
  --parchment-dark: #f5e6c8;
  --ink: #3d2b1f;
  --ink-soft: #6b5544;
  --heart: #ff6b9d;
  --heart-empty: #f0cece;
  --accent-blue: #4fc3f7;
  --accent-green: #69f0ae;
  --accent-purple: #ce93d8;
  --red: #e74c3c;
}

*, *::before, *::after { box-sizing: border-box; }
html { scroll-behavior: smooth; }

body {
  margin: 0;
  font-family: 'VT323', monospace;
  font-size: 22px;
  color: var(--ink);
  background: #6ab04c;
  overflow-x: hidden;
  image-rendering: pixelated;
}

h1, h2, h3, .pixel-font {
  font-family: 'Press Start 2P', monospace;
  line-height: 1.6;
}

/* ── Fixed sky background ── */
.sky-layer {
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100vh;
  background: linear-gradient(180deg, var(--sky-top) 0%, var(--sky-bottom) 60%, var(--grass) 60%, var(--grass-dark) 100%);
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

/* ── Pixel clouds ── */
.cloud {
  position: absolute;
  background: #fff;
  opacity: 0.92;
  border-radius: 2px;
  box-shadow:
    16px 6px 0 #fff,
    32px 0  0 #fff,
    -16px 6px 0 #fff,
    8px  -6px 0 #fff,
    24px -6px 0 #fff;
}
.cloud1 { width:40px;height:14px;top:7%;left:-10%;animation:driftCloud 55s linear infinite; }
.cloud2 { width:28px;height:10px;top:15%;left:-25%;animation:driftCloud 70s linear infinite;animation-delay:-22s; }
.cloud3 { width:50px;height:16px;top:4%;left:-40%;animation:driftCloud 90s linear infinite;animation-delay:-45s; }
.cloud4 { width:34px;height:12px;top:20%;left:-15%;animation:driftCloud 80s linear infinite;animation-delay:-10s; }
@keyframes driftCloud {
  from { transform: translateX(0); }
  to   { transform: translateX(150vw); }
}

/* ── Nav ── */
nav.signpost {
  position: sticky;
  top: 0;
  z-index: 100;
  background: var(--wood-dark);
  border-bottom: 4px solid #4a2e0f;
  padding: 8px 12px;
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  justify-content: center;
  align-items: center;
}
nav.signpost a {
  font-family: 'Press Start 2P', monospace;
  font-size: 9px;
  color: var(--parchment);
  text-decoration: none;
  background: var(--wood);
  padding: 8px 10px;
  border-bottom: 3px solid var(--wood-dark);
  transition: transform 0.1s, background 0.1s;
  white-space: nowrap;
}
nav.signpost a:hover {
  background: var(--gold-dark);
  transform: translateY(-2px);
}
nav.signpost .brand {
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  color: var(--gold);
  text-shadow: 2px 2px 0 #000;
  margin-right: 12px;
}

/* ── Section wrapper ── */
section {
  position: relative;
  z-index: 1;
  max-width: 1000px;
  margin: 0 auto;
  padding: 70px 20px;
}

/* ── Section headers ── */
.section-title {
  text-align: center;
  font-size: 20px;
  color: var(--parchment);
  text-shadow: 3px 3px 0 var(--wood-dark);
  margin-bottom: 8px;
}
.section-sub {
  display: block;
  width: fit-content;
  margin: 0 auto 40px;
  background: var(--parchment);
  color: var(--ink-soft);
  border: 3px solid var(--gold-dark);
  padding: 4px 16px;
  font-size: 20px;
  text-align: center;
}

/* ── Pixel panel ── */
.pixel-panel {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 0 0 0 3px var(--gold) inset, 6px 6px 0 rgba(0,0,0,0.18);
  padding: 28px;
}

/* ── Wood button ── */
.pixel-btn {
  display: inline-block;
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  color: var(--parchment);
  background: var(--wood);
  border: none;
  padding: 12px 18px;
  text-decoration: none;
  cursor: pointer;
  border-bottom: 4px solid var(--wood-dark);
  transition: transform 0.08s;
  margin: 6px;
}
.pixel-btn:hover { background: var(--gold-dark); transform: translateY(-2px); }
.pixel-btn:active { transform: translateY(2px); border-bottom-width: 2px; }
.pixel-btn.gold { background: var(--gold-dark); border-bottom-color: #8a6000; }
.pixel-btn.gold:hover { background: var(--gold); }

/* ── Scroll reveal ── */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* ======================== HERO ======================== */
#home {
  min-height: 92vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding-top: 20px;
  position: relative;
  z-index: 1;
}

.hero-scene {
  position: relative;
  width: 100%;
  max-width: 700px;
  min-height: 300px;
}

/* ── Pixel trees ── */
.pixel-tree {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  position: absolute;
  bottom: 0;
  image-rendering: pixelated;
}
.pixel-tree .trunk {
  width: 14px; height: 22px;
  background: #7d4f27;
  border-top: 2px solid #5c3620;
}
.pixel-tree .canopy { display: flex; flex-direction: column; align-items: center; }
.pixel-tree .c3 { width: 48px; height: 16px; background: #4a8e2c; }
.pixel-tree .c2 { width: 36px; height: 14px; background: #5aaa38; margin-top: -4px; }
.pixel-tree .c1 { width: 22px; height: 12px; background: #6ab04c; margin-top: -4px; }

/* ── Junimo animations ── */
@keyframes junimoHop {
  0%,100% { transform: translateY(0) rotate(0deg); }
  25%      { transform: translateY(-22px) rotate(-6deg); }
  75%      { transform: translateY(-18px) rotate(5deg); }
}
@keyframes junimoWobble {
  0%,100% { transform: scaleX(1); }
  50%      { transform: scaleX(0.92); }
}

.junimo-wrap {
  position: absolute;
  bottom: 60px;
  animation: junimoHop var(--hop-dur, 0.9s) ease-in-out infinite;
  animation-delay: var(--hop-delay, 0s);
}
.junimo-wrap svg {
  display: block;
  image-rendering: pixelated;
  filter: drop-shadow(0 4px 0 rgba(0,0,0,0.18));
}

/* ── Portrait card ── */
.portrait-card {
  width: 240px;
  margin: 0 auto 24px;
  border: 5px solid var(--gold-dark);
  box-shadow: 0 0 0 3px var(--gold) inset, 8px 8px 0 rgba(0,0,0,0.2);
  background: var(--parchment);
  padding: 6px;
  image-rendering: auto;
}
.portrait-card img {
  width: 100%;
  display: block;
}

/* ── Dialogue box ── */
.dialogue-box {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 6px 6px 0 rgba(0,0,0,0.18);
  padding: 18px 22px;
  text-align: left;
  font-size: 22px;
  min-height: 80px;
  position: relative;
  max-width: 600px;
  margin: 0 auto 10px;
}
.dialogue-box::after {
  content: "";
  position: absolute;
  top: -16px; left: 50%; transform: translateX(-50%);
  width: 0; height: 0;
  border-left: 12px solid transparent;
  border-right: 12px solid transparent;
  border-bottom: 16px solid var(--wood-dark);
}
.cursor-blink {
  display: inline-block;
  width: 10px; height: 18px;
  background: var(--ink);
  animation: blink 1s steps(1) infinite;
  vertical-align: middle;
  margin-left: 2px;
}
@keyframes blink { 50% { opacity: 0; } }

/* ── Hero grass strip ── */
.hero-grass {
  position: relative;
  height: 80px;
  width: 100%;
  background: repeating-linear-gradient(90deg,
    var(--grass-bright) 0 8px,
    var(--grass) 8px 16px);
  border-top: 4px solid var(--grass-dark);
  overflow: hidden;
}

/* ── Hero flowers ── */
.flower {
  position: absolute;
  bottom: 10px;
  width: 12px; height: 12px;
  border-radius: 50%;
}

/* ======================== ABOUT ======================== */
.about-grid {
  display: grid;
  grid-template-columns: 220px 1fr;
  gap: 28px;
  align-items: start;
}
@media (max-width: 640px) { .about-grid { grid-template-columns: 1fr; } }

.char-card {
  background: var(--parchment);
  border: 4px solid var(--gold-dark);
  box-shadow: 6px 6px 0 rgba(0,0,0,0.15);
  padding: 18px;
  text-align: center;
}
.char-card .char-name {
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  color: var(--ink);
  margin: 10px 0 4px;
}
.char-card .char-title {
  font-size: 18px;
  color: var(--ink-soft);
}
.char-stats { margin-top: 12px; }
.char-stats .stat-row {
  display: flex;
  justify-content: space-between;
  border-bottom: 2px solid var(--parchment-dark);
  padding: 3px 0;
  font-size: 18px;
}

.journal-panel {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 6px 6px 0 rgba(0,0,0,0.15);
  padding: 24px;
}
.journal-panel h3 {
  font-family: 'Press Start 2P', monospace;
  font-size: 11px;
  color: var(--gold-dark);
  margin: 0 0 14px;
}
.journal-panel p {
  line-height: 1.5;
  margin: 0 0 12px;
  font-size: 22px;
}

/* ── Education timeline ── */
.edu-timeline { margin-top: 28px; }
.edu-entry {
  display: flex;
  gap: 16px;
  padding: 14px 0;
  border-bottom: 2px dashed var(--parchment-dark);
}
.edu-star {
  font-size: 24px;
  flex-shrink: 0;
  line-height: 1;
  margin-top: 2px;
}
.edu-year {
  font-family: 'Press Start 2P', monospace;
  font-size: 9px;
  color: var(--gold-dark);
}
.edu-place { font-size: 20px; color: var(--ink-soft); }

/* ======================== SKILLS ======================== */
.skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}
.skill-card {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.15);
  padding: 16px;
}
.skill-name {
  font-family: 'Press Start 2P', monospace;
  font-size: 9px;
  color: var(--ink);
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.skill-bar-bg {
  height: 14px;
  background: var(--parchment-dark);
  border: 2px solid var(--wood-dark);
  position: relative;
  overflow: hidden;
}
.skill-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--gold-dark), var(--gold));
  position: absolute;
  left: 0; top: 0;
  transition: width 1s ease;
}
.skill-bar-fill::after {
  content: '';
  position: absolute;
  right: 0; top: 0; bottom: 0;
  width: 4px;
  background: rgba(255,255,255,0.5);
}
.skill-level {
  font-size: 16px;
  color: var(--ink-soft);
  margin-top: 4px;
  text-align: right;
}

/* ======================== QUESTS ======================== */
.quest-card {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.15);
  margin-bottom: 18px;
  cursor: pointer;
  transition: box-shadow 0.15s, transform 0.15s;
}
.quest-card:hover { transform: translateY(-2px); box-shadow: 6px 6px 0 rgba(0,0,0,0.2); }
.quest-top {
  display: flex;
  gap: 16px;
  align-items: flex-start;
  padding: 18px;
}
.quest-icon { font-size: 32px; flex-shrink: 0; }
.quest-tag {
  font-family: 'Press Start 2P', monospace;
  font-size: 8px;
  color: var(--gold-dark);
  display: block;
  margin-bottom: 6px;
}
.quest-card h4 {
  margin: 0 0 6px;
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  color: var(--ink);
  line-height: 1.7;
}
.quest-card .authors { font-size: 18px; color: var(--ink-soft); margin: 0 0 4px; }
.quest-card .venue   { font-size: 18px; color: var(--accent-blue); margin: 0; }
.quest-detail {
  display: none;
  padding: 0 18px 18px;
  font-size: 20px;
  color: var(--ink-soft);
  border-top: 2px dashed var(--parchment-dark);
}
.quest-card.open .quest-detail { display: block; }
.toggle-hint {
  text-align: center;
  font-size: 16px;
  color: var(--ink-soft);
  padding: 4px;
  background: var(--parchment-dark);
}

/* ======================== TREASURE ======================== */
.treasure-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 18px;
}
.treasure-tile {
  background: var(--parchment);
  border: 4px solid var(--gold-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.15);
  padding: 18px;
  text-align: center;
  position: relative;
}
.treasure-tile .year-badge {
  position: absolute;
  top: -2px; right: 10px;
  font-family: 'Press Start 2P', monospace;
  font-size: 8px;
  background: var(--gold-dark);
  color: var(--parchment);
  padding: 3px 7px;
}
.treasure-tile .trophy { font-size: 36px; display: block; margin: 8px 0; }
.treasure-tile .amount {
  font-family: 'Press Start 2P', monospace;
  font-size: 12px;
  color: var(--gold-dark);
  margin-bottom: 8px;
}
.treasure-tile h4 {
  font-family: 'Press Start 2P', monospace;
  font-size: 8px;
  color: var(--ink);
  margin: 0 0 6px;
  line-height: 1.8;
}
.treasure-tile .org  { font-size: 18px; color: var(--ink-soft); }
.treasure-tile .desc { font-size: 18px; color: var(--ink-soft); margin-top: 6px; }

/* ======================== VALLEY FOLKS ======================== */
.folks-group { margin-bottom: 50px; }
.folks-group-title {
  font-family: 'Press Start 2P', monospace;
  font-size: 12px;
  color: var(--parchment);
  text-shadow: 2px 2px 0 var(--wood-dark);
  margin-bottom: 20px;
}
.npc-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
.npc-card {
  background: var(--parchment);
  border: 4px solid var(--gold-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.15);
  padding: 16px 12px;
  text-align: center;
  width: 148px;
  transition: transform 0.15s, box-shadow 0.15s;
  cursor: default;
}
.npc-card:hover {
  transform: translateY(-4px);
  box-shadow: 4px 8px 0 rgba(0,0,0,0.22);
}
.npc-name {
  font-family: 'Press Start 2P', monospace;
  font-size: 7px;
  color: var(--ink);
  margin: 10px 0 4px;
  line-height: 1.7;
}
.npc-role { font-size: 16px; color: var(--ink-soft); line-height: 1.3; }
.npc-hearts { margin-top: 8px; font-size: 18px; letter-spacing: 1px; }

/* ======================== ADVENTURE LOG ======================== */
.adv-list { display: flex; flex-direction: column; gap: 20px; }
.adv-card {
  background: var(--parchment);
  border: 4px solid var(--wood-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.15);
  padding: 20px;
  display: flex;
  gap: 18px;
  align-items: flex-start;
}
.adv-icon { font-size: 36px; flex-shrink: 0; }
.adv-card h4 {
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  color: var(--ink);
  margin: 0 0 6px;
  line-height: 1.7;
}
.adv-card .meta { font-size: 18px; color: var(--ink-soft); margin-bottom: 8px; }
.adv-card ul { margin: 0 0 10px; padding-left: 20px; font-size: 20px; }
.adv-card li { margin-bottom: 4px; }
.status-badge {
  display: inline-block;
  font-family: 'Press Start 2P', monospace;
  font-size: 8px;
  padding: 4px 10px;
  background: var(--accent-blue);
  color: var(--ink);
  border: 2px solid var(--ink);
}
.status-badge.done { background: var(--accent-green); }
.status-badge.guild { background: var(--accent-purple); }

/* ======================== CONTACT ======================== */
.mailbox-wrap {
  text-align: center;
  max-width: 500px;
  margin: 0 auto;
}
.mailbox-icon {
  font-size: 72px;
  cursor: pointer;
  display: block;
  transition: transform 0.2s;
  animation: floatMail 2.5s ease-in-out infinite;
}
.mailbox-icon:hover { transform: scale(1.15); }
@keyframes floatMail {
  0%,100% { transform: translateY(0); }
  50%      { transform: translateY(-8px); }
}
.email-reveal {
  display: none;
  font-family: 'Press Start 2P', monospace;
  font-size: 12px;
  background: var(--gold);
  color: var(--ink);
  padding: 12px 18px;
  border: 4px solid var(--gold-dark);
  margin-top: 14px;
}

/* ======================== TREE DIVIDER ======================== */
.tree-divider {
  position: relative;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  gap: 20px;
  height: 90px;
  overflow: hidden;
  background: repeating-linear-gradient(90deg, var(--grass-bright) 0 8px, var(--grass) 8px 16px);
  border-top: 4px solid var(--grass-dark);
  border-bottom: 4px solid var(--grass-dark);
  padding-bottom: 4px;
}
.tree-divider .pixel-tree { position: static; }

/* ── Small divider tree (shorter) ── */
.pixel-tree.sm .trunk  { width:10px; height:14px; }
.pixel-tree.sm .c3     { width:34px; height:12px; }
.pixel-tree.sm .c2     { width:24px; height:10px; }
.pixel-tree.sm .c1     { width:14px; height:8px; }

/* ======================== MUSIC PLAYER ======================== */
.music-player {
  position: fixed;
  bottom: 18px; right: 18px;
  z-index: 200;
  background: var(--parchment);
  border: 4px solid var(--gold-dark);
  box-shadow: 4px 4px 0 rgba(0,0,0,0.2);
  padding: 10px 14px;
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: 'Press Start 2P', monospace;
  font-size: 9px;
  color: var(--ink);
}
.music-player button {
  background: var(--gold-dark);
  border: none;
  color: var(--parchment);
  font-family: 'Press Start 2P', monospace;
  font-size: 10px;
  padding: 6px 10px;
  cursor: pointer;
  border-bottom: 3px solid #8a6000;
}
.music-player button:hover { background: var(--gold); color: var(--ink); }
.music-player input[type=range] { width: 60px; accent-color: var(--gold-dark); }
.note-float {
  font-size: 18px;
  animation: noteFloat 1.8s ease-in-out infinite;
}
@keyframes noteFloat {
  0%,100% { transform: translateY(0) rotate(-5deg); opacity:1; }
  50%      { transform: translateY(-8px) rotate(5deg); opacity:0.6; }
}

/* ======================== FOOTER ======================== */
footer {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 24px;
  background: var(--wood-dark);
  color: var(--parchment);
  font-size: 18px;
  border-top: 4px solid #4a2e0f;
}
</style>
</head>
<body>

<!-- Fixed sky + cloud layer -->
<div class="sky-layer" aria-hidden="true">
  <div class="cloud cloud1"></div>
  <div class="cloud cloud2"></div>
  <div class="cloud cloud3"></div>
  <div class="cloud cloud4"></div>
</div>

<!-- ── Navigation ── -->
<nav class="signpost" aria-label="Main navigation">
  <span class="brand">✦ PHONETICS VALLEY</span>
  <a href="#home">The Farm</a>
  <a href="#about">Journal</a>
  <a href="#skills">Tools</a>
  <a href="#quests">Quest Log</a>
  <a href="#treasure">Treasure</a>
  <a href="#folks">Valley Folks</a>
  <a href="#adventure">Adventure</a>
  <a href="#contact">Send a Dove</a>
</nav>

<!-- ── HERO ── -->
<!-- PLACEHOLDER_HERO -->

<!-- TREE DIVIDER 1 -->
<!-- PLACEHOLDER_DIVIDER_1 -->

<!-- ── ABOUT ── -->
<!-- PLACEHOLDER_ABOUT -->

<!-- TREE DIVIDER 2 -->
<!-- PLACEHOLDER_DIVIDER_2 -->

<!-- ── SKILLS ── -->
<!-- PLACEHOLDER_SKILLS -->

<!-- TREE DIVIDER 3 -->
<!-- PLACEHOLDER_DIVIDER_3 -->

<!-- ── QUEST LOG ── -->
<!-- PLACEHOLDER_QUESTS -->

<!-- TREE DIVIDER 4 -->
<!-- PLACEHOLDER_DIVIDER_4 -->

<!-- ── TREASURE CHEST ── -->
<!-- PLACEHOLDER_TREASURE -->

<!-- TREE DIVIDER 5 -->
<!-- PLACEHOLDER_DIVIDER_5 -->

<!-- ── VALLEY FOLKS ── -->
<!-- PLACEHOLDER_FOLKS -->

<!-- TREE DIVIDER 6 -->
<!-- PLACEHOLDER_DIVIDER_6 -->

<!-- ── ADVENTURE LOG ── -->
<!-- PLACEHOLDER_ADVENTURE -->

<!-- TREE DIVIDER 7 -->
<!-- PLACEHOLDER_DIVIDER_7 -->

<!-- ── CONTACT ── -->
<!-- PLACEHOLDER_CONTACT -->

<!-- ── FOOTER ── -->
<!-- PLACEHOLDER_FOOTER -->

<!-- ── MUSIC PLAYER ── -->
<!-- PLACEHOLDER_MUSIC -->

<!-- ── JAVASCRIPT ── -->
<!-- PLACEHOLDER_JS -->

</body>
</html>
```

- [ ] **Step 2: Verify skeleton opens without errors**
Open `/Users/dududu/portfolio/index.html` in a browser. You should see a nav bar, the sky gradient background, drifting clouds. No JS errors in console.

---

### Task 3: Hero Section

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_HERO -->`

Read the portrait base64 from `/private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64` and store the full string as `PORTRAIT_B64`.

- [ ] **Step 1: Read the portrait base64**
```bash
cat /private/tmp/claude-501/-Users-dududu/437b2026-eaca-4dd1-8cc9-28441524e81f/scratchpad/portrait.b64
```
Copy the entire output (the long base64 string). You will paste it into the `src` attribute below.

- [ ] **Step 2: Replace `<!-- PLACEHOLDER_HERO -->` with the following block**

Replace the comment with (substitute `PORTRAIT_B64` with the actual base64 string):

```html
<section id="home">
  <!-- Sky scene with trees and Junimos -->
  <div class="hero-scene">

    <!-- Background pixel trees -->
    <div class="pixel-tree" style="left:2%;bottom:80px;position:absolute">
      <div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div>
      <div class="trunk"></div>
    </div>
    <div class="pixel-tree" style="left:12%;bottom:80px;position:absolute">
      <div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div>
      <div class="trunk"></div>
    </div>
    <div class="pixel-tree" style="right:3%;bottom:80px;position:absolute">
      <div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div>
      <div class="trunk"></div>
    </div>
    <div class="pixel-tree" style="right:14%;bottom:80px;position:absolute">
      <div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div>
      <div class="trunk"></div>
    </div>

    <!-- Junimos (green, yellow, red, purple, blue, orange) -->
    <div class="junimo-wrap" style="left:5%; --hop-dur:0.9s; --hop-delay:0s">
      <svg viewBox="0 0 24 30" width="64" height="80" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#3db33d" transform="rotate(-20,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#3db33d"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#5de65d"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#3db33d"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#3db33d"/>
      </svg>
    </div>

    <div class="junimo-wrap" style="left:18%; --hop-dur:1.1s; --hop-delay:0.2s">
      <svg viewBox="0 0 24 30" width="56" height="70" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#c8920a" transform="rotate(15,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#c8920a"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#f9ca24"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#c8920a"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#c8920a"/>
      </svg>
    </div>

    <div class="junimo-wrap" style="left:32%; --hop-dur:0.8s; --hop-delay:0.4s">
      <svg viewBox="0 0 24 30" width="60" height="75" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#aa2020" transform="rotate(-10,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#aa2020"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#ff6b6b"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#aa2020"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#aa2020"/>
      </svg>
    </div>

    <div class="junimo-wrap" style="right:30%; --hop-dur:1.0s; --hop-delay:0.1s">
      <svg viewBox="0 0 24 30" width="58" height="73" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#8040c0" transform="rotate(20,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#8040c0"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#c084fc"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#8040c0"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#8040c0"/>
      </svg>
    </div>

    <div class="junimo-wrap" style="right:16%; --hop-dur:0.85s; --hop-delay:0.3s">
      <svg viewBox="0 0 24 30" width="62" height="78" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#1a60d0" transform="rotate(-15,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#1a60d0"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#60a5fa"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#1a60d0"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#1a60d0"/>
      </svg>
    </div>

    <div class="junimo-wrap" style="right:4%; --hop-dur:0.95s; --hop-delay:0.5s">
      <svg viewBox="0 0 24 30" width="58" height="73" style="image-rendering:pixelated">
        <ellipse cx="12" cy="3" rx="5" ry="3" fill="#c05a0a" transform="rotate(10,12,3)"/>
        <rect x="11" y="4" width="2" height="5" fill="#c05a0a"/>
        <ellipse cx="12" cy="18" rx="11" ry="12" fill="#fb923c"/>
        <rect x="7" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <rect x="14" y="13" width="3" height="3" rx="1" fill="#1a0a00"/>
        <ellipse cx="7" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <ellipse cx="17" cy="18" rx="2.5" ry="1.5" fill="#ffaaaa" opacity="0.5"/>
        <rect x="7" y="27" width="4" height="3" rx="1" fill="#c05a0a"/>
        <rect x="13" y="27" width="4" height="3" rx="1" fill="#c05a0a"/>
      </svg>
    </div>

    <!-- Grass strip with flowers -->
    <div class="hero-grass">
      <div class="flower" style="left:8%;background:#ff6b9d"></div>
      <div class="flower" style="left:22%;background:#f9ca24"></div>
      <div class="flower" style="left:45%;background:#ff6b9d;width:10px;height:10px"></div>
      <div class="flower" style="left:65%;background:#fff"></div>
      <div class="flower" style="left:80%;background:#f9ca24;width:10px;height:10px"></div>
      <div class="flower" style="right:6%;background:#ff6b9d"></div>
    </div>
  </div>

  <!-- Portrait card -->
  <div class="portrait-card">
    <img src="data:image/webp;base64,PORTRAIT_B64" alt="Xiaojing Du — pixel art portrait" loading="eager">
  </div>

  <!-- Dialogue box -->
  <div class="dialogue-box">
    <span id="dialogue-text"></span><span class="cursor-blink"></span>
  </div>

  <!-- CTA buttons -->
  <div style="margin-top:20px">
    <a href="#about" class="pixel-btn">[ Enter the Valley ]</a>
    <a href="#quests" class="pixel-btn gold">[ Read the Quest Log ]</a>
  </div>
</section>
```

- [ ] **Step 3: Replace all five tree divider placeholders**

Replace each `<!-- PLACEHOLDER_DIVIDER_N -->` (N = 1–7) with:
```html
<div class="tree-divider" aria-hidden="true">
  <div class="pixel-tree sm"><div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div><div class="trunk"></div></div>
  <div class="pixel-tree"><div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div><div class="trunk"></div></div>
  <div class="pixel-tree sm"><div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div><div class="trunk"></div></div>
  <div class="pixel-tree"><div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div><div class="trunk"></div></div>
  <div class="pixel-tree sm"><div class="canopy"><div class="c1"></div><div class="c2"></div><div class="c3"></div></div><div class="trunk"></div></div>
</div>
```

- [ ] **Step 4: Open in browser, verify**
- Junimos should be visible, bouncing at different rates
- Portrait card should show Xiaojing's pixel art image
- Trees should appear left and right of the scene
- Grass strip with flowers at bottom of hero

---

### Task 4: About + Skills Sections

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_ABOUT -->` and `<!-- PLACEHOLDER_SKILLS -->`

- [ ] **Step 1: Replace `<!-- PLACEHOLDER_ABOUT -->`**

```html
<section id="about">
  <h2 class="section-title">📖 Farmer's Journal</h2>
  <p class="section-sub">a little about the farmer</p>

  <div class="about-grid">
    <div class="char-card reveal">
      <div style="font-size:64px">🧑‍🌾</div>
      <div class="char-name">Xiaojing Du</div>
      <div class="char-title">PhD Researcher · Cambridge</div>
      <div class="char-stats">
        <div class="stat-row"><span>🔬 Specialty</span><span>Phonetics</span></div>
        <div class="stat-row"><span>🌍 Origin</span><span>China</span></div>
        <div class="stat-row"><span>🏡 Valley</span><span>Cambridge</span></div>
        <div class="stat-row"><span>⭐ Level</span><span>PhD yr 2</span></div>
        <div class="stat-row"><span>📧 Dove</span><span>xed20@cam.ac.uk</span></div>
      </div>
    </div>

    <div class="journal-panel reveal">
      <h3>✦ Entry — Summer, Year 2</h3>
      <p>Hello there! I'm Xiaojing — a phonetician and PhD researcher at the <strong>Cambridge Phonetics Laboratory</strong>, University of Cambridge. I spend my days farming acoustic data, measuring how voices shimmer and shift: from the tones of Jin Chinese to the subtle breathiness hiding in everyday English speech.</p>
      <p>Before arriving in this valley, I studied at <strong>Wake Forest University</strong>, earning a B.S. in Mathematical Statistics &amp; Linguistics in 2024 — where I first discovered that language and mathematics could grow side by side in the same field.</p>
      <p>These days I tend two main plots: a <em>frame-level voice quality classifier</em> that teaches machines to hear what phoneticians hear, and the <em>tones of Huojia Jin Chinese</em>, a dialect with secrets buried deep in its mountain terroir. It's honest work, and the harvest has been good.</p>

      <div class="edu-timeline">
        <div class="edu-entry">
          <div class="edu-star">⭐</div>
          <div>
            <div class="edu-year">2025 – Present</div>
            <div><strong>PhD, Linguistics</strong></div>
            <div class="edu-place">University of Cambridge · Clare Hall</div>
          </div>
        </div>
        <div class="edu-entry">
          <div class="edu-star">🌟</div>
          <div>
            <div class="edu-year">2020 – 2024</div>
            <div><strong>B.S. Mathematical Statistics &amp; Linguistics</strong></div>
            <div class="edu-place">Wake Forest University · Winston-Salem, NC</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Replace `<!-- PLACEHOLDER_SKILLS -->`**

```html
<section id="skills">
  <h2 class="section-title">🛠️ Equipped Tools</h2>
  <p class="section-sub">items in the farmer's kit</p>

  <div class="skills-grid">
    <div class="skill-card reveal">
      <div class="skill-name">🐍 Python</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:90%"></div></div>
      <div class="skill-level">Expert · Lv 9</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">📊 R</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:85%"></div></div>
      <div class="skill-level">Advanced · Lv 8</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">🔢 MATLAB</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:80%"></div></div>
      <div class="skill-level">Advanced · Lv 8</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">🎙️ Praat</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:95%"></div></div>
      <div class="skill-level">Expert · Lv 9</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">🧠 PyTorch / HuggingFace</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:75%"></div></div>
      <div class="skill-level">Proficient · Lv 7</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">📈 Functional Data Analysis</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:80%"></div></div>
      <div class="skill-level">Advanced · Lv 8</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">🗣️ Acoustic Phonetics</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:95%"></div></div>
      <div class="skill-level">Expert · Lv 9</div>
    </div>
    <div class="skill-card reveal">
      <div class="skill-name">🌐 Mandarin / English / Jin</div>
      <div class="skill-bar-bg"><div class="skill-bar-fill" style="width:100%"></div></div>
      <div class="skill-level">Native / Fluent · Lv 10</div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Open browser, verify**
Both sections should appear below the hero. Journal text is legible. Skill bars are visible and gold-filled.

---

### Task 5: Quest Log + Treasure Chest

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_QUESTS -->` and `<!-- PLACEHOLDER_TREASURE -->`

- [ ] **Step 1: Replace `<!-- PLACEHOLDER_QUESTS -->`**

```html
<section id="quests">
  <h2 class="section-title">📜 Quest Log</h2>
  <p class="section-sub">completed and in-progress quests</p>
  <p class="reveal" style="text-align:center;color:var(--parchment);font-size:20px;margin-bottom:24px">Every researcher's farm grows through the quests they complete. Tap any scroll to read more. ▾</p>

  <div class="quest-card reveal" onclick="this.classList.toggle('open')">
    <div class="quest-top">
      <div class="quest-icon">🌾</div>
      <div>
        <span class="quest-tag">MAIN QUEST · 2025 · Under Review</span>
        <h4>Phonation in Jin Chinese: A Frame-Level Voice Quality Analysis</h4>
        <p class="authors">Du, X., Post, B., &amp; Nolan, F.</p>
        <p class="venue">Submitted to Journal of Phonetics</p>
      </div>
    </div>
    <div class="quest-detail">The first study to apply frame-level voice quality probes (trained on SSL features) to Jin Chinese tones. Reveals that creaky phonation is not merely paralinguistic but tonally contrastive in this under-studied Sinitic variety.</div>
    <div class="toggle-hint">▾ tap to expand</div>
  </div>

  <div class="quest-card reveal" onclick="this.classList.toggle('open')">
    <div class="quest-top">
      <div class="quest-icon">🗺️</div>
      <div>
        <span class="quest-tag">MAIN QUEST · 2024</span>
        <h4>Automatic Frame-Level Voice Quality Labelling Using Self-Supervised Speech Representations</h4>
        <p class="authors">Du, X., Post, B., &amp; Nolan, F.</p>
        <p class="venue">Interspeech 2024, Kos, Greece</p>
      </div>
    </div>
    <div class="quest-detail">Introduced a frame-level VQ labelling pipeline using SSL models (wav2vec 2.0, HuBERT) fine-tuned on annotated corpora. Demonstrated that mid-network layers carry the richest phonation-type signal — a key finding for interpretable voice AI.</div>
    <div class="toggle-hint">▾ tap to expand</div>
  </div>

  <div class="quest-card reveal" onclick="this.classList.toggle('open')">
    <div class="quest-top">
      <div class="quest-icon">🌻</div>
      <div>
        <span class="quest-tag">SIDE QUEST · 2025</span>
        <h4>Tonal Perception and the Huojia Minimal Pair: Listener Sensitivity to Phonation Cues</h4>
        <p class="authors">Du, X., Post, B., &amp; Nolan, F.</p>
        <p class="venue">ICPhS 2027 (in preparation)</p>
      </div>
    </div>
    <div class="quest-detail">A perception experiment using resynthesised Huojia Jin minimal pairs to isolate which acoustic dimensions (F0, VQ, duration) listeners use to distinguish tones. Directly tests Lu et al.'s theoretical claims with Cambridge participants.</div>
    <div class="toggle-hint">▾ tap to expand</div>
  </div>

  <div class="quest-card reveal" onclick="this.classList.toggle('open')">
    <div class="quest-top">
      <div class="quest-icon">🔭</div>
      <div>
        <span class="quest-tag">CROSSOVER QUEST · 2025</span>
        <h4>Galaxy Mergers in the Epoch of Reionization: Merger Rates at z = 4.5–8.5</h4>
        <p class="authors">Duan, Q., Conselice, C. J., et al., incl. Du, X.</p>
        <p class="venue">Monthly Notices of the Royal Astronomical Society, 540, 774–805</p>
      </div>
    </div>
    <div class="quest-detail">Statistical contribution to a large-scale JWST galaxy survey — proof that a phonetician's statistics toolkit travels well beyond linguistics. A detour into astrophysics that unexpectedly broadened the research horizon.</div>
    <div class="toggle-hint">▾ tap to expand</div>
  </div>

  <div class="quest-card reveal" onclick="this.classList.toggle('open')">
    <div class="quest-top">
      <div class="quest-icon">🔭</div>
      <div>
        <span class="quest-tag">CROSSOVER QUEST · 2026</span>
        <h4>Galaxy Mergers in the Epoch of Reionization II: Merger-Triggered Star Formation and AGN Activities at z = 4.5–8.5</h4>
        <p class="authors">Duan, Q., Conselice, C. J., et al., incl. Du, X.</p>
        <p class="venue">Monthly Notices of the Royal Astronomical Society, 546, stag008</p>
      </div>
    </div>
    <div class="quest-detail">A follow-up statistical contribution continuing the JWST galaxy-merger collaboration. Same rigour, bigger cosmic stakes.</div>
    <div class="toggle-hint">▾ tap to expand</div>
  </div>
</section>
```

- [ ] **Step 2: Replace `<!-- PLACEHOLDER_TREASURE -->`**

```html
<section id="treasure">
  <h2 class="section-title">💰 Treasure Chest</h2>
  <p class="section-sub">loot found along the journey</p>

  <div class="treasure-grid">
    <div class="treasure-tile reveal">
      <span class="year-badge">2026</span>
      <span class="trophy">🏅</span>
      <div class="amount">£5,000</div>
      <h4>Cambridge Language Sciences Incubator Fund</h4>
      <div class="org">University of Cambridge</div>
      <div class="desc">PI · "Hearing Every Voice: Interpretable Voice Quality Labelling for Inclusive AI"</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2026</span>
      <span class="trophy">🎪</span>
      <div class="amount">£500</div>
      <h4>TAL Research Area Workshop Fund</h4>
      <div class="org">University of Cambridge</div>
      <div class="desc">Lead organiser · Tone, Stress and Prosody Workshop</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2026</span>
      <span class="trophy">🎒</span>
      <div class="amount">£500</div>
      <h4>Boak Student Support Fund</h4>
      <div class="org">Clare Hall, University of Cambridge</div>
      <div class="desc">Conference travel &amp; presentation</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2026</span>
      <span class="trophy">🧭</span>
      <div class="amount">£400</div>
      <h4>MMLL PhD Research Fund Award</h4>
      <div class="org">University of Cambridge</div>
      <div class="desc">Fieldwork &amp; data collection</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2025–</span>
      <span class="trophy">🎓</span>
      <div class="amount">Scholar</div>
      <h4>Cambridge Trust Scholarship</h4>
      <div class="org">University of Cambridge</div>
      <div class="desc">Doctoral study funding</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2025–</span>
      <span class="trophy">🌏</span>
      <div class="amount">Scholar</div>
      <h4>China Scholarship Council Scholarship</h4>
      <div class="org">China Scholarship Council</div>
      <div class="desc">Overseas doctoral study funding</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2023</span>
      <span class="trophy">💰</span>
      <div class="amount">$4,000</div>
      <h4>Wake Forest Research Fellowship</h4>
      <div class="org">Wake Forest University</div>
      <div class="desc">Community structure of tense vowels, Central Plains dialect</div>
    </div>
    <div class="treasure-tile reveal">
      <span class="year-badge">2022</span>
      <span class="trophy">🖋️</span>
      <div class="amount">$2,000</div>
      <h4>Wake Forest Arts &amp; Humanities Research Fellowship</h4>
      <div class="org">Wake Forest University</div>
      <div class="desc">Vocalic differences via vowel distance &amp; Partitioned Local Depth</div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser** — Quest cards should be clickable (toggle detail). Treasure tiles should display in a grid.

---

### Task 6: Valley Folks + NPC Sprites

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_FOLKS -->`

The NPC sprites are built via a JS template function. This section contains a `<div id="npc-container">` that JavaScript populates in Task 8. Here we add the section shell.

- [ ] **Step 1: Replace `<!-- PLACEHOLDER_FOLKS -->`**

```html
<section id="folks">
  <h2 class="section-title">🏘️ Valley Folks</h2>
  <p class="section-sub">the NPCs who helped this farm grow</p>

  <!-- NPC cards injected here by renderNPCs() in JS -->
  <div id="npc-container"></div>
</section>
```

The actual NPC data and rendering JS goes in Task 8. Do not add individual NPC cards here manually.

- [ ] **Step 2: Verify placeholder exists**
Open browser; the Valley Folks section header should appear with an empty body (no cards yet — that's expected).

---

### Task 7: Adventure Log + Contact + Footer

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_ADVENTURE -->`, `<!-- PLACEHOLDER_CONTACT -->`, `<!-- PLACEHOLDER_FOOTER -->`

- [ ] **Step 1: Replace `<!-- PLACEHOLDER_ADVENTURE -->`**

```html
<section id="adventure">
  <h2 class="section-title">🗺️ Adventure Log</h2>
  <p class="section-sub">expeditions and guild service</p>

  <div class="adv-list">
    <div class="adv-card reveal">
      <div class="adv-icon">🎙️</div>
      <div>
        <h4>Voice Quality and Tone in Jin Chinese</h4>
        <div class="meta">University of Cambridge · 2025 – Present</div>
        <ul>
          <li>Investigating phonation type in Jin Chinese tones, contributing speech-science evidence from an under-studied Sinitic variety.</li>
          <li>Analysing F0, duration, HNR, H1–H2, CPP, and frame-level voice quality measures alongside SSL-derived probes.</li>
        </ul>
        <span class="status-badge">⚔️ Active Quest</span>
      </div>
    </div>

    <div class="adv-card reveal">
      <div class="adv-icon">🗣️</div>
      <div>
        <h4>Research Intern · TTS and Wu Dialect Conversion</h4>
        <div class="meta">Bank of Beijing · 2024</div>
        <ul>
          <li>Analysed Shanghainese speech recordings for dialectal text-to-speech development.</li>
          <li>Built aligned datasets from human speech and TTS outputs for dialect-specific modelling.</li>
          <li>Applied t-SNE and AdapterHub methods to speech synthesis adaptation.</li>
        </ul>
        <span class="status-badge done">✅ Quest Complete</span>
      </div>
    </div>

    <div class="adv-card reveal">
      <div class="adv-icon">🌽</div>
      <div>
        <h4>Tense Vowels in the Central Plains Dialect</h4>
        <div class="meta">Wake Forest University · 2023 – Present</div>
        <ul>
          <li>Analysed speech data from 20 native speakers of Henan Mandarin using acoustic analysis, PCA, and Partitioned Local Depth.</li>
          <li>Developed functional data analysis methods for tonal contours and short time-series variation.</li>
        </ul>
        <span class="status-badge">⚔️ Active Quest</span>
      </div>
    </div>

    <div class="adv-card reveal">
      <div class="adv-icon">📅</div>
      <div>
        <h4>Coordinator · Phonetics and Phonology Seminar</h4>
        <div class="meta">University of Cambridge · 2025 – 2026</div>
        <ul>
          <li>Coordinated the weekly Phonetics and Phonology Seminar series at the Cambridge Phonetics Laboratory.</li>
          <li>Handled speaker invitations, scheduling, and hybrid delivery for 30+ sessions.</li>
        </ul>
        <span class="status-badge guild">🛡️ Guild Duty</span>
      </div>
    </div>

    <div class="adv-card reveal">
      <div class="adv-icon">🛠️</div>
      <div>
        <h4>Lead Organiser · Tone, Stress and Prosody Workshop</h4>
        <div class="meta">University of Cambridge · 2026</div>
        <ul>
          <li>Organised a one-day interdisciplinary workshop on tone, stress, prosody, speech perception, fieldwork methods, and computational approaches.</li>
        </ul>
        <span class="status-badge done">✅ Quest Complete</span>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Replace `<!-- PLACEHOLDER_CONTACT -->`**

```html
<section id="contact">
  <h2 class="section-title">🕊️ Send a Dove</h2>
  <p class="section-sub">dispatch a message to the farmer</p>

  <div class="pixel-panel mailbox-wrap reveal">
    <span class="mailbox-icon" onclick="revealEmail()" title="Click to reveal email">📬</span>
    <p style="font-size:22px;color:var(--ink-soft)">The mailbox is always open.<br>Click the envelope to reveal my address.</p>
    <div class="email-reveal" id="emailReveal">xed20@cam.ac.uk</div>
    <p style="margin-top:18px;font-size:20px;color:var(--ink-soft)">
      Cambridge Phonetics Laboratory<br>
      Theoretical &amp; Applied Linguistics<br>
      University of Cambridge
    </p>
  </div>
</section>
```

- [ ] **Step 3: Replace `<!-- PLACEHOLDER_FOOTER -->`**

```html
<footer>
  <p>🌾 Grown with curiosity, coffee, and Stardew Valley magic. 🌾</p>
  <p style="margin-top:6px;opacity:0.7">© 2026 Xiaojing Du · Cambridge Phonetics Laboratory</p>
</footer>
```

- [ ] **Step 4: Verify in browser** — Adventure log cards appear. Contact section shows animated mailbox. Footer is dark wood.

---

### Task 8: Music Player + All JavaScript

**Files:**
- Modify: `/Users/dududu/portfolio/index.html` — replace `<!-- PLACEHOLDER_MUSIC -->` and `<!-- PLACEHOLDER_JS -->`

- [ ] **Step 1: Replace `<!-- PLACEHOLDER_MUSIC -->`**

```html
<div class="music-player" role="region" aria-label="Music player">
  <span class="note-float" aria-hidden="true">♪</span>
  <button id="playBtn" onclick="togglePlay()" title="Play/Pause music">▶</button>
  <span style="font-size:9px;max-width:120px;overflow:hidden;white-space:nowrap">Summer · ConcernedApe</span>
  <input type="range" id="volSlider" min="0" max="1" step="0.05" value="0.35" title="Volume" oninput="setVol(this.value)">
  <audio id="bgMusic" loop>
    <source src="audio/summer.mp3" type="audio/mpeg">
  </audio>
</div>
```

- [ ] **Step 2: Replace `<!-- PLACEHOLDER_JS -->`**

```html
<script>
/* ── Typewriter dialogue ── */
const lines = [
  "Oh! A visitor! Welcome to Phonetics Valley! ♪",
  "I'm Xiaojing — I farm acoustic data and grow tones...",
  "Make yourself at home. The valley has much to discover! ★"
];
let lineIdx = 0, charIdx = 0, typing = true;
const dialogueEl = document.getElementById('dialogue-text');

function typeNext() {
  const line = lines[lineIdx];
  if (typing) {
    dialogueEl.textContent = line.slice(0, charIdx + 1);
    charIdx++;
    if (charIdx >= line.length) {
      typing = false;
      setTimeout(() => { typing = false; }, 1800);
      setTimeout(advanceLine, 2800);
    } else {
      setTimeout(typeNext, 55);
    }
  }
}
function advanceLine() {
  lineIdx = (lineIdx + 1) % lines.length;
  charIdx = 0;
  typing = true;
  dialogueEl.textContent = '';
  setTimeout(typeNext, 200);
}
setTimeout(typeNext, 600);

/* ── Scroll reveal ── */
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); } });
}, { threshold: 0.12 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

/* ── Email reveal ── */
function revealEmail() {
  const el = document.getElementById('emailReveal');
  el.style.display = el.style.display === 'block' ? 'none' : 'block';
}

/* ── Music player ── */
const audio = document.getElementById('bgMusic');
const playBtn = document.getElementById('playBtn');
audio.volume = 0.35;

function togglePlay() {
  if (audio.paused) {
    audio.play().then(() => { playBtn.textContent = '⏸'; }).catch(() => {});
  } else {
    audio.pause();
    playBtn.textContent = '▶';
  }
}
function setVol(v) { audio.volume = parseFloat(v); }

/* ── NPC sprite renderer ── */
function npcSVG(hair, skin, outfit, pants, extra) {
  return `<svg viewBox="0 0 16 32" width="96" height="192" style="image-rendering:pixelated;display:block;margin:0 auto">
    <rect x="3"  y="0"  width="10" height="1"  fill="${hair}"/>
    <rect x="2"  y="1"  width="12" height="3"  fill="${hair}"/>
    <rect x="2"  y="4"  width="12" height="6"  fill="${skin}"/>
    <rect x="4"  y="6"  width="2"  height="2"  fill="#1a0a00"/>
    <rect x="10" y="6"  width="2"  height="2"  fill="#1a0a00"/>
    <rect x="3"  y="8"  width="2"  height="1"  fill="#ffb3ba" opacity="0.6"/>
    <rect x="11" y="8"  width="2"  height="1"  fill="#ffb3ba" opacity="0.6"/>
    <rect x="6"  y="10" width="4"  height="2"  fill="${skin}"/>
    <rect x="2"  y="12" width="12" height="9"  fill="${outfit}"/>
    <rect x="0"  y="12" width="2"  height="8"  fill="${outfit}"/>
    <rect x="14" y="12" width="2"  height="8"  fill="${outfit}"/>
    <rect x="0"  y="20" width="2"  height="2"  fill="${skin}"/>
    <rect x="14" y="20" width="2"  height="2"  fill="${skin}"/>
    <rect x="2"  y="21" width="12" height="1"  fill="${pants}"/>
    <rect x="3"  y="22" width="4"  height="7"  fill="${pants}"/>
    <rect x="9"  y="22" width="4"  height="7"  fill="${pants}"/>
    <rect x="7"  y="22" width="2"  height="7"  fill="#1a0a00" opacity="0.15"/>
    <rect x="2"  y="29" width="5"  height="3"  fill="#3d2b1f"/>
    <rect x="9"  y="29" width="5"  height="3"  fill="#3d2b1f"/>
    ${extra || ''}
  </svg>`;
}

const NPCS = [
  {
    group: 0, name: "Prof. Brechtje Post", role: "PhD Supervisor · Cambridge", hearts: 10,
    hair:"#d4a017", skin:"#f5cba7", outfit:"#7b2d8b", pants:"#2c3e50",
    extra:'<rect x="2" y="12" width="12" height="2" fill="#9b4dab"/>'
  },
  {
    group: 0, name: "Prof. Francis Nolan", role: "PhD Supervisor · Cambridge", hearts: 10,
    hair:"#9e9e9e", skin:"#f5cba7", outfit:"#1a3a5c", pants:"#1a2733",
    extra:'<rect x="7" y="13" width="2" height="7" fill="#4a8abf" opacity="0.5"/>'
  },
  {
    group: 1, name: "Prof. Kenneth Berenhaut", role: "Statistics Mentor · Wake Forest", hearts: 8,
    hair:"#5d3a1a", skin:"#f5cba7", outfit:"#17867d", pants:"#1a3a2c", extra:''
  },
  {
    group: 1, name: "Prof. Jeff Mielke", role: "Phonetics Mentor · Wake Forest", hearts: 8,
    hair:"#3d2b1f", skin:"#d4a574", outfit:"#2d6a4f", pants:"#1a3020", extra:''
  },
  {
    group: 1, name: "Dr. César Gutiérrez", role: "Research Mentor · Wake Forest", hearts: 7,
    hair:"#1a1a1a", skin:"#c8a07a", outfit:"#f39c12", pants:"#1a2733",
    extra:'<rect x="2" y="12" width="12" height="9" fill="#e67e22" opacity="0.25"/>'
  },
  {
    group: 1, name: "Dr. Sneha Jadhav", role: "Research Mentor · Wake Forest", hearts: 7,
    hair:"#1a1a1a", skin:"#b5845a", outfit:"#e91e8c", pants:"#2c3e50",
    extra:'<rect x="14" y="4" width="2" height="3" fill="#e91e8c"/>'
  },
  {
    group: 1, name: "Dr. Tiffany Judy", role: "Research Mentor · Wake Forest", hearts: 7,
    hair:"#8b3a0f", skin:"#f5cba7", outfit:"#f9ca24", pants:"#2c3e50", extra:''
  },
  {
    group: 2, name: "C. McGhee", role: "Voice Quality Project", hearts: 6,
    hair:"#c0392b", skin:"#f5cba7", outfit:"#1565c0", pants:"#1a2733", extra:''
  },
  {
    group: 2, name: "M. Qian", role: "Voice Quality Project", hearts: 6,
    hair:"#1a1a1a", skin:"#d4a574", outfit:"#6a0dad", pants:"#2c3e50", extra:''
  },
  {
    group: 2, name: "L. Shahidi", role: "Voice Quality Project", hearts: 6,
    hair:"#3d2b1f", skin:"#c8a07a", outfit:"#ff6b6b", pants:"#2c3e50", extra:''
  },
  {
    group: 2, name: "C. Xu", role: "Voice Quality Project", hearts: 6,
    hair:"#1a1a1a", skin:"#d4a574", outfit:"#43a047", pants:"#1a3020", extra:''
  },
  {
    group: 2, name: "Q. Duan & JWST Team", role: "Galaxy Mergers · Statistical Collab", hearts: 5,
    hair:"#1a1a1a", skin:"#d4a574", outfit:"#0d1b2a", pants:"#0a0f1a",
    extra:`<rect x="4" y="14" width="1" height="1" fill="white" opacity="0.8"/>
           <rect x="9" y="16" width="1" height="1" fill="white" opacity="0.8"/>
           <rect x="7" y="13" width="1" height="1" fill="white" opacity="0.8"/>
           <rect x="11" y="18" width="1" height="1" fill="white" opacity="0.6"/>`
  }
];

const groupMeta = [
  { title: "🏫 The Elders", sub: "PhD supervisors" },
  { title: "🌻 Mentors from the Old Valley", sub: "Wake Forest University" },
  { title: "🤝 Fellow Farmers", sub: "collaborators & co-authors" }
];

function renderNPCs() {
  const container = document.getElementById('npc-container');
  const groups = [[], [], []];
  NPCS.forEach(n => groups[n.group].push(n));

  groups.forEach((members, gi) => {
    const meta = groupMeta[gi];
    const groupDiv = document.createElement('div');
    groupDiv.className = 'folks-group reveal';
    const hearts = n => '♥'.repeat(n.hearts) + '<span style="opacity:0.3">' + '♥'.repeat(10 - n.hearts) + '</span>';
    groupDiv.innerHTML = `
      <div class="folks-group-title">${meta.title}</div>
      <div style="text-align:center;color:var(--parchment);font-size:18px;margin-bottom:16px">${meta.sub}</div>
      <div class="npc-grid">
        ${members.map(n => `
          <div class="npc-card">
            ${npcSVG(n.hair, n.skin, n.outfit, n.pants, n.extra)}
            <div class="npc-name">${n.name}</div>
            <div class="npc-role">${n.role}</div>
            <div class="npc-hearts" style="color:var(--heart)">${hearts(n)}</div>
          </div>
        `).join('')}
      </div>`;
    container.appendChild(groupDiv);
  });

  // Re-observe newly added reveal elements
  container.querySelectorAll('.reveal').forEach(el => observer.observe(el));
}
renderNPCs();
</script>
```

- [ ] **Step 3: Verify in browser**
- Typewriter text appears in dialogue box and cycles through 3 messages
- Scroll down: sections fade in on scroll
- Valley Folks: 12 NPC pixel-art characters appear in 3 groups with hearts
- Music player in bottom-right: click ▶ to start Summer track
- Mailbox in contact section: click to toggle email

---

### Task 9: Final Verification

**Files:** Read-only check of `/Users/dududu/portfolio/index.html`

- [ ] **Step 1: Check file exists and has reasonable size**
```bash
wc -c /Users/dududu/portfolio/index.html
```
Expected: > 500,000 bytes (large because of embedded base64 portrait).

- [ ] **Step 2: Confirm audio file in place**
```bash
ls -lh /Users/dududu/portfolio/audio/summer.mp3
```
Expected: file exists, size matches Downloads original.

- [ ] **Step 3: Open and do a full page walk-through**
Open `file:///Users/dududu/portfolio/index.html` in Safari or Chrome.

Check each section:
1. **Hero** — Junimos bouncing, portrait card showing Xiaojing's pixel art image, typewriter dialogue cycling
2. **Journal** — Bio text readable, character card shows, education timeline visible
3. **Tools** — 8 skill bars rendered with gold fill
4. **Quest Log** — 5 quest cards; clicking each expands detail
5. **Treasure Chest** — 8 tiles in grid
6. **Valley Folks** — 3 groups, 12 SVG NPC sprites, hearts coloured pink
7. **Adventure Log** — 5 entries with coloured status badges
8. **Send a Dove** — clicking mailbox reveals email
9. **Music player** — clicking ▶ plays summer.mp3 (confirm in browser audio permissions)
10. **Tree dividers** — visible between all sections

- [ ] **Step 4: Check no broken images or console errors**
Open DevTools > Console. Expected: no red errors. Portrait `<img>` should render (not show broken icon).

- [ ] **Step 5: Commit**
```bash
cd /Users/dududu/portfolio
git init
git add index.html audio/summer.mp3
git commit -m "feat: Stardew Valley portfolio — Junimo hero, SVG NPCs, summer palette, game-speak wording"
```

---

## Self-Review

**Spec coverage:**
- ✅ Bright Stardew palette (new CSS vars)
- ✅ Junimo hero scene (6 SVG Junimos, 4 trees, flowers)
- ✅ Portrait embedded as base64 WebP
- ✅ All 8 sections with game-speak NPC wording
- ✅ 12 NPC SVG sprites via JS template
- ✅ Tree dividers between sections
- ✅ Music player wired to `audio/summer.mp3`
- ✅ Scroll reveal animations
- ✅ Quest card expand/collapse
- ✅ Email reveal on click

**No placeholders:** All steps contain actual code. No TBDs.

**Type consistency:** `npcSVG()` defined in Task 8, called in `renderNPCs()` in same script block. `observer` defined before `renderNPCs()` call so re-observation works. `dialogueEl`, `audio`, `playBtn` all defined before use.
