# EduceLab CT Pixel Calculator

A standalone web tool for estimating Cone Beam CT (CBCT) geometry, voxel size, and sampling strategies. Designed to help researchers optimize scan settings based on object size and specific machine constraints.

## Features

### 1. Geometry Optimization
* **System Presets:** Pre-configured constraints for EduceLab systems: **Waygate V|tome|x M300** and **Bruker Skyscan 1273**.
* **Auto-Optimize Mode:** Automatically calculates the optimal Source-Object Distance (SOD) and Source-Detector Distance (SDD) to maximize resolution while keeping the object within the detector's field of view.
* **Manual Mode:** Allows fine-tuning of distances. When switching from Auto to Manual, the calculator pre-fills the inputs with the optimal values as a starting point.
* **Double-Wide (Offset) Support:** Calculates geometry for offset scans (virtual detector extension) if the system supports it.

### 2. Real-Time Visualization
* Interactive 2D canvas visualization showing the X-ray source, object position, detector active area, and safety margins.
* Visual feedback for object collisions or field-of-view violations.

### 3. Sampling Strategy Assistant
Calculates the required number of projections based on the **Crowther Criterion** (Nyquist sampling) to prevent aliasing artifacts. It provides four tiers of recommendations:
* **Nyquist Limit (1x):** The theoretical limit for perfect reconstruction.
* **High Quality (2x):** Recommended for standard, high-detail scans (undersampled by factor of 2).
* **Fast Scan (3x):** For high-throughput or quick preview scans.
* **Custom Step:** Allows users to input a specific rotational step size (e.g., 0.25°) to see the resulting Effective Resolution.

### 4. Custom Configuration
* **Manual Intrinsics:** Modify pixel pitch, detector width, and travel limits directly in the UI.
* **JSON Import:** Load custom machine configurations via a JSON file.

## Quick Start

1.  Download `index.html`.
2.  Open the file in any modern web browser (Chrome, Firefox, Edge, Safari).
3.  Select your **CT System** from the dropdown.
4.  Enter your **Object Diameter** (mm).
5.  View the **Projected Pixel Size** and **Sampling Recommendations** immediately.

## Custom System Configuration

You can load your own CT system parameters by clicking **"Load Config JSON"**. The file should follow this structure:

```json
{
  "name": "My Custom CT",
  "pixelPitch": 0.2,
  "widthPx": 2048,
  "minSOD": 10,
  "maxSOD": 900,
  "minSDD": 1000,
  "maxSDD": 1000,
  "maxObjectDiameter": 400,
  "allowDoubleWide": true,
  "overlapPx": 150
}
```

| Field | Description |
| :--- | :--- |
| `pixelPitch` | Detector pixel size in mm. |
| `widthPx` | Physical width of the detector in pixels. |
| `minSOD` / `maxSOD` | Mechanical travel limits for the stage. |
| `minSDD` / `maxSDD` | Mechanical travel limits for the detector. |
| `allowDoubleWide` | `true` if the system supports offset scans. |
| `overlapPx` | Number of pixels of overlap required for offset scans. |

## Sampling Logic

The calculator estimates the **Effective Resolution** based on the rotational step size.
* **Formula:** Arc Length ≈ Radius × Step(rad).
* If the Arc Length is larger than the projected pixel size, the effective resolution is limited by the sampling rate rather than the magnification.

## License

MIT License. Free to use for academic and industrial scan planning.