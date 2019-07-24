#**Traffic Sign Recognition** 

Train and test a traffic sign recognition algorithm.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./training_image_examples.png "Visualization"
[image4]: ./test_images/stop.jpg "Stop Sign"
[image5]: ./test_images/speed30.jpg "Speed (30kph)"
[image6]: ./test_images/speed60.jpg "Speed (60 kph)"
[image7]: ./test_images/noentry.jpg "No Entry"
[image8]: ./test_images/turnrightahead.jpg "Right Turn Ahead"

## Rubric Points
Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Data Set Summary & Exploration

#### Data Set Summary
I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32x32
* The number of unique classes/labels in the data set is 43

#### Data Set Visualization

Here is an exploratory visualization of the data set. It picks a few random images from the training set and displays it along with the respective label. The label is looked up by reading the CSV file with label names into a pandas data frame. This gives good insight into how the images appear.

![alt text][image1]

### Design and Test a Model Architecture

#### Pre-processing
The data pre-processing here is simple. The original data is in uint8 and lies between 0 and 255. I simply normalize the image so that it lies in the range -1 to 1 and convert it to float. Normalizing data around the mean and scaling it to be between -1 and 1 is crucial to allowing the model to work with numbers that don't lead to numerical instabilities. The normalization was wrapped into a function for convenience. 


Additionally the data was shuffled. This is necessary because training samples arranged by labels could prevent the model from learning, because successive epochs will be overfitting to a particular traffic sign, followed by another one and so on.


#### Model Description

My final model consisted of the following layers:

| Layer         			|     Description	        					| 
|:-------------------------:|:---------------------------------------------:| 
| Input         			| 32x32x3 RGB image   							| 
| Convolution 5x5	 		| 1x1 stride, valid padding, outputs 28x28x16 	|
| RELU						|												|
| Max pooling				| 2x2 stride,  outputs 14x14x16 				|
| Convolution 5x5			| 1x1 stride, valid padding, outputs 10x10x32	|
| RELU						|												|
| Max pooling				| 2x2 stride,  outputs 5x5x32 					|
| Fully connected			| 800 to 240									|
| Softmax					| etc.        									|
| RELU						|												|
| Fully connected			| 240 to 100									|
| RELU						|												|
| Fully connected			| 100 to 43										|
| 							|												|
 


#### Model Training
The model was trained using a batch size of 64, for 20 epochs. Because I was running this on my desktop CPU, I used a small batch size. The standard learning rate of 0.001 sufficed, and the Adam Optimizer was used.


#### Accuracy
The model achieved a final validation accuracy of 95%. This was achieved after trying a few modified architectures. I started with the architecture from the LeNet lab, and followed it up with some modifications. Finally adding another few layers, and adjusting the kernel sizes and number of filters lead to the improved accuracy.

 
### Test Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] 
![alt text][image7] ![alt text][image8]

These images required an additional preprocessing step, i.e. resizing the images to 32x32. This was accomplished using OpenCV. The trained network made the following predictions:

| Actual Sign Names    | Predicted Sign Names   | Probability
|:--------------------:|:----------------------:|:---------------:|
| Stop                 | Right-of-way           | 0.64            |
| Speed limit (30km/h) | No passing             | 0.77            |
| Speed limit (60km/h) | Speed limit (60 km/h)  | 1               |
| No entry             | Go straight or left    | 0.99            |
| Turn right ahead     | Turn right ahead       | 0.95            |


The model was able to correctly guess 2 of the 5 traffic signs, which gives an accuracy of 40%. This hints that the model does not generalize well enough to images "in the wild".

Among the images the model failed at, 2 got low probabilities (under 0.8). Let us look at the top 5 softmax probabilities for a sample image.
For the third image, the model is very sure that this is a speed limit (60 km/h) sign (probability of 1), and the image does contain this sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Speed Limit (60 km/h)   						| 
| 0.0     				| Speed Limit (50 km/h)							|
| 0.0					| Speed limit (80km/h)							|
| 0.0	      			| Slippery road					 				|
| 0.0				    | Dangerous curve to the left					|
 




