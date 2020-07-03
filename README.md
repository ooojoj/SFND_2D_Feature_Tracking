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

This task was implemented with the use of the `while` loop, which checks the buffer size against defined threshold `dataBufferSize`. While condition is true the first image is deleted and the next image is appended.

## MP.2 Keypoint Detection
Lines 84-113 in MidTermProject_Camera_Student.cpp  
Lines 154-273 in matching2D_Student.cpp

This task required to implement a set of detectors including HARRIS, FAST, BRISK, ORB, AKAZE, and SIFT and make them selectable by setting a string accordingly. The Harris detector was implemented as in the classroom. The remaining detectors were implemented using the OpenCV library e.g. `cv::FastFeatureDetector::create();` for FAST. It is important to highlight that with new version of OpenCV library the syntax may change and for version 4.3.0 the implementation for SIFT detector was different than in the classroom; i.e. `cv::SiftFeatureDetector::create()` instead of `cv::xfeatures2d::SIFT::create()`.

## MP.3 Keypoint Removal
Lines 118-133 in MidTermProject_Camera_Student.cpp 

Function `contains()` which checks whether the rectangle contains the point was used to filter out the keypoints beyond the defined rectangle. 

## MP.4 Keypoint Descriptors
Lines 165-174 in MidTermProject_Camera_Student.cpp  
Lines 66-109 in matching2D_Student.cpp

Similar to the keypoints detectors implementation the OpenCV library was used to add a set of descriptors including BRIEF, ORB, FREAK, AKAZE and SIFT. Note that in this implementation the AKAZE descriptor works only with the AKAZE detector.

## MP.5 Descriptor Matching
Lines 194-210 in MidTermProject_Camera_Student.cpp  
Lines 7-45 in matching2D_Student.cpp

The implementation is based on the code from the classroom. To deal with different types of descriptors HOG-based (SIFT) or binary (BRIEF, ORB, FREAK, AKAZE) a control logic was implemented so the L2 norm is chosen when SIFT is used.

## MP.6 Descriptor Distance Ratio
Lines 46-63 in matching2D_Student.cpp

The K-Nearest-Neighbor matching is implemented with the descriptor distance ratio of 0.8.

## MP.7 Performance Evaluation 1

See results in *results/MP7_results.csv*

The evaluation investigated the number of keypoints generated by detectors. From the first test the first three detectors are:

1. BRISK with the average 276.2 keypoint per image.
2. AKAZE with the average 276.2 keypoint per image.
3. FAST with the average 149.1 keypoint per image.

However, in this test, the fast implementation was with the parameters used in the classroom and shown below.
```
int threshold = 30;  
bool bNMS = true;    
cv::FastFeatureDetector::DetectorType type = cv::FastFeatureDetector::TYPE_9_16;
```
Some trial and error was done with the parameters but FAST with default settings produced the best results, which were averaging around 408.2 keypoints per image. The above ranking was then updated with FAST on the top.

## MP.8 Performance Evaluation 2 and MP.9 Performance Evaluation 3
The number of keypoints and the respective times for detectors and descriptors were recorded and saved to the file *results/MP89_results.csv*
Additional results are in *results/MP89_results_new.csv*. This file contains results with the default settings for the FAST detector.

From the aggregated results the FAST detector is a clear winner and its combination with descriptors such BRIEF, ORB. FREAK of SIFT obtained the shortest time in total. It is worth to note that the second test with default settings for FAST has not resulted in a much longer. FAST was still the fastest with an average of 6ms of total time when combinded with the BRIEF detector.

1. FAST+BRIEF
2. FAST+ORB
3. FAST+SIFT

Obviously, more expert parameters tuning may lead to other conclusions but FAST+BRIEF is a good starting combination for the task of detecting keypoints on preceding vehicle.