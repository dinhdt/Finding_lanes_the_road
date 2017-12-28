# **Finding Lane Lines on the Road** 

The goal in this project to detect the lane markings of the own lane and mark those in the image.



The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report



---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline consists of following steps
1. Convert image to grayscale
2. Reduce noise in image using Gaussian blur
3. Get edges in image using Canny edge detection
4. Remove pixels from edges image outside region of interest
5. Transform edge pixels to Hough space to find lines in image
6. Get line parameters for left/right lane
7. Draw lanes and overlay with original image within ROI


In order to find the parameters of the left and right lane, all pairs of points that are outputted from the cv2.HoughLinesP function are clustered depending on their gradient to either left or right lane.
E.g. a line that has a negative gradient (dy/dx = ((y2-y1) / (x2 - x1)) < 0) belongs to the left lane and vice versa.
The mean point and gradient is determined by averaging all points and gradients in the respective cluster. After calculating the mean point and gradient the missing bias parameter is determined. In the end all line parameters are determined and used for generating points as an input for the cv2.draw() function in order to draw the lines.




### 2. Potential shortcomings

When no lines are detected the program crashes in the averaging step. Furthermore there is weak performance on curvy lanes since the algorithm is looking for straight lines.

### 3. Possible improvements
The first shortcoming can be easily improved by using exception handling.
In order to handle curved lanes using the current algorithm, one should try to find segments of straight lanes since we are searching for lines and average over them.
Therefore one could slice the image in y direction into several parts such that these parts contain only straight sub-lanes and then run the pipeline on each of those slices and concatenate all the intermediate results at the end. 
