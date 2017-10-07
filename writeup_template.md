# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I built the pipeline according to the tutorial.
First grayscale the image than use a Gaussian filter for averaging to get a smoother image.
Next step: finding the edges with the Canny algorithmn. With edges is meant the gradients of pixels with respect to the surrounding pixels. With the two thresholds the range is set, which pixels will be dropped and which pixels will be marked as 'strong' pixels.

Next task was to define the shape of the relevant image section. The polygon that defines the relevant part of the image.
I was surprised that it can be difficult to find the right shape here.

The next step was to apply the function for the region of interest that was defined by the vertices.

Next step was to transform the found lines to Hough space. This is necessary to get the pixels/lines that really describe lane lines in the image. 
The function includs the draw_lines function. This function is the crucial part of the pipeline.
I modified the draw lines function by evaluation the slope of each found lines by the Hough space transformation.
If the slope is < 0, the line is part of the left lane line of the image and vice versa.
After separation of the lane lines I calculated the mean values of the parameters because the objective is to have only one solid line in the image.
I used the equation for a linear line y = mx + b to calculate the intercept and by transforming the equation to x = (y-b)/m I got the x-values for my lines which correspond with the lane lines.
THe parameters for the y-axis were set by the image shape (y_max) and by finding the minimum y-value of my region of interest (y_min).
I included an if-statement to check if the mean value of the slope is 'Not a Number'. That happened sometimes and raised an error during processing of the videos.

Last step is to insert the lines to the initial image by applying the weighted_img function.



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is a section on the road without any lane lines. 

Another shortcoming could be the performance for curves or uphill and downhill driving because the region of interest is set with the defined vertices.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a adaptive region of interest to react to changing driving conditions.


