# CarND-LaneLines-P1 
## Introduction
This is the first project in Self-Driving Car Nano-Degree. See [here](https://github.com/udacity/CarND-LaneLines-P1) for the original repo.  The goal of this project to detect two lanes given input images or videos. Two grading files are `P1.ipynb` and the section `reflection` in this document. Outputs are under folder **test_images_output** and **test_videos_output**.


## Reflection

#### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline is in the function `detect_lanes(image)`, which consists of 5 steps.

* **Gray** Call the given helper function `grayscale` to convert RGB to grayscale.
* **Gaussian smoothing** Use kernel window size = 5 to smooth the grayscale image.
* **Canny** Detect canny edges with params: `low_threshold = 10` `high_threshold = 150`
* **Mask** Select the region of interest formed by four vertices. These four vertices vary based on the mounted location of the camera. In this project's setup, four vertices are `v1 = [int(0.21*w),h]`, `v2 = [int(0.44*w),int(0.59*h)]`, `v3 = [int(0.50*w),int(0.59*h)]`, and `v4 = [w,h]`
* **Hough Transform** Vote to get line segments. Params are: `rho = 1`, `theta = np.pi/180`, `threshold = 15`, `min_line_len = 60`, `max_line_gap = 30`

The Hough Transform function calls the `draw_lines()` to lineup the segments into a single long detected lane. The algorithms are given as follows.

1. Seperate line segments into two categories: LEFT and RIGHT based on their slopes. Slope range for LEFT is > 0.1  and slope range for RIGHT is < -0.2.
2. In each category, fit all element segment using one single lane by averging segments' centers and slopes.
3. Draw the left lane and the right lane by giving proper endpoints.

Finally, overlap this detected image with the original image to display RGB results.

#### 2. Identify potential shortcomings with your current pipeline
Shortcommings with this current pipeline include but not limit to:
1. The vertices of the region of interest need to be changed for a different camera-mounting location.
2. The sand on the road could affect the interpolation process.
3. A clear and open lane is needed for detection.
4. Large road curvature could violate the single-straight lane assumption.

#### 3. Suggest possible improvements to your pipeline
1. The lane shape assumption could be curved instead of straight.
2. The interpolation method would be more stable if a weighted-least-square-error scheme could be used.
