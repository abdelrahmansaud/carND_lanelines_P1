# **Finding Lane Lines on the Road** 

## Writeup Template

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[gray]: ./examples/grayscale.jpg "Grayscale"
[region]: ./test_images_output/output_region_solidYellowLeft.jpg "region"
[final]: ./test_images_output/output_solidWhiteCurve.jpg "final"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale.

![alt text][gray]

Then I applied gaussian blur to filter out image noise and smooth the image, then I applied algorithm for edge detection called Canny edge detector which, at this step, detects edges accross the image, so since for the given set of images the lane lines start wide from the bottom of the image and straight forward towards the center, I draw a polygon on that area then apply a mask to filter out the rest of the image, leaving only that area of interest. 

![alt text][region]

After that the Hough transorm algorithm is applied to find lines, it does so by a voting procedure in parameter space, after tuning the parameters for the algorithm the result is just the extracted line on a black image, so that is weighted with the original image which results in a normal image with the extracted lines highlighted on it.

![alt text][final]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function so that it iterates over the lines and calculates the slope and center for each of them, a line with a positive slope would belong to the right lane while a negative slope indicates that it belongs to the left lane line.

After averaging the slopes and centers, we now have a slope and a point on it for each lane line which we can use to extrapolate, we can use them in the point-slope formula to find other points along that line (only 2 are needed for each line of course). This results in two long lines.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen if the lane lines are not pointing forward, for example if the car is turning or not already in pointing in the right direction, the lane lines would be filtered out during identification of region of interest.

Another shortcoming could be that all the images were in daylight, with a not crowded traffic in it, so the solution was developed and tested on specific conditions so it's not known how it would behave on other conditions.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have more images taken in different conditions than the current set and tune the algorithm accordingly if needed.
