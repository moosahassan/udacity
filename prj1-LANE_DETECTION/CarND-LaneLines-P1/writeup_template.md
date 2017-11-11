# **Finding Lane Lines on the Road** 

## Outline

### This document provides an overview of the finding lanes project. The key parts of this project included setting up the piple to process the images and using it to find lanes for a video stream.

---

**Finding Lane Lines on the Road**

The steps of this project are the following:
* Setting up the tools related to computer vision and image processing
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Image processing pipeline.

My pipeline consisted of 5 steps. The very first in this pipeline is to load the image and convert it to grayscale.
A couple of considerations during this step were to ensure that video can be supported by treating each frame of video clip as an image. The other consideration was to convert the color image to grayscale so that we can apply edge-detection to identify boundaries of the lane.

In the next step, we utilize Canny function to extract edges from grayscale image. These edges help computers visual shape of objects. As an input to the Canny function, we must supply low- and high-thresholds. These thresholds are to establish how strong an edge has to be. The strength of an edge is proportional to how gradient which in turn is proportional to the difference between surrounding values.

The third step involves extracting the region of interest for further processing. This can be achieved by establishing boundary coordinates within the 2D data of image (x, y). Ensuring a region of interest eliminates lines that may not be representative of lanes. 

In the next step, we use Hough Transform to find intersecting lines within the edges that lie in the region of interest. The Hough Transform is used to convert Image space consisting of pixels (x, y) into Hough space consisting of polar coordinates (rho, theta). The Hough Transform also ensures that we avoid perpendicular lines that have infinite slope in Image space. To operate on an image, the Hough Transform requires these parameters to be specified.

rho -- distance resolution in pixels of the Hough grid
theta -- angular resolution in radians of the Hough grid
threshold -- minimum number of votes (intersections in Hough grid cell)
min_line_length -- minimum number of pixels making up a line
max_line_gap -- maximum gap in pixels between connectable line segments

After a few attempts, a good combination of these parameters was found to be 
(rho = 2, theta = pi/2, threshold = 15, min_line_length = 10, max_line_gap = 50)

As a final step, we processed the Hough lines so that they can be extrapolated and fitted onto the image under consideration. The left and right lanes were processed separately and then placed on the same image. 

[//]: # (Image References)

[image2]: ./examples/lanes-1.jpg "Lanes 1"

[//]: # (Image References)

[image3]: ./examples/lanes-2.jpg "Lanes 2"


![alt text][image1]


### 2. Potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the image dimensions are changed. This could lead to spurious lines being added to the region of interest. 

[//]: # (Image References)

[image4]: ./examples/dimensions-1.jpg "Dimensions 1"

[//]: # (Image References)

[image5]: ./examples/dimensions-2.jpg "Dimensions 2"

Another potential shortcoming would be when the images have lot of jitter. This can be the case in poor road conditions. The jitter in the location of lines would make the lane detection unstable.

[//]: # (Image References)

[image6]: ./examples/jitter-1.jpg "Jitter 1"

[//]: # (Image References)

[image7]: ./examples/jitter-2.jpg "Jitter 2"

### 3. Possible improvements to your pipeline

A possible improvement would be to process each image based on its true diemnsion. A simple approach to address this would be to normalize the data before further processing.


For the jitter effect of edges and lines, it is best to use a moving weighted average to smooth out the effects of changing lines. 