#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of N steps. 
1. Convert frame to grayscale 
2. Detect edges
3. Apply a region of interest filter to consider only edges in the triangle formed by the lower corners and middle of the image.
4. Use houghlines to detect line segments.  
5. Filter, group and average the segments to determine the left and right lane lines.

My approach deviated from the helper code in the following ways:
1. In addition to using standard RGB2GRAY conversion from OpenCV, I also converted to HSV, then extract the 'saturation' channel.  This approach was used to better distinguish a yellow lane line from a gray road.
2. To perform edge detection, I did not use the Canny detector.  Instead, my goal was to detected oriented-edges approximately along the direction where the right and left lane is expected to be.  This was done by computing the image gradient and taking the magnitude in the directions of the inner edge of each expected lane.  The result was 4 edge images (left/right x gray/sat) which were then merged using bitwise OR.
3. Modified draw_lines to filter line segments by slope, x-intercept with the bottom of the frame, group them by the x-intercept as left/right lane lines, then average each group.  I draw the segment of the resulting line between image height*5/8 to height.

###2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the lane lines are not approximately aligned with the vehicle's direction of travel.

Another shortcoming could be that another vehicle is in directly in front of us.

###3. Suggest possible improvements to your pipeline

A possible improvement would be to use expectation maximization in draw_lines to determine the lane lines based on an expected initial position of the lane lines.

Another potential improvement could be to not merge the 4 edge images and use the output of houghlines from each as input to the filtering and grouping algorithm.

Another potential improvement is tracking the line segment in each frame and using the position and interframe image difference as additional input into the lane detection algorithm.

Another potential improvement is fine-tuning the position of the final line segment to best fit the maxima in the gradient image.
