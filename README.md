## Writeup Template

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Find Corners
* Camera Calibrate
* Distortion Correction
* Combine Thresholds(Sobel): color_sobel,sobel_magn,sobel_direction
* Warp Transform
* Lane finding
[//]: # (Image References)

[image1]: ./output_images/corner.jpg "Corners"
[image2]: ./output_images/undist.png "Undistorted"
[image3]: ./output_images/ColorConversion.png "Color HSV"
[image4]: ./output_images/lane_bird_view.png "Warp Transformation"
[image5]: ./output_images/lane.png "lane line on uwarmp image"
[image6]: ./output_images/Lane_line_fit.png "lane line on uwarmp image"
[video1]: ./project_video.mp4 "Video"


### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Finding Corners
#### 1. I have used cv2 library (i.e functin name findChessboardCorners) to find out the corners.

![Corner image 1][image1]


### Camera Calibration

#### 2. I have read all the images from the given folder "/camera_cal/calibration*.jpg" and created a list of image and object points. Finally, I have passed the image and object points to the cv2.calibrateCamera to  generate metrics for calibration and distortion coefficient.

### Pipeline (single images)

#### 3. Distortion corrected image.

To demonstrate this step, I have applied cv2.undistort function and passed the  calibration and distortion coefficient metrics to this function which is obtained from the last step. Here, how I apply the distortion correction to one of the test images like this one:

![alt text][image2]

### Sobel

#### 4. I have used combination of color transforms, gradients, and color threshold to changes the image is called sobel transformation. I have used HSV color transformation.

![alt text][image3]

### Warp Transformation
#### 5. Finally converted the binary image to bird-view image. Here is an example of a transformed image.

![alt text][image4]

### Lane Finding
#### 5. I have used the same code as Udacity provided in video tutorial.

![alt text][image5]
![alt text][image6]

---

### Pipeline (video)

#### 1. Final video output
Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.
* There are cases when a lane is not visible properly in the video in that case an empty axis returns in find lane code.
