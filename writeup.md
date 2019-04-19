# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

1. Use color selection to pick out the yellow and white pixels in the image.
2. Use region of interest masking to mask out the areas of the image that are not roadway.
3. Convert the image to grayscale.
4. Use Canny edge detection to detect the edges of the painted lines in the roadway.
5. Use the Hough transform to convert Canny edges into line segments.

In order to draw a single line on the left and right lanes, I **redefined** the draw_lines() function by:
1. Sorting Hough line segments into those belonging to the left and right lanes.
Then for each lane:
  1. Computing the average x- and y-coordinates of all the line segments making up that lane. (The intuition is to compute the midpoint of the line to draw.)
  2. Also computing the average slope of all the line segments.
  3. To minimize jitter, the slopes from the last three frames were averaged.
  3. Using the average slope to extend the "midpoint" to the top and bottom of the region of interest. These are the lines to overlay on the image.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming is that white or yellow cars can be misdetected as part of the lane. (This can be a problem if the car is in the neighboring lane and grazes the region of interest mask, but it is especially a problem if the car is in the roadway ahead.)

Another shortcoming is that the pipeline is brittle to changes in color; i.e. if the roadway is very pale or the lanes are heavily shaded, color selection may no longer be able to pick out the lane lines and only the lane lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to detect car-shaped objects and at the part of the pipeline when line segments are sorted into left and right lanes, to not include line segments lying within those objects.

Another potential improvement could be, prior to color selection, to detect patches of shadow and "erase" them by brightening those patches until they match the neighboring pixel's colors.
