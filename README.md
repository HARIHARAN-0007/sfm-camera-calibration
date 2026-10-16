# SfM Camera Calibration

Camera calibration module for a **Structure from Motion (SfM)** project using OpenCV and chessboard images. This repository contains code to estimate camera intrinsic parameters and lens distortion coefficients before reconstructing a 3D scene from multiple images.

## Overview

Camera calibration is an important preprocessing step in an SfM pipeline. It helps characterize the camera's imaging geometry and lens distortion, providing camera parameters that can support accurate feature-based 3D reconstruction.

This project uses a chessboard calibration pattern to estimate camera parameters from multiple calibration images.

## Objectives

- Estimate the camera intrinsic matrix.
- Calculate lens distortion coefficients.
- Detect chessboard corners in calibration images.
- Evaluate calibration quality using reprojection error.
- Generate camera parameters for use in the SfM reconstruction pipeline.

## Technologies Used

- Python
- OpenCV
- NumPy
- Google Colab (if used for cloud-based execution)

## Project Structure

```text
sfm-camera-calibration/
├── calibration.py          # Camera calibration implementation
├── calibration_images/     # Chessboard calibration images
├── requirements.txt        # Python dependencies
├── .gitignore              # Files excluded from Git
└── README.md               # Project documentation
```

*Note: Adjust the filenames and folder structure to match the files actually included in your repository.*

## Calibration Methodology

### 1. Collect calibration images

Capture multiple images of a chessboard pattern from different angles and positions. Use images with clear, visible corners and varied orientations to improve calibration quality.

### 2. Detect chessboard corners

OpenCV detects the internal corners of the chessboard pattern. The calibration configuration used in this project is:

```python
CHECKERBOARD = (6, 9)
```

This represents **6 internal corners along one dimension and 9 along the other**, not the number of squares. Ensure these dimensions match the actual chessboard pattern used for calibration.

### 3. Estimate camera parameters

Using the detected corners and their corresponding 3D chessboard coordinates, OpenCV estimates:

- **Camera matrix (K):** Contains focal-length parameters and the principal point.
- **Distortion coefficients:** Model lens distortion, including radial and tangential distortion.
- **Rotation vectors:** Describe the chessboard's orientation relative to the camera for each calibration view.
- **Translation vectors:** Describe the chessboard's position relative to the camera for each calibration view.

### 4. Evaluate calibration quality

The calibration process produces a reprojection error that measures the discrepancy between observed image points and their projected positions under the estimated camera model.

A lower reprojection error generally indicates a better fit, but calibration quality should also be evaluated using image coverage, corner-detection quality, and consistency across views.

## Installation

Install the required Python libraries:

```bash
pip install opencv-python numpy
```

If the code uses additional libraries, include them in `requirements.txt`.

## Usage

1. Clone this repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/sfm-camera-calibration.git
   cd sfm-camera-calibration
   ```

2. Place the chessboard calibration images in the directory expected by the script.
3. Update the image path and checkerboard configuration if necessary.
4. Run the calibration script:

   ```bash
   python calibration.py
   ```

5. Review the estimated camera matrix, distortion coefficients, and reprojection error.

Replace `calibration.py` with your actual Python filename if it differs.

## Integration with Structure from Motion

The estimated camera intrinsics and distortion parameters can be used as inputs to the broader SfM pipeline, depending on the reconstruction software and its camera model.

A typical SfM workflow consists of:

1. Camera calibration and distortion analysis.
2. Feature detection and description.
3. Feature matching between scene images.
4. Geometric verification and relative pose estimation.
5. Camera pose estimation and sparse 3D reconstruction.
6. Multi-view stereo and dense reconstruction, if required.

**Important:** The chessboard rotation and translation vectors describe the calibration pattern's pose in individual calibration images. They are not the camera poses for the building images used in the SfM reconstruction.

## Outputs

Depending on the implementation, the calibration script may produce:

- Camera intrinsic matrix.
- Lens distortion coefficients.
- Calibration RMS error or reprojection error.
- Detected chessboard corners and visualizations.
- Saved calibration parameters for downstream processing.

## Notes

- Use calibration images captured with the same camera and image resolution as the scene images whenever possible.
- Use a range of chessboard orientations and positions.
- Avoid blurry images and poorly detected corners.
- Keep calibration images separate from the building or scene images used for 3D reconstruction.
- Verify that the downstream SfM software uses compatible camera parameters and distortion conventions.

## Applications

This module is intended for academic and experimental 3D reconstruction workflows involving photogrammetry, computer vision, and Structure from Motion.

## License

Add a license file if you intend to distribute this code for reuse. If this is an academic project, include the appropriate course or institutional attribution where required.
