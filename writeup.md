# **Finding Lane Lines on the Road** 

### This is a brief write-up for Project 1 of Udacity Self Driving Cars Nanodegree. 




[//]: # (Image References)



---

### Reflection

### 1. Description of the initial pipeline

The initial pipeline consisted of the following steps. 

1- Convert the images to grayscale.

[image1]: ./report images/gray.png "gray"

2- Apply a Gaussian smoothing filter.

[image2]: ./report images/gray_blurred.png  "gray_blurred"

3- Use Canny algorithm to obtain the edges in the image.

4- Apply a polynomial mask to focus on the desired area of the image that we expect to see the lines.

5- Apply a Hough transform to the detected edges to get lines in the expected area of the image.

### 1. Averaging the lines
At this point, there are multiple lines detected for a lane line, and some lines are partially recognosed. Idealy, we would like to have only two average lines (one for the right and one for the left) per lane.


Since the location (0,0) starts from top left of the image and the positive direction of "y" is towards the bottom of the image, the left lane should have a positive slope, and the right lane should have a negative slope. Therefore, we will separate positive slope lines from and negative slope lines and obtain the averages of them. I perform this task using the function **average_slope_intercept** which takes *lines* from the output of the Hought transform and returns two wighted average *left_lane* and *right_lane* as outputs.

The output of function **average_slope_intercept** is in **slope** and **intercept**. I defined a simple function **make_line_points** that takes the the line defined with **slope** and **intercept** and as well as the vertical coordinate of the two ends of the line **y1** and **y2** and returns the lines in terms of (x,y) coordinates or pixles. 

Next, another function **lane_lines** is defined which uses **average_slope_intercept** to obtain the left and right lanes in slop and intercept and converts them to pixles using **make_line_points**. The output of this function is again *left_lane* and *right_lane* but in coordinates (x,y).

Finally, function **draw_lines** takes the **original image** and the output of **lane_lines** as inputs and draws the averaged lines on the original image in the corresponding location on the  



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are other lines and edges in the masked area of the image. This will lead to recognition of undesired lines in the expected area and distortion of the algorithm.

Another shortcoming could be when the shoulder of the road is very narrow and the distance between the lines and other objects such as trees and walls and guard rails is very small which will cause problem.

Also, the other problem that might happen is when another car or object in in front of the car in the masked area which might again distort the lines. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
