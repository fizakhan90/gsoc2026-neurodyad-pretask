# GSoC 2026 NeuroDyads Pre-Task
## Brain-to-Brain Decoder | ML4SCI

**Project:** Brain-to-Brain Decoder — Validating Neural Synchrony Patterns in Human Conversation

---

## Overview

This repository contains my completed pre-task submission for the GSoC 2026 
NeuroDyads project under ML4SCI. The task involved preprocessing two simultaneous 
EEG recordings from a conversational dyad, applying CEBRA for representation 
learning, and interpreting the resulting embeddings.

---

## What's in This Repository

- `ML4Sci_pretask(1).ipynb` — Complete Jupyter notebook containing all 
  code, figures, and written analysis

---

## Pipeline Summary

**Part 1 : Preprocessing (MNE-Python)**
- Loaded two raw EDF files (Listener and Speaker, 64-channel, 250Hz)
- Segmented into positive affect (DIN1 marker 1→2) and negative affect 
  (DIN1 marker 3→end) conditions
- Resolved a 0.217s duration mismatch between participants for CEBRA alignment
- Applied PICARD ICA with Common Average Reference for artifact removal
- Used ICLabel for component classification, verified through visual inspection
- Listener: 16/20 components rejected | Speaker: 18/20 components rejected

**Part 2 : CEBRA Embedding**
- Constructed T×128 joint matrix (Listener + Speaker channels)
- Applied per-segment z-normalization after discovering ~40x variance mismatch 
  between segments
- Trained CEBRA with 3D output embeddings, labeled by affect condition
- Ran shuffled-data control to test genuineness of learned structure
- Evaluated with KNN decoding accuracy and InfoNCE loss

**Part 3 & 4 : Interpretation and Reflection**
- Analyzed embedding geometry and control results
- Identified specific limitations of the analysis given this dataset

---

## Key Findings

- The embedding collapsed into two tight blobs rather than a continuous manifold, 
  suggesting CEBRA learned a trivial statistical separation rather than genuine 
  brain-to-brain coordination structure
- Per-segment normalization was necessary but insufficient  the fundamental 
  limitation was too few clean neural components in the Speaker (2/20 retained) 
  due to extensive speech-related muscle artifacts
- The shuffled control confirmed minimal genuine temporal structure was learned, 
  with near-identical InfoNCE losses (main: 5.6659 vs control: 5.6695)

---

## Dependencies
```
mne
mne-icalabel
python-picard
cebra
scikit-learn
numpy
matplotlib
```

Install with:
```bash
pip install mne mne-icalabel python-picard cebra scikit-learn numpy matplotlib
```

---

## Technical Environment

- Python 3.12
- Google Colab
- MNE-Python 1.11.0
- CEBRA 0.6.0

