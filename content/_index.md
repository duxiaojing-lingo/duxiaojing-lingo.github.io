---
title: ''
summary: ''
date: 2026-06-25
type: landing

sections:
  # ── Bio ──────────────────────────────────────────────────────────────────
  - block: resume-biography-3
    content:
      username: me
      text: ''
      button:
        text: Download CV
        url: uploads/cv.pdf
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      background:
        gradient_mesh:
          enable: true
      name:
        size: md
      avatar:
        size: medium
        shape: circle

  # ── Current Work ─────────────────────────────────────────────────────────
  - block: markdown
    id: current-work
    content:
      title: 'Current Work'
      subtitle: ''
      text: |-
        ### VQ Frame Detector — SLT 2026
        Frame-level voice quality detection (creaky · breathy · whispery) at 50 Hz using a WavLM-large probe (V5).
        Trained on 27 naturalistic speakers + RAVDESS + controlled VQ recordings.
        → [Demo repo](https://github.com/duxiaojing-lingo/vq_frame_detector)

        ### Layer Analysis for ICASSP 2027
        Investigating which WavLM layers best encode phonation type, with soft-label supervision and a three-tier corpus designed for ecological validity.

        ### VQ Annotation Corpus
        Building a large-scale frame-level phonation corpus across naturalistic, controlled, and acted speech with continuous-gradient labels.
    design:
      columns: '1'

  # ── Featured Publications ─────────────────────────────────────────────────
  - block: collection
    id: papers
    content:
      title: Publications
      filters:
        folders:
          - publications
        featured_only: true
    design:
      view: article-grid
      columns: 2

  # ── All Publications ──────────────────────────────────────────────────────
  - block: collection
    content:
      title: ''
      text: ''
      filters:
        folders:
          - publications
        exclude_featured: true
    design:
      view: citation

  # ── News ──────────────────────────────────────────────────────────────────
  - block: collection
    id: news
    content:
      title: News
      subtitle: ''
      text: ''
      page_type: blog
      count: 5
      filters:
        author: ''
        category: ''
        tag: ''
        exclude_featured: false
        exclude_future: false
        exclude_past: false
      offset: 0
      order: desc
    design:
      view: card
      spacing:
        padding: [0, 0, 0, 0]
---
