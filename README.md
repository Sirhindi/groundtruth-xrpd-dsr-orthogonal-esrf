
# Ground-Truth Benchmark for Debye–Scherrer Ring Detection in XRPD Images

This repository provides a manually annotated ground-truth benchmark for the
detection of Debye–Scherrer rings in X-ray powder diffraction (XRPD) images.
It accompanies the paper *"Density-Based Detection of Debye–Scherrer Rings from
Orthogonal Detectors"* and is intended to support objective, reproducible
evaluation of automatic ring-detection algorithms.

The dataset consists of five XRPD images acquired from orthogonal detectors at
the European Synchrotron Radiation Facility (ESRF). For each image, every
visually definitive elliptical ring has been manually marked and fitted with an
ellipse, with the fitted parameters released as structured XML.

## Repository structure

```
.
├── Original images/            # Raw XRPD images (no annotation)
├── Images with groundtruth/    # Same images with marked rings overlaid
└── Groundtruth XML/            # Fitted ellipse parameters, one file per image
```

### Original images
The unannotated XRPD images in PNG format, provided as the input to detection
algorithms.

### Images with groundtruth
The same images in PNG format with the annotated rings drawn on top, provided
for visual reference and quick inspection of the ground truth.

### Groundtruth XML
One XML file per image, containing the parameters of the ellipse fitted to each
annotated ring. 

## Dataset

| Image      | Detector type | Dimensions   |
|------------|---------------|--------------|
| D0         | orthogonal    | 981 × 1043   |
| Tilted000  | orthogonal    | 1024 × 1024  |
| Max1       | orthogonal    | 1024 × 1024  |
| Si12       | orthogonal    | 2048 × 2048  |
| LaB6       | orthogonal    | 1024 × 2048  |

## Annotation protocol

Each ring is defined as a visually definitive elliptical region. Noisy outer
sections where rings appear too broken to identify reliably are excluded from
the ground truth. For every annotated ring, at least five points were marked
along the ring, with at least one point in each quadrant, and an ellipse was fit
to the marked points. Both the marked points and the resulting ellipse
parameters are stored.

## XML format

Each XML file describes the rings annotated in one image. The root element is
`<struct>`, which contains one `<Ell>` element per annotated ring. Each `<Ell>`
element contains:

- Five `<param>` elements — the five parameters of the ellipse fitted to the
  ring, in the following order:
  1. center x-coordinate (pixels)
  2. center y-coordinate (pixels)
  3. semi-major axis length (pixels)
  4. semi-minor axis length (pixels)
  5. rotation angle of the major axis (radians; may be negative)
- `<GT_x>` and `<GT_y>` elements — the coordinates of the manually marked
  ground-truth points along the ring, one `<GT_x>` and one `<GT_y>` tag per
  point. The number of marked points varies from ring to ring (at least five,
  with at least one point per quadrant).

A representative ring entry:

```xml
<struct>
  <Ell>
    <param> ... </param>   <!-- center x (px) -->
    <param> ... </param>   <!-- center y (px) -->
    <param> ... </param>   <!-- semi-major axis (px) -->
    <param> ... </param>   <!-- semi-minor axis (px) -->
    <param> ... </param>   <!-- rotation angle (radians) -->
    <GT_x> ... </GT_x>     <!-- one GT_x / GT_y pair per marked point -->
    <!-- ... further points ... -->
    <GT_y> ... </GT_y>
    <!-- ... further points ... -->
  </Ell>
  <!-- one <Ell> per ring -->
</struct>
```

## Usage

These files allow detected ellipses to be compared against the ground truth.
In the accompanying paper, a detection is matched to a ground-truth ring when
their intersection over union (IoU), exceeds a threshold. Matched detections 
are counted as true positives and used to compute precision, recall, and F1-score.

## Citation


## License


## Contact

Rabia Sirhindi and Nazar Khan, Department of Computer Science, University of the
Punjab, Lahore, Pakistan.
