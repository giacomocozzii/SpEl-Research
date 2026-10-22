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
