# **Finding Lane Lines on the Road** 

## Project Writeup

---

**Finding Lane Lines on the Road**

- The goals of the project were to construct a pipeline that finds lane line on an image, then apply that pipeline to a series of images in a video.

---

### Reflection

### 1. Description of the pipeline


The pipeline consists of the following main steps:  
   
- Converting the image to grayscale using the grayscale_img function
- Applying a Gaussian blur on the grayscaled image  
- Using Canny edge detection to detect edges on the image
- Applying the Hough transformation on the edges, which involves the draw_lines function, described in detail below
- Adding an appropriate image mask 
- Applying the lines on the original image to achieve an annotated version of it

In order to draw a single line on each lane line and to reduce the shaky/jittery movement of the lines, I used the following method in the draw_lines function:

- Iterating through all the line segments detected by Canny edge detection, given as the 'lines' parameter
- Validating their slope, filtering out near-horintal and near-vertical lines (which cannot be lane lines)
- Classifying them as left or right lane lines based on their slope
- Drawing left and right lane lines from the corresponding line segments. This involves:
    - Averaging the coordinates of the segments and deciding on an 'average' line. 
      I found that this method work better then a simple linear regression.
    - If there are not many data points in the current frame, we use the cache to draw a line.
    - If there are enough data points in the image, we add the resulting line to the cache, and get the final line
      using a weighted average of the cache and the line in the current frame, thus achieving a more stable effect 
      and reducing some of the jitter

### 2. Potential shortcomings

One shortcoming of the pipeline is that it cannot handle the challenge video. It contains shadows, large curves of lines and other factors that make it difficult for this simple lane finding algorithm to work. 
Naturally this is a naive implemenation of the lane finding algorithm.

### 3. Possible improvements to the pipeline

A possible improvement would be to apply more advanced color filtering, fine-tuning the Canny detection and Hough transorm parameters, applying a more advanced smoothing function, and finding a better mask to use on the images.
