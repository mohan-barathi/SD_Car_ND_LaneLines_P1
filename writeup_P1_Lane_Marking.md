# **Finding Lane Lines on the Road** 

## Student name : Mohan Barathi


---

**Finding Lane Lines on the Road**

The goals of this project include:
* Making a pipeline that finds lane lines on the road
* Reflect the work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. The pipeline implementation:

#### The pipeline implemented consists of the following steps:
* First, the image is converted to grayscale, (8-bit image, with each element in the array having possible values from 0 to 255)
* Gaussian blur is used on this grayscaled image, to smoothen it, with an arbitrary kernel size of 5
* Then, Canny edge detection is used to find the edges of the gray-scaled image, with an arbitrary low and high thresholds of 50 and 150 respectively
* Over this edge detected image, I have masked the parts of the image other than lane, using RoI helper function
* Hence, we get an image with lane edges detected, using which hough lines are determined, with following arbitrary values
`rho = 2
 theta = np.pi/180
 threshold = 15
 min_line_length = 40
 max_line_gap = 20`

#### The draw_lines() function is modified, to handle these hough lines, and to come up with 2 solid lane lines:
* Segregating the lanes as left and right
* Finding the average Gradient for left and right lane lines
* Finding the top most co-ordinate for left and right lanes
* Using the top-most co-ordinate and average slope, the bottom-most co-ordinate for left and right lanes are derived
* Respective error handlings are done, where the frames with no lanes are handled properly
* And finally, drawing these 2 lane lines on given image

The output of the above methods would look like this: 

![alt text][./test_images_output/output_Solid_Lanes_solidWhiteRight.jpg]

This works well with the given video clips too.


### 2. Potential shortcomings with the current pipeline

* The co-ordinates for masking are arbitrary, and hence are not generic.
* This is the reason that this implementation didnot work with `Optional Challenge`
* The `draw_lines()` function contains too much logic in it. This should be cleaned up, and broken into multiple functions


### 3. Possible improvements to this pipeline

* The masking, hough, and canny values are set arbitrarily. A way should be deduced to set these values dynamically
* The operation with the hough lines to deduce the lanes can be optimised using object oriented approach. An abstract object that contains the required members and methods, so that different lanes can be inherited, avoiding the duplication as we see in the code
