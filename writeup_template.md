# **Finding Lane Lines on the Road** 

## Project report

---

**Finding Lane Lines on the Road**

In this report, I will briefly go over the following.

* A pipeline that finds lane lines on the road using opencv
* Reflection
* Possible shortcomings in my procedure. 


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./examples/canny_edge_image.jpg "Edge detection on euqalized image"
[image3]: ./test_images/processed_solidYellowCurve.jpg "Solid Yellow curve example"

---


---
### Pipeline
- Convert original image into gray scale. 
- Apply gaussian filter to smoothen the image. Used a smaller kernel size to get a better image.
- Equalize the image using openCV for better edge detection.
- Detect edges using canny edge detection. 
- select a region of interest from the image that comprises of lanes.
- Apply hough transform to obtain various line segments detected in  masked image(in ROI)
  * Threshold : provides you information of number of curves in m and b domain that intersect at a single point  to be considered as line segments in your image.
  * min_line_len:  of the various line segments that pass your threshold criteria, choose the line segments that are at least of this much length. 
  * max_line_gap: Farthest distance between two points to consider them as line segments. This value should be close to min_lin_len as it may turn out that there may not be no line segment satisfying both the conditions.
-Draw lines to clearly depict lanes detected by your model. 

![alt text][image1]

---
### Reflection

 In this setcion, I will breifly go over some details and challenges. Firstly, the idea of equalisation seems to improve image clarity  after converting to gray scale. At a glance, it seems to help with edge detection better. However, this also results in some unessecary gradients to pop up during edge detction(mostly these are horizontal line segments). Unfortunately, these couldnot be cleared by adjusting the values of threshold in canny edge detetion procedure. I took care of them by adjusting slopes to consider during drawing lines.

![alt text][image2]

Another important step was to select higher values of threashold, min_line_len and max_line_gap. As suggested in reading material, these help in getting rid of spurios line segments and make life easier in coming up with a simpler draw_lines().
Also a point to be noted that, too high values might not provide even a single line segment at the output of hough_lines function.

Now coming to draw_lines(). The main idea behind this fucntion is to obtain the slope of lane and find a point, so that we can draw a line that matches the lane. Once you obtain the information of a line overlaping with the lane, I computed the end vertices of lanes by using the vertices chosen from region of interest vertices. This will be clear from my code.

I wrote a fucntion to compute slope of each line segment detected by hough_lines. In this fucntion I discarded any slope whose absolute value is less than 0.35. Chose this number by trail and error. The main goal was to avoid the unessacary gradients obtained after equalisation. Later, binned all the sloped and observed that these are very close by in value. Also appeneded the mid point of each valid line segment correspond to slope computed as a point on the line. 
Once I had a slope and point for each line segment, I computed optimal slope by computing mean of different slopes. Then choose the midpoint of the line segment that has closest slope to mean as optimal point. These are lablelled appropiately in my code. Using these two values and end points of the line segments, drew lines over lanes.


![alt text][image3]


### 2. Identify potential shortcomings with your current pipeline

On of the major shortcomming may be not being able to obtain line segments from the hough_lines function. Typical scenarios would be sharp curve, due high values of threshold, min_line_len and max_line_gap. Also, computing the mean of all possible slope will result in incorrect lane marking. 


### 3. Suggest possible improvements to your pipeline
For shape turns shrink the region of interest and also adjust the threashold value to breakdown the lane into smaller linear line segments. This will help get us better accuracy in finding lanes.
