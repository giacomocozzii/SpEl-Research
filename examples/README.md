# Example score material

The following input files were supplied for portfolio preparation. They are **not redistributed in this public repository** because reproduction rights for specific scanned editions have not been verified.

| Input filename | Work / instrument | Intended test case |
|---|---|---|
| `beethoven5-violin.pdf` | Beethoven, Symphony No. 5 / violin | Dense orchestral part |
| `brahms-clarinet.pdf` | Brahms, Clarinet Sonata Op. 120 No. 2 / clarinet | Large modern-style engraving |
| `brahms-vecchio.pdf` | Brahms, Clarinet Sonata Op. 120 No. 2 / clarinet | Historical scan and page margins |
| `grieg-viola.pdf` | Grieg, Piano Concerto Op. 16 / viola | Dense multi-system page |
| `mozart40-violin.pdf` | Mozart, Symphony No. 40 / violin | Historical scan with high notation density |
| `mozart-K467.Bassoon.pdf` | Mozart, Piano Concerto No. 21 / bassoon | Part layout with measure numbers |
| `mozart-K550-Oboe.pdf` | Mozart, Symphony No. 40 / oboe | Modern part layout |

These descriptions concern **input variety**, not measured detection performance.

## Adding genuine detection results

For each example selected for publication, export from the local SpEl-Demo:
1. Original page (or an authorized excerpt)
2. Automatically detected score crop
3. Barline overlay produced by the model
4. Detector version, confidence threshold, and page number

Do not draw detections manually and label them as model output. Do not report mAP, precision or recall without evaluation against annotated ground truth.
