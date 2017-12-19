## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I performed the camera caliberations, by first find the corners in the set of given chessboard images, we used the cv2.findChessboardCorners() to find them, and I appended the corner points to the image_point() and the object_point() list.

and subsequently calculated the necessary caliberation matrices for the performing the undistortion on the images, using the function cv2.caliberateCamera() function

I also created a wrapper function to perform undistortion of the image


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I wrote down all the thresholding functions that were described in the lectures 
1) Absolute Threshold
2) Magnitude Threshold
3) Directional Threshold
4) Color Threshold ( L & S Channels from HLS color space)

After playing out with different possible hyperparameters, for hours I was able to come out with an optimal set of parametes, I also used masking to single out my region of interest for this task

Refer to the "Final Tidy Version Advanced Lane Lines.ipynb" for visualizations of each of the thresholding, separately as well as of the final pipeline

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

I used the cv2.getPerspectiveTransform() function to perform the perspective transform on the image, 
first I manually determined the source and the destination coordinates, on which the transform was to be performed, and finally I was able to get the perspective transforms, you can refer to the notebook for viewing the effect on each image, of the test_image folder

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Finally, I used the pipeline that was taught in the videos

The basis of the approach is the sliding window approach, in which we slide a window of some width and height all across the image, from the bottom towards the top, and we try to determine the non zero pixels in the thresholded as well as warped image, by doing this we are able to recognize the positions of all the relevant pixel, and in the window, 

We apply the histogram approach to determine the x position where the density of the pixels is highest, it is probably the position where the actual lane line lies, 

Using this approach we make a list and keep on appending the positions that are currently visible in the image, and finally use the cv2.polyfit() function to fit a second degree curve on the detected pixels.


#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

To calculate the radius of curvature, I used the formulae of the radius of curvature, which was described in the course content, it was possible to directly apply it to the image, becauase we had a curve already fitted on the lines, and by performing simple math and using the coefficients of the variables of the curve, by only directly plugging them to the formulae yielded the radius of curvature


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Please refer to the "Final Tidy Version Advanced Lane Lines.ipynb" notebook, it has the visualizations of all the test images, with the lane shaded and the curves on the lane lines already fitted, as well as the radius of curvature on as text on the image

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

project_video_output.mp4 has been uploaded already on the repo, I created a wrapper which performs all the steps in sequence and hence is able to determine the lane lines, and using VideoFileClip library and fl_image() functions I was able to fit the pipeline in the images

The model did reasonably well on the Project and the Challege video, but failed miserably on the Harder Challene video

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

This was by far the toughest project that I worked on, in this nanodegree, it consumed well above 15 hours, and there were a lot of different hyperparameters in the pipeline that can be tuned, 

Although there are many improvements that I can make, in the model (which I will, once I get some more free time) but as of now, this submission is also decent enough

Particulary I can use the Line class approach and use the sanity check approach, where I would track the drastic turns and the light variations and using it, we could possibly fit the lane cureves better,

Also, I think we can go by a more mathematical appraoch in which we can use some advanced Scientific Technique to fit the curves and predict of what is to come next

Also I would like to know, if there are some papers that take a more mathematical approach (ie. Differential Equations or other Scientific Computing techniques) for performing these tasks
