# SpEl — Intelligent Music Score Processing

**Computer vision for extracting musical content and identifying measure boundaries in scanned scores.**

SpEl (*Spartito Elettronico*, or *Electronic Music Score*) is an ongoing independent research and software-development project exploring how computer vision can turn scanned musical scores into structured, navigable documents. Its current focus is on two tasks: identifying the relevant score region on each PDF page and locating barlines that delimit musical measures.

> **Project status (October 2026):** Research prototype. Score-region detection has been trained and evaluated on a small validation set; barline detection and end-to-end integration remain under development. This repository presents the research and its progress, not the private implementation or datasets.

## Motivation

Scanned scores are primarily images. Even when the musical notation is legible to a performer, software cannot directly infer where the music begins and ends, where individual measures lie, or how to navigate between them. Reliable geometric understanding is a useful first step toward searchable and interactive digital sheet music.

SpEl investigates an incremental pipeline that combines learned object detection, document geometry and human-in-the-loop annotation.

## Research pipeline

```text
Scanned PDF / score images
          |
          v
Page rendering
          |
          v
Score-region detection (YOLO)
          |
          v
Cropping and coordinate tracking
          |
          v
Barline detection (OpenCV baseline / YOLO)
          |
          v
Candidate measure boundaries + coordinates
          |
          v
Visual review and manual correction
```

The two detection problems are handled separately: a model first localizes the music-bearing region of a page, and a second stage analyzes the cropped content for thin vertical measure separators. Coordinates can be mapped back to the original page.

## What has been achieved

### 1. Score-region detection

- Implemented PDF-to-image preprocessing and a manual annotation workflow.
- Trained a one-class YOLO detector to localize the principal score area on scanned pages.
- Built scripts for inference, cropped-page previews and coordinate export.
- Explored geometric refinement of the detected vertical bounds. In practice, retaining titles and avoiding overly tight upper crops requires care.

**Initial validation results**

| Metric | Reported value |
| --- | ---: |
| mAP@50 | ~0.995 |
| mAP@50–95 | ~0.78 |
| Initial labeled dataset | 51 pages from 5 scores |

These are **preliminary validation results**, not a claim of performance on unseen publishers, score layouts or a separate held-out test set. The dataset is small, and model selection and evaluation protocols require further consolidation.

### 2. Barlines and measure boundaries

Two complementary approaches have been investigated:

- **Classical computer vision:** staff-line analysis and OpenCV morphological operations to identify candidate vertical separators. Early experiments showed false positives from other vertical notation elements.
- **Learned detection:** a separate YOLO-based barline detector trained on cropped score regions, supported by manual annotation and correction tools.

The current software can generate candidate detections, visualization previews and coordinate outputs. **A reliable held-out benchmark for barline precision, recall and localization error has not yet been established.** Consequently, no barline accuracy figure is reported here.

### 3. Annotation and dataset workflows

The project includes tools for rendering pages, labeling score regions, preparing cropped pages for barline annotation, correcting predicted barlines, and generating training/validation datasets. Randomized splitting at the score level has been explored to reduce leakage between pages of the same source score.

### 4. Exploratory director-score data

A separate experiment uses exported score-related data to build a barline training dataset. As of this summary, the associated model checkpoint and inference integration have not been verified; this branch should be considered experimental rather than part of the validated pipeline.

## Technical challenges

- **Very thin targets:** barlines can occupy only a few pixels, making resolution and annotation accuracy important.
- **Look-alike symbols:** stems, brackets and other vertical notation can resemble barlines.
- **Variable page layouts:** margins, titles and page furniture complicate robust cropping.
- **Coordinate consistency:** predictions on cropped or resized images must map correctly to original PDF pages.
- **Data scarcity:** a small number of annotated scores limits confidence in generalization.
- **Reproducibility:** training configurations, dataset splits and checkpoint choices need to be standardized.

## Current limitations

The present work is a prototype, not a finished optical music recognition system. It does **not** claim to transcribe notes, reconstruct complete musical semantics, or reliably number every measure in arbitrary orchestral scores. The detector stages are not yet exposed through a single polished end-to-end application.

## Roadmap

1. Consolidate a reproducible inference pipeline with explicit model configuration and safe per-run outputs.
2. Establish a held-out evaluation set for barline detection, including precision, recall, F1 and localization error.
3. Improve handling of multiple systems, unusual layouts and visually ambiguous vertical symbols.
4. Add automated tests for coordinate transformations and dataset validation.
5. Publish illustrative before/after examples using scores with appropriate redistribution rights.
6. Build a lightweight interactive demonstration for uploading a PDF and inspecting detections.

## Repository scope and availability

This repository is intended as a **research overview and project portfolio**. The development code, trained model weights, original score scans and annotation datasets are maintained separately and are **not publicly distributed here**. This also avoids inadvertently publishing third-party copyrighted sheet music.

Illustrative images, benchmark tables and a demo may be added as they become ready and their publication rights are verified.

## Technologies

Python · Ultralytics YOLO · OpenCV · PDF/image preprocessing · custom annotation and evaluation utilities

## Contact

For technical discussion, collaboration or access inquiries, please open an issue in this repository once issues are enabled.

---

*Last updated: October 2026. All performance numbers above refer to the initial score-region detection validation experiment, not to the barline detector.*
