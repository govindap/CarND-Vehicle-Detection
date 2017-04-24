
**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png
[image2]: ./examples/HOG_example.jpg
[image3]: ./examples/sliding_windows.jpg
[image4]: ./examples/sliding_window.jpg
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the get_hog_features() function in the VehicleDetectionProject5.ipynb IPython notebook 

I started by reading in all the `vehicle` and `non-vehicle` images.  

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Notebook shows example using the `YCrCb` color space and HOG parameters of `orientations=32`, `pixels_per_cell=(16, 16)` and `cells_per_block=(2, 2)`:




####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and settled on below values, I choose those values based on the classifier accuracy in identifying the images in LinearSVC kernel.

The following values gave best performance.
color_space = 'YCrCb'
orient = 32
pix_per_cell = 16
cell_per_block = 2
hog_channel = 'ALL' 

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using Hog, spatial color features and histogram bin features

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search in the following x_y_scale -- y_start_stop = [200, 600], x_start_stop = [750, 1200]

Following window sizes are used for search -- (64, 64), (96, 96), (116, 116), (128, 128) with xy_overlap of (0.7,0.7)


####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to try to minimize false positives and reliably detect cars?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Examples are displayed in notebook.

---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./box_project_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I tried to find best heat map thresholds based on the test images.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Example result showing the heatmap for the test images and the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the test images in the notebook


---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The pipeline is not very accurate in vehicle detection because of the classifier technique used, deep learning will improve the accuracy significantly better. I spent lot of time finding the right feature combination for the best SVC classification accuracy. It's not very robust to positive identification when the car is visible partially or when there are trees, shadows or visibility issues along the way.' It can be made more robust by trying different classifiers and improving the accuracy on the test set. Finding the right threshold to improve detection and filter out noise took lot of iterations. 

