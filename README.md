# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

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
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

# Project Writeup

## MP.1 Data Buffer Optimization
Lines 69-75 in MidTermProject_Camera_Student.cpp 

This task was implement with the use of the `while` loop, which check the buffer size against defined threshold `dataBufferSize`. While condition is  true the first image is deleted and the next image is appended.

## MP.2 Keypoint Detection
Lines 84-113 in MidTermProject_Camera_Student.cpp 
Lines 154-273 in matching2D_Student.cpp

This task required to implement a set of detectors including HARRIS, FAST, BRISK, ORB, AKAZE, and SIFT and make them selectable by setting a string accordingly. The Harris detector was implemented as in the classroom. The remaining detectors were implmented using the OpenCV library e.g. `cv::FastFeatureDetector::create();` for FAST. It is important to higlight that with new version of OpenCV library the syntax may change and for version 4.3.0 the implementation for SIFT detector was diffrent than in the classroom; i.e. `cv::SiftFeatureDetector::create()` instead of `cv::xfeatures2d::SIFT::create()`.

## MP.3 Keypoint Removal
Lines 118-133 in MidTermProject_Camera_Student.cpp 

Function `contains()` which checks whether the rectangle contains the point was used to filter out the keypoints beyond the defined rectangle. 

## MP.4 Keypoint Descriptors
Lines 165-174 in MidTermProject_Camera_Student.cpp 
Lines 66-109 in matching2D_Student.cpp

Simialry to the keypoints detection implementation the OpenCV libary was used to add a set of descriptors including BRIEF, ORB, FREAK, AKAZE and SIFT. Note that in this implementation the AKAZE descriptor works only with AKAZE detector.

## MP.5 Descriptor Matching
## MP.6 Descriptor Distance Ratio
## MP.7 Performance Evaluation 1
## MP.8 Performance Evaluation 2
## MP.9 Performance Evaluation 3