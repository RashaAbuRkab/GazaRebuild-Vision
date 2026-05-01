# GazaRebuild Vision — AI-Powered Structural Damage Assessment
## AIO 2026 | Computer Vision Track | Project Summary

---

### Problem Statement

Gaza has witnessed the largest scale of urban destruction in modern history. Over 60% of buildings in Gaza Strip have been damaged or destroyed since October 2023, according to UN satellite assessments. When reconstruction begins, **the single most urgent bottleneck is structural safety assessment** — determining which buildings can be safely re-entered, repaired, or must be demolished.

Currently, this process requires **licensed structural engineers** to physically inspect each building — one by one. Given that:
- 160,000+ buildings are damaged
- There is a severe shortage of engineers in Gaza
- Many areas remain dangerous to enter
- International teams face access restrictions

...manual assessment could take **decades** at current rates, leaving hundreds of thousands of families in limbo.

**Our system solves this exact bottleneck.**

---

### Proposed Solution

**GazaRebuild Vision** is a working web application that accepts a photograph of a damaged building and outputs an immediate, standardized structural safety classification — replicating the judgment of a trained engineer for first-pass screening.

```
[Photo of Building] → [CV Damage Detection] → [Rule-Based Assessment] → [Safety Report]
     (any camera)         (YOLOv8-seg)           (ATC-20 protocol)         (3 levels)
```

The system performs **two tasks in sequence**:

1. **Visual damage detection** — A fine-tuned YOLOv8 segmentation model identifies and quantifies structural damage indicators: cracks, spalling, column deformation, partial collapse, and roof failure.

2. **Structured safety assessment** — A rule engine applies the internationally recognized ATC-20 post-disaster building evaluation protocol to convert visual measurements into one of three actionable decisions:
   - 🟢 **INSPECTED (Safe)** — Building may be occupied
   - 🟡 **RESTRICTED USE** — Limited access, professional assessment required
   - 🔴 **UNSAFE** — Do not enter under any circumstances

---

### Why This Specific Problem?

Most AI-for-Gaza projects focus on mapping and counting — understanding the *scale* of destruction from satellite imagery. That problem is largely solved by organizations like UNOSAT and Cohere for AI.

The **unsolved problem** is the next step: **ground-level safety classification** that enables actual reconstruction decisions. This is the gap between knowing a building is damaged and knowing what to do about it.

Our application targets:
- **Aid workers and NGO field teams** conducting rapid assessments
- **Local community leaders** making immediate housing decisions
- **Future reconstruction engineers** as a pre-screening and prioritization tool

---

### Technical Architecture

#### Component 1: Damage Detection Model
- **Base model**: YOLOv8-seg (segmentation variant for pixel-level damage mapping)
- **Training data**: 
  - PEER PHI-Net database (UC Berkeley — labeled post-earthquake damage photos)
  - QUAKEML-GEN and RescueNet datasets (Kaggle)
  - Augmented with Gaza-specific imagery from UNITAR/UNOSAT open archives
- **Output**: Bounding boxes + segmentation masks for 6 damage classes with confidence scores and damage area percentages

#### Component 2: ATC-20 Assessment Engine
A deterministic rule system implementing the American Red Cross / California OES post-disaster evaluation standard:

| Condition Detected | Assessment |
|---|---|
| Column damage ≥ 60% OR partial collapse visible | UNSAFE |
| Structural wall crack width > threshold AND coverage > 30% | RESTRICTED |
| Roof collapse OR foundation failure visible | UNSAFE |
| Non-structural damage only, coverage < 20% | INSPECTED |
| Multiple moderate indicators | RESTRICTED |

#### Component 3: Web Application
- **Frontend**: React.js — drag-and-drop image upload, real-time visual annotation overlay, PDF report generation
- **Backend**: FastAPI (Python) — model inference endpoint, report generation
- **Deployment**: Hugging Face Spaces (free, no infrastructure cost)

---

### Dataset Strategy

| Source | Type | Size | Access |
|---|---|---|---|
| PEER PHI-Net | Labeled structural damage photos | ~4,000 images | Public (UC Berkeley) |
| RescueNet (Kaggle) | Post-disaster aerial + ground imagery | ~5,000 images | Public |
| UNOSAT Gaza Archive | Gaza-specific building imagery | ~800 images | Public |
| Data augmentation | Synthetic variations | +3,000 images | Generated |

Total training set: ~12,000+ labeled images covering earthquake, explosion, and conflict damage patterns — all structurally relevant to Gaza's context.

---

### Project Timeline (12 Days)

| Days | Milestone |
|---|---|
| 1–2 | Dataset collection, cleaning, labeling in Roboflow |
| 3–5 | YOLOv8 fine-tuning on Google Colab (T4 GPU) |
| 6–7 | ATC-20 rule engine implementation + integration |
| 8–9 | React frontend + FastAPI backend |
| 9–10 | Integration testing, demo video |
| 11–12 | Documentation, Hugging Face deployment, submission |

---

### Expected Results & Metrics

- **Model mAP@50**: ≥ 0.72 on held-out test set
- **Classification accuracy**: ≥ 85% agreement with expert engineer labels on benchmark cases
- **Inference time**: < 3 seconds per image on CPU
- **Working demo**: Live application accessible via URL at submission

---

### Innovation & Impact

**What makes this different from existing tools:**

1. **Gaza-specific** — Most structural AI tools are trained on earthquake damage (Japan, Turkey). We augment with conflict-damage imagery specific to Gaza's building typology (reinforced concrete frame, 3-8 floors).

2. **Interpretable output** — Unlike a black-box classifier, our system explains *why* it gave a rating: "Column deformation detected (67% confidence) in load-bearing zone — UNSAFE under ATC-20 section 3.2." This builds trust with non-technical users.

3. **Offline-capable design** — The model can be exported to ONNX format and run on a mobile device without internet — critical for Gaza's connectivity limitations.

4. **Standards-aligned** — Using ATC-20 means our outputs are recognized and actionable by international reconstruction organizations.

---

### Team
AIO 2026 Participant | Palestine | Computer Vision Track

*This project is submitted to AI Olympiad 2026 — CV Track*
