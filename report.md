I did a lot of experiment for this project so I try to summarize the experiments and adjustments which led me to final model and data:

-I used the data provided by Udacity.

-Originally I only tried the center images. But the result was not good enough and the car was getting outside the road many times.

-So I thought I need to have more data for the model to learn better. Based on the recommended papers and websites from Udacity, I loaded the side camera images too. Looking at the images on different angles and running the training multiple times, I found that for the left and right cameras, .175 is a good number to be added to or subtracted from the steering angle, which is considered as "diff_angle" in code.

-I tried the full size image 320*160 and resized images of 160*80. For full size images, it obviously took longer time to train the model and I didn't see any meaningful improvement in car driving. So I used the resized images for training.

-Set the validation se data to %20 of the data.

-Preprocessing the data:

The more good data we have, the better. Our data is images from the road. When a car drives by itself in a road, there is no guarantee that when it passes from a point, it will be exactly in the same spot of the road which is was trained. It can be in the middle, more left or more right. So to train the model with more data which can heppen in reality, we can generate more data from our exising data which can simulate real data. The other reason to generate more data is that our data is imbalanced. Looking at the distribution, it is clear we have lots of data for angles arounf 0, .175 and -.175, but much smaller number for other angles.

Studying the recommended text from Udacity, there were many techniques to generate valid data:
.Flipping the image and negating the steering angle.
.Shifting the image to right or left and changing the steering angle a little bit.
.Changing the brightness of the image to simulate different light levels.
.Creating shadows on parts of image to simulate real shadow on road.
.Cropping the image top an d bottom.

I found that flipping and shifting helps the driving gets better. Changing the brightness helped to improve for the second track. I didn't use shadowing because from the tracks I have, none of them has shdowy road to test it.

The model was trained good without cropping image, so I didn't cropt the image. Although in general it could result in smaller images size and shorte processing time.


-Model:
I used the Nvidia CNN architecture for my model. The layers in the model include: normalization, 5 convolutional layers followed by RELUs and 3 fully connected layers followed by RELUs. As I was experimenting and training the model, I added 1 dropout layers as I made sure it doesn't change the model performance on the track to help avoid overfitting. I also added a average pooling layer to the model.

-Training:
Becasue we generate data from our existing data, the size of the data used for training will be very large. So I used generator to train the model. In each iteration, the generator picks random data sample and generates 32 new data points for it and once bacth is ready it will be used to be fed to model for training. I could do it  determinsitically instead of random selection of samples, but wanted to experience how random selection of samples will train the system and itlooks like it does it well.

-At the end. I save the trained model.

-After many times of training and tweaking the parameters of different part of data generationa and model pipelines, I found the sweet spot for both tracks.


