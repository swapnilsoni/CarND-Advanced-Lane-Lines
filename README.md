**Advanced Lane Finding Project**

---

The goals / steps of this project are the following:

* Finding Corners
* Camera Calibrate
* Distortion Correction
* Combine Thresholds(Sobel): color_sobel,sobel_magn,sobel_direction
* Warp Transform, Reverse warp
* Detect lane pixels and fit to find the lane boundary
* Determine the curvature of the lane and vehicle position with respect to center.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position

[//]: # (Image References)
[image1]: ./output_images/calibration20.jpg "No Corners"
[image7]: ./output_images/corner_test20.png "Corners"
[image8]: ./output_images/undest_chess.png "Undistorted1"
[image9]: ./output_images/undest_chess2.png "Undistorted2"
[image10]: ./output_images/undest_chess3.png "Undistorted3"
[image2]: ./output_images/undist.png "Undistorted"
[image3]: ./output_images/ColorConversion.png "Color HSL"
[image11]: ./output_images/binary_y_w_line.png "Binary Transformation"
[image12]: ./output_images/binary_y_w_shadow_line.png "Binary with shadow Transformation"
[image13]: ./output_images/mgn_sobel.png "Sobel Transformation"
[image4]: ./output_images/lane_bird_view.png "Warp Transformation"
[image14]: ./output_images/warp1.png "Warp Transformation"
[image15]: ./output_images/warp2.png "Warp Transformation"
[image5]: ./output_images/lane_on_unwarpimage.png "window on a warped image"
[image6]: ./output_images/lane_on_originimage.png "lane line on uwarmp image"
[image16]: ./output_images/lane_on_originimage1.png "lane line on uwarmp image"

[image16]: ./output_images/withcurvature.png "With curvature text"
[video1]: ./project_video.mp4 "Video"


### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### 1. Finding Corners


There are two coordinates are necessary "object points" and "image points". First, I have used the cv2 library (i.e function name findChessboardCorners) to find out the corners(image points). Finally, I then calculated the object points" which will be the (x, y, z) coordinates of the chessboard corners. Here, z coordinate will be zero, such that the object points are the same for each calibration image.

To calculate all the objects and images points of an image I read all the images given by Udacity folder "/camera_cal/calibration*.jpg" and created a list of images and objects points.

Here is one of the examples:

![No Corner image 1][image1]
![Corner image 1][image7]

### 2. Camera Calibration

Once we have the image and object points of the image we need calibrate the image using those points. A Calibration is an algorithm to project 3D coordinates to 2D coordinates of the image. I have passed the images and objects points to the cv2.calibrateCamera which returns the two metrics are calibration and distortion coefficient.


### 3. Distortion correction of the image.

To demonstrate this step, I have applied cv2.undistort function and passed the calibration and distortion coefficient metrics to the function which is obtained from the last step (Calibration). Here, I applied this distortion correction to the test image using the cv2.undistort() function and obtained this result:



![alt text][image8]
![alt text][image9]
![alt text][image10]

This is an undistored car street image from test forlder.

![alt text][image2]


### 4. Sobel (Use color transforms, gradients, etc., to create a thresholded binary image.)
#### 1. Convert a color(RGB) image to HSL, here is an example:
![alt text][image3]

#### 2. Binary images and filtiring only white and yellow color
![alt text][image11]
Binary images and filtiring only white and yellow color with shadow image
![alt text][image12]

#### 3. I have tried combination of color transforms, gradients, and color threshold to changes the image is called sobel transformation.
![alt text][image13]

### Warp Transformation
#### 4. Finally converted the binary image to birds-eye image. This is also called the perspective transformation using the source and destination coordinates. I have used cv2.getPerspectiveTransform and warpPerspective functions to achieve this goal.

The getPerspectiveTransform function requires two input first source and second destination coordinates and returns a metrics which helps to warp the image. If you switch the params then it gives you the original image. Once you get the metrics use it as an input to the warpPerspective function to look image as a birds-eye view.
Source
```
 src = np.float32([
            [580, 460],
            [700, 460],
            [1040, 680],
            [260, 680],])
```
Distination
```
dst = np.float32([
            [260, 0],
            [1040, 0],
            [1040, 720],
            [260, 720],
        ])
```

Here is many examples:

![alt text][image4]
![alt text][image14]
![alt text][image15]

### Lane Finding
#### 5. This code is inspiered from the Udacity code which is available in video tutorial.

##### How it works:
* Use the warped binary image to find out the peak of the left and right halves of the histogram.
* Use the window technique to find out the left and right indices(x and y coordinates) of the lanes. Fit the line once you find out all the x-y coordinates on both lanes.
* The above steps are used only once, the first image of a video, in the pipeline
* Using np.linspace to generate y-coordinates intervals of the binary-warped-image.
* Calculate the X coordinated of all y-coordinates intervals using the second-order polynomial.
* Then use the predicted X and Y coordinates draw the lane on the image.

![alt text][image5]
![alt text][image6]
![alt text][image16]
![alt text][image17]
---

### Pipeline (video)

#### 1. Final video output
Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.
* There are cases when a lane is not visible properly in the video in that case an empty axis returns in the find lane code.
* As I am extracting white and yellow colors from the image assuming that it is only lane on the road. I feel the algorithm fail if there is snow or water on the lane. Although, I have not tested.
