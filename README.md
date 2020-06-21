# SFND 3D Object Tracking

Welcome to the final project of the camera course. By completing all the lessons, you now have a solid understanding of keypoint detectors, descriptors, and methods to match them between successive images. Also, you know how to detect objects in an image using the YOLO deep-learning framework. And finally, you know how to associate regions in a camera image with Lidar points in 3D space. Let's take a look at our program schematic to see what we already have accomplished and what's still missing.

<img src="images/course_code_structure.png" width="779" height="414" />

In this final project, you will implement the missing parts in the schematic. To do this, you will complete four major tasks: 
1. First, you will develop a way to match 3D objects over time by using keypoint correspondences. 
2. Second, you will compute the TTC based on Lidar measurements. 
3. You will then proceed to do the same using the camera, which requires to first associate keypoint matches to regions of interest and then to compute the TTC based on those matches. 
4. And lastly, you will conduct various tests with the framework. Your goal is to identify the most suitable detector/descriptor combination for TTC estimation and also to search for problems that can lead to faulty measurements by the camera or Lidar sensor. In the last course of this Nanodegree, you will learn about the Kalman filter, which is a great way to combine the two independent TTC measurements into an improved version which is much more reliable than a single sensor alone can be. But before we think about such things, let us focus on your final project in the camera course. 

## Algorithm Steps

1. Images are stored in a ring buffer.
2. Bounding boxes are created with YOLO network. Which establishes region of interests (ROI).
3. Lidar data has been cropped to include preceding vehicle.
4. ROIs are associated with lidar data.
5. Features on images are detected and tracked with different detectors and descriptors.
6. Bounding boxes are matched using the feature matching.
7. TTC lidar is calculated using the cısecutive frames of each bounding box.

## FP.5 : Performance Evaluation 1

Even if preceding vehicle comes closer at frame of the lidar data calculated TTC value for lidar does not decreases monotonocially. This is mainly due to increased box size with decreased distance. Even if we implemented a median filter it only helps a little. Therefore, outlier detection should be implemented as a next step.

## FP.6 : Performance Evaluation 2

TTC estimation for camera keypoints are directly depended on selected detector, descriptor and matcher combination. When we evaluate the performance of TTC calculation using camera we can see that, performance increases as number of keypoints therefore matches increase. When we looked at the results of previous projects approximately same detector and descriptor combinations also perform well here. For example when we look at the HARRIS detector its performance seems worse compared to SHITOMASI, FAST, and SIFT. 

In below best performing combinations compared to Lidar TTC is given. Average error from lidar ttc is considered.

Detector Type | Descriptor Type | Avg Deviation From Lidar |
--|--|--|
FAST | BRIEF | 0.13 |
--|--|--|
SIFT | BRISK | 0.072 |
--|--|--|
AKAZE | ORB | 0.09 |
--|--|--|

Results in more detail can be found in comparison.csv

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level project directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./3D_object_tracking`.