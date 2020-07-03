# Dance Form Classification
So this was a Deep-learning Image classification Challenge which was hosted on the Hacker Earth. Apart from the classification task, the major challenge was to the deal with the
very less amount of data which was a total of 364 images divided into 8 classes with an average of 45 images per class. [Here](https://www.hackerearth.com/challenges/competitive/hackerearth-deep-learning-challenge-identify-dance-form/machine-learning/identify-the-dance-form-deea77f8/) is the link to the contest.<br>
<br>
Following was my approch to deal with the challenge.

## Idea 
So the data is very less hence the first and only thought that comes to the mind is of the transfer learning with image-augmentation. This combination has been proved to be very effective
in many of the previous Image Classification Competitions. Transfer Learning helps to leverage the basic features learned by the neural-network that have proven to be good in the image-net competition.
The data augmentation help in increasing the data from the existing data, damping the problem of overfitting up to a certain margin.

## Tools Used
Keras - Library to train Deeplearning models<br>
Albumentations - Image Augmentation Library<br>
Matplotlib - Data Visualization<br>


## Visualizing the Data

![Visualization](https://github.com/kevin3010/Machine-Learning-Competition/blob/master/Hacker-Earth/Dance-Form-Classification/images/DataVisualization.PNG)

**Observations**
* It can be observed that most of the images are have black background. This obeservation will make sense when we do the image augmentation.
* The data has human figures( it is obvious :) ) and humans are also one of the class in the imagenet competitions.

## Checking for Imbalance
![imbalance plot](https://github.com/kevin3010/Machine-Learning-Competition/blob/master/Hacker-Earth/Dance-Form-Classification/images/plots.PNG)
<br>
**Observations**
* It can be observed that *manipuri* class has the lowest data. Though the difference is not much compared to the other classes but considering the fact that data is very less 
this differnce may make a difference.I augmented the images from the manipuri class but I cannot confirm wheather it made any changes in the result.

## Transfer-Learning
**Findings on Experimentation**
* **InceptionV3** was the most effective model. After removing the top layer, few more layers were also added to get the final output. This layers
were followed by Dropout after each layer to reduce the problem of overfitting.
* The entire model was trained on top of weights from the imagenet dataset. When the pre-trained model was used as a feature extractor it gave very less f1_score. I guess the reason for this is that all the images are having a person in them hence only using the pre-trained model
as feature extractor would not give significant differentiation for different types of dance forms.
* Low learning rate was very effective because the model is already well trained we only need to fine tune the model.

## Data-Augmentation
I used **Albumentations** Library for the data-augmentation. The main reason for using this library was that it provided a larger number of augmentation and the most important of all was the *CoarseDropout*. The fill value was kept to white color and the reason was that the background was black. Now I cannot confirm this change had any impact on the result as the score was almost similar.
And we know the fact the neural networks don't give the same result all the time. Apart from that, I use one-more augmentation from GitHub that was very much similar to *CoarseDropout* but fills the cutout region with random noise. This Additional 
augmentation was found to increase accuracy.

Following is the visualizaion

![augmentation](https://github.com/kevin3010/Machine-Learning-Competition/blob/master/Hacker-Earth/Dance-Form-Classification/images/data-augmentation.PNG)

## Training

So I must confess that this is very wrong method of training but the reason I choosed it was that I started the competition just 5 days back and there was not much time for
experimentation. So, all I did was the trial and error.
I didn't make the validation set, all I did was trained the model and check for the f1_score online by submitting the output. Though I found that after training the model until it reached 90% training accuracy then increasing the probability of random-cut out was very effective. After increasing the cutout probability I checked for f1_score after every epoch
util I find that the model is overfitting.
