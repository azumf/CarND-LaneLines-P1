# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goal of this project is to find lane lines in images and highlight them.


[//]: # (Image References)
[//]: # (use it with  ![hog features][image1])

[image1]: ./examples/grayscale.jpg 
[image2]: ./examples/laneLines_thirdPass.jpg
[image3]: ./examples/line-segments-example.jpg
---

### Reflection

### 1. Software pipeling for simple lane lines finding

![title][image3]

#### 1.1 Grayscale the image

For further processing (e.g. to Hough Space) the image needs to be grayscaled. Therefore the function `grayscale()` is implemented. It uses openCV to return a grayscaled copy for an input image.

#### 1.2 Gaussian Blur

To average the pixels of the image a Gaussian filter was applyed to it. The outcome of this step is a smoother image.

#### 1.3 Edge detection with Canny Edge Detection

Next step: finding the edges with the Canny algorithmn. The edges are the gradients of pixels with respect to the surrounding pixels. With the two thresholds of the `canny(gray_img, thresh1, thresh2)` function, the range is set which pixels will be dropped and which pixels will be marked as 'strong' pixels.

#### 1.4 Region of interest

Next task was to define the shape of the relevant image section, the polygon that defines the relevant part of the image.
I was surprised that it can be difficult to find the right shape here.
Finally, the following edge points showed the best performance:
`[[(0,imshape[0]),(450, 320), (500, 320), (imshape[1],imshape[0])]]`

#### 1.5 Transform to Hough space

Next step was to transform the found lines to Hough space. This is necessary to get the pixels/lines that really describe lane lines in the image. The function includes the draw_lines function. This function is the **crucial part of the pipeline**.
I modified the draw lines function by evaluation the slope of each found lines by the Hough space transformation.
If the slope is < 0, the line is part of the left lane line of the image and vice versa.

After separation of the lane lines I calculated the mean values of the parameters because the objective is to have only one solid line in the image.
I used the math. equation for a linear line `y = mx + b` to calculate the intercept and by transforming the equation to `x = (y-b)/m`, I got the x-values for my lines which correspond with the lane lines.

The parameters for the y-axis were set by the image shape `(y_max)` and by finding the minimum y-value of my region of interest `(y_min)`.
I included an if-statement to check if the mean value of the slope is 'Not a Number'. That happened sometimes and raised an error during processing of the videos.

#### 1.6 Insert lane lines to raw image

Last step is to insert the lines to the initial image by applying the weighted_img function.



### 2. Potential shortcomings with current pipeline

One potential shortcoming would be what would happen when there is a section on the road without any lane lines. 

Another shortcoming could be the performance for curves or uphill and downhill driving because the region of interest is set with the defined vertices.



### 3. Possible improvements to the pipeline

A possible improvement would be to have a adaptive region of interest to react dynamically to changing driving conditions.


