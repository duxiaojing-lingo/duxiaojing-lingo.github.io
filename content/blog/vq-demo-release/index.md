---
title: 'VQ Frame Detector: demo repo released'
date: '2026-06-25'
summary: 'The VQ Frame Detector demo is now publicly available on GitHub. Clone, install, and run the Streamlit app to detect creaky, breathy, and whispery phonation in your own recordings.'
tags:
  - Voice Quality
  - Demo
  - SLT 2026
authors:
  - me
---

The demo code for our **VQ Frame Detector** is now public at
[github.com/duxiaojing-lingo/vq_frame_detector](https://github.com/duxiaojing-lingo/vq_frame_detector).

The app runs entirely locally — just clone and run:

```bash
pip install -r requirements.txt
streamlit run app.py
```

WavLM-large (~1.3 GB) downloads automatically on first use.
The trained V5 probe bundle is included in the repo (`models/probes_v5.joblib`).

**What the demo does:**
- Detects creaky, breathy, and whispery phonation at 50 Hz
- Export per-frame predictions as CSV or Praat TextGrid
- Acoustic MDS explorer (no labels required)
- Segment-by-segment annotation correction tab

This demo accompanies our SLT 2026 submission on frame-level voice quality detection with class-specific WavLM layer probes.
