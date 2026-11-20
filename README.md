# Camera calibration for an industrial inspection station

[Open the interactive HTML review](https://raw.githack.com/denoskume/camera-calibration/main/review/index.html) · [Download the offline viewer](https://github.com/denoskume/camera-calibration/raw/refs/heads/main/review/index.html) · [Read the executed notebook](Camera_Calibration.ipynb)

## Mission

Before a fixed camera can locate features on a planar inspection fixture, an engineer needs to know how board coordinates map to image pixels. This Centrale Nantes MSc case study uses Zhang’s method to estimate the camera intrinsics and the pose of a chessboard in nine laboratory photographs. The inspection station provides the industrial context.

## Results

| Measure | Result |
|---|---:|
| Chessboard views retained | 9 of 9 |
| Detected corners | 432 |
| Mean reprojection error | 0.434 px |
| Overall RMS reprojection error | 0.534 px |
| Highest view-level RMS error | 0.859 px on img7.jpg |

![Mean corner reprojection error in pixels for each chessboard view](outputs/figures/mean_reprojection_error_by_view.png)

The board has 8 × 6 inner corners and a square pitch of 30 mm. The notebook refines their image locations, fits a normalized DLT homography per view, solves Zhang’s constraints for the common intrinsic matrix, and recovers nine board-to-camera poses. The review lets you toggle corner overlays on every source image. The exported files contain the [camera matrix and poses](outputs/calibration.json) and [per-view measurements](outputs/view_errors.csv).

## Experience gained

- Used point normalization and singular value decomposition to estimate homographies from coordinates expressed in metres and pixels.
- Checked recovered rotations for orthogonality and unit determinant before using them to project the board.
- Used per-view residuals to identify the pose that warrants the closest inspection. The fitted pinhole model omits distortion. Qualifying it for dimensional inspection would require independent reference measurements and a part tolerance.

## Reproduce

Download the repository ZIP and extract it. From its root, install the dependencies in a Python 3.12 environment with `python -m pip install -r requirements.txt`. Open `Camera_Calibration.ipynb` in VS Code, select that environment, and run all cells. The notebook regenerates six diagnostic figures, the calibration parameters, the error table and the standalone HTML viewer.

[Original MSc lab and historical materials](https://github.com/denoskume/msc-coro-dassip-portfolio/tree/0698d843d50896879ec3aec2ebae7d5a170b76c0/computer-vision/camera-calibration)
