
# **Finding Lane Lines on the Road** 

## Writeup 

###  by Brandon  (brandlee@umich.edu)

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_blurgray.jpg
[image2]: ./test_images_output/solidWhiteCurve_canny.jpg
[image3]: ./test_images_output/solidWhiteCurve_Hough.jpg
[image4]: ./test_images_output/solidWhiteCurve_Solid_Lanes.jpg
[image5]: ./test_images_output/solidWhiteCurve_mask.jpg


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. All tested output images produced in each step are in the test_images_output folder.

#### First, I converted the images to grayscale, and I also apply Gaussian Blurring to smooth the figure.    Here is one example. 

![alt text][image1]

#### Second, applying Canny function to find the edges. 

![alt text][image2]

#### Third, we need to define the area of interest, and mask the outside portion. 

![alt text][image5]

#### Fourth, with the Canny output we can apply Hough Transform to define lines (plot in red)

![alt text][image3]

#### Finally, the section of lines are extrapolated to solid lines (plot in green)

![alt text][image4]


#### Draw Single Solid Line
   In order to draw a single line on the left and right lanes, I did not directly modify the draw_line function.  Instead, I define a new function (Avg_Exp_lines) that takes the lines from Hough transform, and treats the end points as data points for first order fitting.  The data points are separated into two groups before the fitting in order to define left line and right line.  
   
   Even if the first order fitting returns two lines, additional steps are provided to fine tune the end points of the single line.  This tune is necessary for having a more stable line during the video lane finding.  The concept is to check if both end points are close to the edges of interested area.  If not, a y-value is picked (in my case, I picked the middle point in y-axis) and inserted into the linear function to obtained the x value.  This x and y become the updated end point of the solid line.  Most of time it works very well, but the drawback is there and will be discussed later in the next part



### 2. Identify potential shortcomings with your current pipeline


   One potential shortcoming would be what would happen when the lane lines are further from the camera and therefore the edges are too short to be selected by Hough Transform.  It is possible to reduce the value of mininum line length to capture those, but this also increase the chance to involve unwanted line.  This becomes a trade-off but s reasonable decision should be focus more on the closer route and pay less attention on the further one.  In addition, after the extrapolation this undetected lines become less critical. 

   Another shortcoming could be the pipeline cannot avoid some roadside dots which eventually generate lines after Hough transform.  This becomes very annoying in the yellow line video and it is visible that many small lines spike out of the lane lines.  Yes those may not be a big deal after the first order fitting, but it certainly affect the stability of the solid lines shown on the video.  


### 3. Suggest possible improvements to your pipeline

   A possible improvement would be to fine tune the area of interest at each frame.  This is more like a real-time tuning and can be achieved by analyzing the result of previous frame.  For example, the current frame finds the end points of the solid line is (x1,y1),(x2,y2), then the area of interest will be a certain amount of pixels away from these points.  In other words, increase the number of interested regions but reduce the total amount of area.  This may be helpful when there is a white car in front of you and sure there will be many edges in the middle of the image.  If the searching area does not include the middle of the lane, this awkard situation may be avoided. 

Another potential improvement could be to eliminate the noise by removing the outliers.  During the coding and debugging process, I found that there are several lines whose slopes are way off the others.  A better way to do this may be define the slope first, eliminate the outliers, and then obtain the remaining lines for extrapolation.  This may return s much stable solid lines.  Initially I would like to do this in my code, but I already passed due and I do not know how to better control array index in a loop.  Hope I'll be back for this when I know Python better and have more time.  




```python

```
