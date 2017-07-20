# **Finding Lane Lines on the Road** 

### This is a brief write-up for Project 1 of Udacity Self Driving Cars Nanodegree. 




[//]: # (Image References)

[image1]: ./report_images/gray.png 
[image2]: ./report_images/gray_blurred.png 
[image3]: ./report_images/edges.png 
[image4]: ./report_images/masked_edges.png 
[image5]: ./report_images/lines.png 
[image6]: ./report_images/avg_lanes.png 



---

## Description of the lane detection pipeline

### 1. Finding Lines 

The initial pipeline consisted of the following steps. 

1- Convert the image to grayscale.

![alt text][image1]

2- Apply a Gaussian smoothing filter.

![alt text][image2]

3- Use Canny algorithm to obtain the edges in the image.

![alt text][image3]

4- Apply a polynomial mask to focus on the desired area of the image that we expect to see the lines.

![alt text][image4]

5- Apply a Hough transform to the detected edges, to get lines.

![alt text][image5]

### 2. Averaging the lines
At this point, there are multiple lines detected for a lane line, and some lines are partially recognized. Ideally, we would like to have only two complete lines (one for the right and one for the left) per lane.

Since the location (0,0) starts from top left of the image and the positive direction of "y" is towards the bottom of the image, the left lane should have a negative slope, and the right lane should have a positive slope. Therefore, we will separate positive slope lines from  negative slope lines and take the averages of them. I performed this task using the function **average_lines** which takes *lines* from the output of the Hough transform and returns two weighted average lines, *left_lane* and *right_lane* as outputs.

The output of function **average_lines** is in "slope" and "intercept" form. I defined a simple function **line2pixles** that takes the line defined in slope-intercept form and the vertical coordinate of the two ends of the line *y1* and *y2* and returns the lines in terms of (x,y) coordinates or pixels. 

Next, I defined another function **lane_lines** which uses **average_lines** to obtain the averaged left and right lanes in slope-intercept form and converts them to pixels using **line2pixels**. The output of this function is again *left_lane* and *right_lane* but in (x,y) form.

Finally, function **draw_lines** takes the original image and the output of **lane_lines** as inputs and draws the averaged lane lines in the corresponding location on the original image.  


## 2. Identify potential shortcomings with your current pipeline
The current algorithm has some potential shortcomings.

The main shortcoming of the algorithm is that it can only handle straight lines. It's difficult to detect curved lines using this algorithm; however, the curved lines usually appear at further distances unless it's a steep curve.  

One potential problem would be what would happen when there are other lines and edges in the masked area of the image such as pedestrian crosswalk lines, street signs, other objects such as cars and even edges due to the damages on the asphalt. This might lead to recognition of undesired lines in the expected area and distortion of the algorithm.

Another shortcoming could be when the shoulder of the road is very narrow and the distance between the lines and other objects such as trees, walls and guard rails is very small which could cause problems.

Additionally, the current algorithm might not work well for steep and hilly roads since the masked area is assumed to be in the center of the image.

## 3. Suggest possible improvements to your pipeline

In order to be able to handle curved lines, we can use perspective transformation and also use polynomials to fit the lines instead of straight lines. For steep and hilly roads, we need to detect the horizontal line between the earth and the sky and adjust the masked area based on that.


