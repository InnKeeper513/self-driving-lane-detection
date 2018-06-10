**Finding Lane Lines on the Road**

### Reflection

### 1. Pipeline Description

My pipeline consisted of 6 different stages
1. Color filter, it is the first step used to filter out noise, by getting rid of irrelevant colors
2. Convertion to greyscale images, using the color filtered image
3. Apply Gaussian blur onto the greyscale image to further reduce noise
4. Use Canny Edge on the Gaussian blured image to find all possible lane edges
5. Add a mask to the canny edge picture, so the only edges being detected is from the lane the car is driving on
6. Find lanes on the path by using HoughLine.

After the receiving all the results from the hough line, divide all of these lines into either left or right lane. Then apply a filter into these left and right lane to make sure that all left lane must stay inside the left lane area, which is from the left side of the mask area to the middle, and same to the right. 

We want to get an average slope value and coordinates value for calculating the current picture's slope value. 

But to get rid of invalid slope data, apply a backtrack algorithm, remember the slope and previous line coordinate from the previous picture. If the current slope for some line is greater or smaller than the previous slope by some threshold value, then just add the previous slope value and coordinate value to get an average slope value. 

If at last, there are no slope can be used to calculate the average slope value, just use the previous slope value and coordinates instead. 

This approach is reasonable, because lane slope should not change by a large amount for each picture (The previous 5 pipeline steps's result can change base on different environmental factors) Therefore, this approach is the safest to proceed.

### 2. Shortcomings of the algorithm.


What if the first several picture has incorrect results? Then the backtrack algorithm will most likely produce incorrect values, the most I can do to avoid this situation is to average up all the slope and to spend more time on color filtering to make the picture data is accurate. 

If the car is making a lane change, then this algorithm will not work, because It can only draw two slopes inside a maskes area.

Since this is only getting 2 slopes with 4 line coordinates, if the car is having a sharp left turn or a sharp right turn, It will not be able to produce an accurate result since It can only draw line not arc.

### 3. Suggest possible improvements to your pipeline


Instead of only getting coordinate values, also get radius values for generating arcs.