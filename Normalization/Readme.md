# Normalization and Regularization

#### What causes overfitting?

Imagine a scenario, where we have general kernels which help us identify 40k kind of images, then further down the line we have a kernel which help identify only 1k kind of images. When we need more number of kernels to identify less number of images, that causes overfitting because one kernel job is to learn one image or 1 kind of image, thats were overfitting is.

Model overfits because we have few specific kernels which are focusing on specific features which are found in the images. Suppose have already used the capacity of the model to detect all the taining images, now the training accuacy is high. If there was a image which model was not able to identify because the model was not able to figure out what are the building blocks of the image or divide the image back into the features, so NN decided to make a kernel and just focus on that image, pick anything in that image and make sure whenever that thing is there we can actually just predict the class.


Adding BN helps reduce overfit, because it changes the images. Lets say we have two images , black cat and white cat. Now there is a kernel identifying a feature in the black cat whose amplitude is going to be low and the same kernel going trough a white cat amplitude is going to be high. All the kernel wanted to say is I have found a ear but for the black cat should the amplitude be lower than the white cat? No. This is where batch norm is going to come in,its going to say the channel will have that amplitude irrespoctive of the fact that whether its going to come from black cat or white cat, that is what we are normalizing. The feature that goes out which says this is the ear of a cat is going to have similar amplitude for black cat and white cat. It reduces overfitting because black cat sent is not black any more, other wise the kernel would not have given us that higher amplitude because of BN we have created a grey cat automatically.


### Image normalization
In image processing, normalization is a process that changes the range of pixel intensity values. When we normalize we are changing our scale from one system to another. when our data is distributed in small domain the kernels do not have enough feature distribution wrt amplitude to be able to find that particular feature and when we stretch our range we get the (normalization back) amplitude changes back and it becomes easier for the kernel to learn.

Normalization is not Eualization, whenever we take photographs by iphone and other modern phone, it trys to readjust the color itself thats called equalization. We never equalize our image when sending to NN.

#### Why do we normalize? 

If we have a scale of 50 to 180, we first subtract by 50 (minimum value), 50 - 50 and 180- 50 and then multiply by 255/50, that is how we do normalization.
When we normlize the data in normal distribution with zero mean, most of the pixel intensities are going to be close to zero without loosing the data, weights are also close to zero because we pick our weights from normal distibution of + and -1, which means when we are multiplying the values there is a gaurantee that the weights are not going to explode, the activations are not going to explode since most of the pixels are close to zero. This ensures that the data is kept between a range of -1 and +1

### Batch Normalization
Batch normalization is dependent on number of channels . Batch Norm, solves the problem called internal covariate shift. Covariate means input feature. Covariate shift means that the distribution of the features is different in different parts of the training/test data. Basically, the amplitudes of the features are different. Internal Covariate shift refers to changes within the neural network,between layers. A kernel always giving out higher activation makes next layer kernels always expect this higher activation and so on.

In case of BN, for each of the channel we have 1 mu each, so for 4 channels we have 4 mu's. Mu is nothing but the average of channel1 from img1 + channel1 from img2 + channel1 from img3 respectively. Then, we have 4 sigmaa's and this is calculated using the variance for each channel.

### Layer Normalization

Layer Normalization is for one image we would be normalizing all the channels in a particular layer. In LN all the channels of the layer is been averaged but for 1 image only, so the total number of mu's are dependent on the images we have.

### Group Normalization

Group Normalization is something in between BN and LN, in GN we would be normalizing the data or values among some of the channels. Lets say, we have a layer with 10 channels and if we make a group of two, then we would divide this 10 channels and divide into 5 & 5. Then we would be doing layer normalization among these 5.

## Regularization

Lets say we have a image of a dog, and we want to delete the ears of the dog in the final image how do we do it in while training the model without using any augmentation techniques?
- Use Drop out to drop the ear pixels
- Add some noise to the network, so that the newtork ignores the ear pixels
- Delete the channel completely

When training a model we need to do as many augmentations as possible, one way is do image augmentataion however we can also do a lot of augmentations within in the channels, like dropping a pixel, cutout in the channel, drop the whole channel etc.

Lets say we have a loss, L
- L1 loss says the sum of all the amplitude (absolute value) of the weights should be equal to zero
- L2 says the sum of all square of the weights should be equal to zero

Lt = L + L1, if we have two loss, the first loss says that the oveall accuacy of the model should be very hight and the L1 says sum of all the weights should be equal to zero, there will be some compromise between these, i.e some of the weights would be zero and batch propogation is going to decide that, the weights those contribute least would be zero. i.e we will start loosing the kernel which are no cooperating or not identifying lot of images, we are going to loose all the specialist kind of kernels, these specialist kernels are those which actually overfit the models because they are specialised in indentifying only one thing.


### Few Points:
- increasing number of kernels/parameters, increasing number of layers, changing the optimizer or learning rate will not help resolve the overfitting issue, learning rate changes the way we update the weight for the images we have sent, changing learning rate when the model is already ovefit will make the model overfit faster
- We should never be adding Relu, dropout or regularization to the last layer. Last layer knows what is not there and what is there in the image, for us to take a call we should know both what is not there and what is there. If we are predicting a dog, the model needs to know that cat or other animals are not there.The moment we add BN, we would be manipulating the amplitude of the weights and we may miss to quote something. Similar is for the drop out also, if we add drop out to last layer there is a possibility that drop out could make class that we are supposed to predict to zero. Also, if we add a relu, then all the negatives which were coming out and telling these classes are not there, then suddenly you are saying nothing about them.
- We should never send a small batch size to a big model, imagine feeding a monster with a small baby spoon. The reason is simple, if you have a model with huge capacity it can learn a lot and it should be sent the data where a lot of variance is there otherwise, all the weights are going to be focusing on one part of dataset. 
