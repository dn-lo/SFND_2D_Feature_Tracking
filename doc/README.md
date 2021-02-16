# Mid-term project report: 2D feature tracking

## MP.1 Data Buffer Optimization

The data buffer is defined as a vector of DataFrames.

When a new frame is loaded, it is pushed at the end of the buffer vector.

If the buffer is full (size larger than *dataBufferSize*), the first element of the vector is erased using the *erase* method. This shifts the other element to the left while preserving the size of the vector.

## MP.2 Keypoint Detection

The detector is selected using the *detectorType* string variable:
- Harris is implemented by computing the Harris response function with the openCV *cornerHarris* class, then normalizing output to 8 bit with convertScaleAbs. Finally, non-maximum suppression (NMS) is used to suppress keypoint candidates which are in the neighbourhood of a keypoint with a higher response.
- FAST, BRISK, ORB, AKAZE, SIFT are all implemented using the corresponding openCV class with default values

## MP.3 Keypoint Removal

A rectangle around the vehicle is defined with the openCV *Rect* class.

Then we erase all elements from the *keypoints* vector which are not contained in the rectangle.

We check whether a keypoint is in the rectangle using the *contains* method of the *Rect* class. This is implemented in a lambda function to check the condition using the function *remove_if*.

## MP.4 Keypoint Descriptors

The descriptor is selected using the *descriptorType* string variable:
- BRIEF, ORB, FREAK, AKAZE, SIFT are all implemented using the corresponding openCV class with default values

## MP.5 Descriptors Matching

FLANN is implemented using the openCV *DescriptorMatcher::FLANNBASED* type with the *DescriptorMatcher::create* method.

To use FLANN, first the binary descriptors have to be converted to floating point rype due to a bug in the current openCV implementation.

## MP.6 Descriptors Distance Ratio

KNN match selection is implemented using the *knnMatch* of the chosen matcher, choosing 2 nearest neighbors.

Then, for each pair of matches, it is checked whether the distance of the best match is lower than 0.8 times the distance of the second best match. If so, the best match is added to the *matches* vector, otherwise the match is discarded.

## MP.7 Performance Evaluation 1

The number of keypoints detected on the preceding vehicle for each detector is indicated in the Open Document Spreadsheet "sheets/performanceEval.ods", in the sheet called "MP.7".

The images containing the detected keypoints on the first frame for each detector can be found in the folder "kptsImg".

It is evident from the images and the spreadsheet that the Harris detector is not able to detect a large enough amount of keypoints on the preceding car to perform the tracking task adequately.

All the other detectors extract a sufficient amount of keypoints.
## MP.8 Performance Evaluation 2

The number of matched keypoints between one image and the next for each detector/descriptor pair is indicated in the Open Document Spreadsheet "sheets/performanceEval.ods", in the sheet called "MP.8".

Again we see that the Harris detector leads to a low number of matched keypoints.

## MP.9 Performance Evaluation 3

The time required for detector and descriptor extraction for detector/descriptor pair is indicated in the Open Document Spreadsheet "sheets/performanceEval.ods", in the sheet called "MP.9".

According to the average time required for the detection + description task, and excluding detectors that were found to be not so performing in MP.7 (BRISK, ORB, Harris), the TOP 3 detector/descriptor pairs are:
1 FAST detector + BRIEF descriptor          (1.35 ms)
2 FAST detector + ORB descriptor            (3.12 ms)
3 ORB detector + BRIEF descriptor           (5.49 ms)

The three pairs were tested and all the keypoint matches were found to be satisfactory to track the preceding vehicle in 2D


