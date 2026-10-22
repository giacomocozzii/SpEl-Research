# Results and evaluation status

## Verified locally

| Component | Current evidence | Status |
|---|---|---|
| PDF/image input | Local Streamlit workflow exercised | Working prototype |
| Score-region detector | Local inference and cropping workflow | Working prototype |
| Barlines detector | Local inference on score crops | Working beta; refinement ongoing |
| Coordinates | Mapping from crop coordinates to source-page pixels | Implemented; visual verification advised |
| Output export | Annotated previews and coordinate CSV | Implemented locally |
| Automated checks | Short unit tests of application behavior and PDF rendering | Passed in development |

## Checkpoint-derived validation results

The two supplied `best.pt` checkpoints contain training histories and validation summaries. The values below are read directly from the saved checkpoint metadata, without executing model inference:

| Detector | Precision | Recall | mAP@50 | mAP@50:95 | Selected epoch | Logged epochs |
|---|---:|---:|---:|---:|---:|---:|
| Score-region | 0.99724 | 1.00000 | 0.99500 | 0.93178 | 20 | 35 |
| Barlines | 0.97401 | 0.96296 | 0.97202 | 0.59051 | 13 | 23 |

![Score-region training](assets/score-region-training.svg)

![Barline training](assets/barline-training.svg)

**Interpretation.** The results are encouraging for compact research prototypes trained on limited datasets. They do not demonstrate robustness across unseen scores. The difference between mAP@50 and mAP@50:95 is particularly relevant to barlines, where a small localization error can matter even when a separator is recognized.

## Not yet published or independently validated

- Independent held-out test-set precision, recall, F1 and mAP for both models
- Representative *actual* before/after inference screenshots
- Page-level and score-level counting accuracy
- Runtime benchmarks on a reproducible hardware configuration
- Robustness across different engraving styles, scan resolutions and historical editions

**The numerical scores above are internal validation metrics from training checkpoints**, not independent benchmarks. Automated application tests do not measure detection precision or recall.

## Example corpus

Development has used sample parts from Beethoven, Brahms, Grieg and Mozart, including a historical Brahms scan. Images in a future gallery will be labeled with the source work, page, detector version, and whether the overlay is an actual prediction or manual ground truth.

## Next steps

1. Review actual prediction overlays from the locally running SpEl-Demo.
2. Choose score excerpts whose reproduction rights are clear.
3. Export paired input/prediction images and a small, manually verified accuracy table.
4. Add only those outputs to this public portfolio repository, keeping model weights and training assets separate.


## Qualitative review of exported predictions (Brahms, Op. 120 No. 2)

**Source:** User-supplied screenshots of actual model predictions, covering four pages of the clean score, five score-region examples, and three pages each of score-region and barline predictions from a historical scan. These images are not manually drawn ground-truth annotations.

**Stage 1 — score-region detection.** The predicted bounding boxes enclose the main notation area on both clean pages and historical scans. On the historical scans, the detected region excludes much of the wide outer page border. On clean pages, the region is close to the full printed page. The score detector is therefore useful as an automated cropping step, although it is not a system-by-system segmentation model.

**Stage 2 — barline detection.** Many candidate boxes align with the expected vertical measure separators, and the model finds candidates throughout densely notated pages. The current output is not error-free: some boxes overlap notes or other vertical marks, some separators are missed, and neighboring candidates can overlap. The historical-scan examples are particularly useful for showing both the promise and the remaining difficulty of robust detection.

**Important distinctions:**
- The red rectangles in the screenshots are *predictions*, not verified correct barlines.
- Numbers above the boxes are model confidence scores, not measure indices or validation accuracy.
- The 97.2% mAP@50 figure is an internal validation result and must not be interpreted as the proportion of correct barlines on these example pages.
- The comparatively lower barline mAP@50:95 (59.1%) is consistent with the challenge of precise localization of very thin objects.
- No page-level precision/recall has been calculated from these screenshots because no independent manually verified ground truth was supplied.

The side-by-side comparison images have been prepared from the exported predictions and original score PDFs. They can be added to this repository after the binary images are uploaded and their source-edition rights have been checked.
