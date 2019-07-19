# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurveOutput.jpg "Solid White Curve"
[image2]: ./test_images_output/solidWhiteRightOutput.jpg "Solid White Right"
[image3]: ./test_images_output/solidYellowCurve2Output.jpg "Solid Yellow Curve 2"
[image4]: ./test_images_output/solidYellowCurveOutput.jpg "Solid Yellow Curve 1"
[image5]: ./test_images_output/solidYellowLeftOutput.jpg "Solid Yellow Left"
[image6]: ./test_images_output/whiteCarLaneSwitchOutput.jpg "White Car Lane Switch"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 4 steps. First, I converted the images to grayscale. Then I applied a Gaussian filter to the
grayscale image and I performed Canny edge detection.  For Canny edge detection, I used a low threshold of 50 and a high
threshold of 150.  Following edge detection, I applied a region of interest mask using a trapezoidal shape.  This step
was followed by application of the Hough Transform.  The parameters I used for the Hough Transform were as follows:
distance resolution of 1 pixel, angular resolution of 1 degree (converted to radians), a minimum number of votes of 35,
a minimum of 25 pixels making up a line, and a maximum gap between connected line segments of 25 pixels.  Additionally,
I decided to apply the Hough Transform to the left and right halves of the image separately.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging the the
lines detected by the Hough Transform.  I did this by taking each pair of points defining a line detected by the Hough
Transform and I calculated the slope and y-intercept of that line.  I then averaged those slopes and y-intercepts
together to define one line.  Following this, I calculated the points on this line that fell inside the masked image
region I defined.  I then extracted two points to represent this line by finding the points with the maximum and
minimum y-values.  The draw_lines() function then drew this line.

To demonstrate the performance of my pipeline, I ran my pipeline on the test images.  The results are shown below.

Solid White Curve:

![alt text][image1]

Solid White Right:

![alt text][image2]

Solid Yellow Curve 1:

![alt text][image4]

Solid Yellow Curve 2:

![alt text][image3]

Solid Yellow Left:

![alt text][image5]

White Car Lane Switch:

![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming of my pipeline is that lane lines that are not solid do not have enough sample points in the
image to extrapolate well.  In this case, the predicted lane line intersects the actual lane line; but usually, the
slope is either too steep or not steep enough.

Another shortcoming could be the way the pipeline handles curved roads.  The images the pipeline handles well are straight
sections of road.  The lane lines of roads with more curvature would not be predicted well by the Hough Transform for lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement may be to use the median line as a prediction instead of the average.  The RANSAC algorithm would
also be an effective approach to filter out the influence of outliers.

Another potential improvement could be to average the predicted lines from consecutive frames of video using a higher
weight for more current video frames in time and lower weights for older video frames.
