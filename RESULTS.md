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

## Not yet published or independently validated

- Held-out precision, recall, F1 and mAP for the barline model
- Representative *actual* before/after inference screenshots
- Page-level and score-level counting accuracy
- Runtime benchmarks on a reproducible hardware configuration
- Robustness across different engraving styles, scan resolutions and historical editions

**No numeric detection-performance claim is made in this repository.** In particular, automated application tests do not measure model precision or recall.

## Example corpus

Development has used sample parts from Beethoven, Brahms, Grieg and Mozart, including a historical Brahms scan. Images in a future gallery will be labeled with the source work, page, detector version, and whether the overlay is an actual prediction or manual ground truth.

## Next steps

1. Review actual prediction overlays from the locally running SpEl-Demo.
2. Choose score excerpts whose reproduction rights are clear.
3. Export paired input/prediction images and a small, manually verified accuracy table.
4. Add only those outputs to this public portfolio repository, keeping model weights and training assets separate.
