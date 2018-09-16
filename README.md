# Self-Driving Car Engineer Nanodegree

---

## Vehicle Detection Project 

---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/vehicle-non-vehicle.png
[image2]: ./output_images/hog.png
[image3]: /output_images/hog-heat.png
[image4]: /output_images/subsamling.png
[video1]: ./project_video_output.mp4


---

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

First I have placed the training datasets in `./training_images/` which are extracted, if not already `In[1]` of notebook `vehicle_detection.ipynb`. Then they are loaded `In[2]` as `vehicles` and `non-vehicles` images.

The number of vehicle images and non-vehicle images are `8792` and `8968` respectively.

Following are few examples of vehicle and non-vehicle images:

![vehicles-nonvehicles][image1]

The Histogram features extraction functions are `In[5]`. Below is an example of HOG features on a random image from vehicle and non-vehicle images:

![hog][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and following parameters started giving good results:

* Color Space         : `YCrCb`
* HOG Orient          : `8`
* HOG Pixels per cell : `8`
* HOG Cell per block  : `2`
* HOG Channels        : `ALL`
* Spatial bin size    :  `(16, 16)`
* Histogram bins      : `32`
* Histogram range     : `(0, 256)`
* Classifier          : `LinearSVC`
* Scaler              : `StandardScaler`

Above parameters gave `0.9865` accuracy.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

The `LinearSVC` classifier is trained `In[7]` which took around `35 seconds` to train using Color Space of `YCrCb`.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The sliding window approach is implemented `In[9]` and `In[10]` of the notebook. To filter out false positive, I used heatmap as described in Udacity course to filter some out. Following is an example output of sliding window alongwith heatmap filtering:


![hog-heat][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

The performance of the method calculating HOG on each particular window was slow. To improve the processing performance, a HOG sub-sampling was implemented as suggested on Udacity's lectures. The implementation of this method could be found on `In [14]`. The following image shows the results applied to the test images (the same heatmap and threshold procedure was applied as well on `In [15]`):

![subsampling][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
The video output can be found in project directory named `project_video_output.mp4`. Here's a [link to my video result](./project_video_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

In the pipeline, there is heatmap and thresholding methods for filtering but it does not seem enough.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The tuning the parameters to get the right detections was very hard. In the output images and video, there are still a lot of false positives which may cause the self-driving car to just halt or stop alot.

Following are the ways I can think of improving the pipeline:

* Use dynamic parameters for HOG
* Mix-and-match with lane-detection from previous project to help define the overall bounding boxes of potential vehicles
* Use decision-tree to analyze redundancy on the feature vector
* Use Bayesian filters in consecutive frames to predict the presense of vehicles. Also use the simple model of cars (moving in forward direction) to bias the filter.

