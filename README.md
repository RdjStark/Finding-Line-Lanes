## Finding Lane Lines
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


[//]: # (Image References)
[image1]: ./test_images_output/origin.jpg "origin"
[image2]: ./test_images_output/gray.jpg "gray"
[image3]: ./test_images_output/blur.jpg "blur"
[image4]: ./test_images_output/edges.jpg "edges"
[image5]: ./test_images_output/roi.jpg "roi"
[image6]: ./test_images_output/lines.jpg "lines"
[image7]: ./test_images_output/result.jpg "result"


Overview
---
In this venture, I will utilize the devices about computer vision to distinguish path lines out and about. I will build up a pipeline on a progression of individual pictures, and later apply the outcome to a video stream (extremely only a progression of pictures).


The project
---
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Test on the pipeline on images and videos
* Improve this pipeline
* Test the improved pipeline on images and videos


### Dependencies
This lab requires docker images:

* [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit)


Ideas for Lane Detection Pipeline
---
Some OpenCV functions I have used for this project are:

cv2.inRange() for color selection  
cv2.fillPoly() for regions selection  
cv2.line() to draw lines on an image given endpoints  
cv2.addWeighted() to coadd / overlay two images cv2.cvtColor() to grayscale or change color cv2.imwrite() to output images to file  
cv2.bitwise_and() to apply a mask to an image  


Build a Lane Finding Pipeline
---
Using the following image as example, my pipeline consisted of 6 steps. 
![alt text][image1]

1. Converted the images to grayscale from RGB model 
![alt text][image2]  

2. Use cv2.GaussianBlur() to blur the image
![alt text][image3]  

3. The first core operation: detect edges of a gray model image
![alt text][image4]  

4. After the edges have been got, my next step is to define a region of interest(i.e., ROI), this method is old but efficient. Cause the camere installed on the car is fixed, so the lane lines is in a specific region, usually a trapezoid.
![alt text][image5]  

5. Anothe core operation: hough transform edges to a set of lines represent by start point and end point. Hough transform get the image of lane lines we want.
![alt text][image6]  

6. Finally, we add the lane lines image and innitial image together.
![alt text][image7]  


Test on videos
---
You know what's cooler than drawing lanes over images? Drawing lanes over video!

We can test our solution on two provided videos:

`solidWhiteRight.mp4`

`solidYellowLeft.mp4`  

The result is in "test_videos_output" directory : solidWhiteRight.mp4 and solidYellowLeft.mp4.


Improved lane finding pipeline
---
At this point, I have the Hough line segments drawn onto the road, but what about identifying the full extent of the lane? Therefore I will improved my pipeline to do this. 

The first step is to diveded the detected lines into left set and right set.
Then in each line set, several special lines are choosen as basic to draw full line. 

The result is in "test_videos_output" directory : white-improved.mp4 and yellow-improved.mp4.


Reflection
---
### 1. Recognize likely deficiencies with your present pipeline 


One potential weakness would be my pipeline can't discover path lines precisely when there are a lot of sunshine or cross of light. Cause in grayscale model of the picture, such a large number of futile edges will be find as edges of path lines by watchful discovery. In this way, the genuine path lines can't be isolated from the commotion edges by hough change, which cause genuine outcome. 

Another deficiency could be there are an excessive number of paraments must be turn, possibly I can locate a lot of paraments fit one sort of condition, similar to straight path lines under bright day(the most straightforward case). Notwithstanding, nobody can discover all the paraments for all circumstances, or even a wide range of cases can't be identified. 

Also, my pipeline cost a lot of time, which can not adjust to the genuine vehicle running quick.


### 2. Possible improvements

A potential improvement is locate a progressively productive and vigorous approach to recognize path lines. Shading space can be utilized to concentrate how to discover white and yellow all the more proficiently, and the best approach to fit path lines can be update by polynomial. 

Another potential improvement could be to utilize equal calculation to quicken my pipeline, we as a whole know Nvidia accomplish great work at this space, accordingly utilize their innovation is a superior way.
