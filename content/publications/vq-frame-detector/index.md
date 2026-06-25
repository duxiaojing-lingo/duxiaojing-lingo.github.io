---
title: 'Frame-Level Voice Quality Detection with WavLM Probes'
authors:
  - me
date: '2026-07-08T00:00:00Z'
doi: ''

publication_types: ['paper-conference']
publication: '*Interspeech SLT 2026*'
publication_short: 'SLT 2026'

abstract: >
  We present a frame-level voice quality detector that identifies creaky, breathy, and whispery
  phonation at 50 Hz in continuous speech. A single-layer MLP probe trained on frozen WavLM-large
  embeddings achieves AUC > 0.95 across all three phonation classes, using class-specific optimal
  layers (breathy L7, creaky L4, whispery L9). The V5 model is trained on a three-tier corpus of
  27 naturalistic interview speakers, RAVDESS acted speech, and controlled VQ recordings, with a
  calibrated positive weight (pos_weight=50) that makes sigmoid > 0.5 scientifically meaningful
  at real-world VQ rates (~2%).

featured: true

links:
  - name: Demo
    url: https://github.com/duxiaojing-lingo/vq_frame_detector

url_pdf: ''
url_code: 'https://github.com/duxiaojing-lingo/vq_frame_detector'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

image:
  caption: 'Frame-level phonation detection across speaker and phonation type'
  focal_point: ''
  preview_only: false

tags:
  - Voice Quality
  - Self-supervised Learning
  - Phonetics
  - WavLM
---
