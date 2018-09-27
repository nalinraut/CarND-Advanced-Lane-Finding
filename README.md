## Advanced Lane Finding Project



[image1]: ./assets/project_video.gif "Final Output"
[image2]: ./assets/calibration.png "Process of camera calibration."
[image3]: ./assets/undistorted.png "Original and Undistorted Images."
[image4]: ./assets/warped.png "Warped and Unwarped Images."
[image5]: ./assets/threshold.png "Combined gradient and color threshold."
[image6]: ./assets/
[image7]: ./assets/


![alt text][image1]
---
The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.
---

### Camera Calibration and Lens Distortion Correction

For camera calibration and lens distortion correction I created a class called Camera
 - `camera = Camera(directory = 'camera_cal/')`initializes the Camera object with the calibration image files
 - `camera.calibrate()` This method uses the loaded images and computes `mtx`- the camera matrix and `dist`- the distortion coefficients using  `cv2.calibrateCamera()`. This requires `objpoints` and `imgpoints` which are computed using `cv2.findChessboardCorners`. `mtx` and `dist` are made into a dictionary and saved as a pickle object `camera_calib.p`.
 - `camera.getCalibration()` method loads the calibration file.
 - `camera.undistort()` is used for correcting the lens distortion in an image.
 
 The images below shows the process of calibration with original and undistorted images.
 ![alt text][image2]
 ![alt text][image3]

### Perspective Transform

For obtaining a perspective transform of an image I created the class Transform
- The `getPoints()` method yields source and destination points for the given image.
- `getPerspectiveTransform()` and `getInversePerspectiveTransform()` yields warped and unwarped images respectively.

The code below outputs source and destination points.

```python
        offset = 75
        src_pts = np.float32([[w // 2 - offset, h* 0.625], 
                          [w // 2 + offset, h * 0.625], 
                          [offset, h], 
                          [w - offset, h]])

        dst_pts = np.float32([[offset, 0], 
                          [w - offset, 0], 
                          [offset, h], 
                          [w - offset, h]])
 ```
The figure below shows warped and unwarped images.
![alt text][image4]

### Combined gradient and color thresholds

I created the Threshold class for applying combined gradient and color thresholds.
The image is converted into HSL space to identify the yellow and white portions since lanes fall into the same color spectrum (Lanes are either white or yellow in color)
In addition to the HSL space, Sobel filter in 'x' direction is applied to image since the filter in 'x' direction detects edges close to horizontal. 
The figure below shows images with gradient and color thresholds in a combination. The green color in the stacked image represents the gradient threshold region and blue color represents the color threshold. 
![alt text][image5]


### Identifying lane line pixel points






---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
