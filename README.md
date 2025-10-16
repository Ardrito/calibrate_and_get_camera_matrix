# Camera calibration tool
## Welcome
This tool provides a script and GUI to capture images and extract several camera matrix parameters for a stereo camera setup (or just an individual camera if preferred).

## How to use
Run either GUI.py or Capture_Calibrate.py  
Use a chessboard pattern ( number of squares horizontally != number of squares vertically ) move the chessboard around the scene slowly, at a distance representative of the planned use case.  
Images will be captured at "snap_time" interval if a valid chessboard is visible to the camera.  
Recommended images: >40  
Minimum recommended grid size: 11x10 (Larger likely to produce better results, depends on intended application)  
R - The rotation between cameras must be provided in Capture_Calibrate if R != 0  
T - Can be provided in the GUI if needed, or through Capture_Calibrate.  

Reprojection errors (error_one & error_two) should be <1, ideally <0.5 

Sample to use the output .npz file:
```
calib = np.load("camera_calibration_data.npz")
mtx_r, dist_r = calib["camera_matrix_one"], calib["dist_one"]
mtx_l, dist_l = calib["camera_matrix_two"], calib["dist_two"]
R, T = calib["R"], calib["T"]
```

This provides the original camera matrix and new camera matrix from opencv-python, in addition mapx and mapy.  
This allows the use of either cv2.undistort or cv2.remap to undistort and image as below:

```
undistorted_image = cv.undistort(image, camera_matrix_one, dist_one, None, newMtx_one)
```

or 

```
undistorted_image = cv2.remap(image, mapx_one, mapy_one, cv2.INTER_LINEAR)
```

## To find camera IDs
On Linux run:
```
v4l2-ctl --list-devices
```

## Requirements
- opencv-python
- numpy

## Returns
This tool creates a .npz file "camera_calibration_data.npz" containing:

- camera_matrix_one
- dist_one
- newMtx_one
- roi_one
- mapx_one
- mapy_one
- error_one
- camera_matrix_two
- dist_two
- newMtx_two
- roi_two
- mapx_two
- mapy_two
- error_two
- R
- T


