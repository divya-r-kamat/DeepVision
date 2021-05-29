# Architectural Basics for Neural Network

Below are few architectural basics that we need to know when building a Neural Network and designing CNN:

* How many layers: 
In general we design layers at least to map the receptive field of the image. But modern networks go further deep and go beyond receptive field to capture more information about the images. 

* MaxPooling:
  - we use it when we want to reduce the channel dimensions i.e if we want to make the channel smaller
  - we should not be using them close to each other we need to have some seperation
  - they should be used when a block has finished the work i.e edges&gradient, textures and patterns, part of objects have been made (normally, it would be at 11x11 and for small images it would be 7x7)
  - they should be as far as possible from the last layer.
  - nn.Maxpool2d()

* 1x1 Convolutions:
  - 1x1 is merges the pre-existing feature extractors, creating new ones, keeping in mind that those features are found together (like edges/gradients which make up an eye)
  - with 1x1 total computation requirement is less, total amount of RAM required is less and it can learn fast, because of lesser number of parameters
  - 1x1 reduces the burden of channel selection on 3x3.
      
    Suppose we have we have a channel of 3x3x128x256 and lets assume, the next kernel after this is going to be 3x3x256x512. Lets look at one single kernel 3x3x256 and imagine this kernel was supposed to work for cat. All of these 256 in this will not be for cat only. Few, say 4 of this will be for cat and the rest 252 will not be for the cat. But the 3x3 still has to maintain these "not for the cat" kernels, because it has to talk to multiply with those channels (with zero or negative value) to say that its not interested. So there is additional burden on 3x3 and has to do two task
    - it has to select the channel to work on 
    - then it has to find its own kernel to work and extract features from that particular channel 

So there is double work on 3x3. What if, we can extact only these 4 channel for cat combine them into 1 channel and then give to 3x3, then 3x3 will require to have only 1 channel. Here we can use 1x1 to extract the important channels make less 3x3's so that the total computation requirement is less, total amount of RAM required is less and it can learn fast, because of lesser number of parameters


* 3x3 Convolutions:
  - 3x3 convolutions help extract the features from the image

* Receptive Field:
  - Receptive field is the field of view that would be covered in a image, i.e how much of the pixel in the first image a pixel has seen. If we convolve on 7x7 image 3 times we get 1x1, so the final layer's or channel's pixel would have seen every pixel in the first image. So the receptive field is basically how much area of the image can we see in the last layer. The last layer should have see the entire whole image or most of the image, if that's true then the network can do a much better prediction, in the above case the receptive field is 7x7

* SoftMax,
  - Softmax is not probability, its just the scaled up number from the numbers which are predicted by the neural network.
  - We should always look at the numbers that are sent to the softmax i.e the amplitude of the values going to softmax and not the softmax output

Negative Log Loss
  - Negative Log Loss is negative log of softmax value of the correct class. By this softmax is forced to make sure one class has a highest value.

* Learning Rate,
  - small learning rate the loss is going to drop very slowly and takes lot of time to converge (0.001)
  - if its too high it never converges
  - so we should be choosing optimal learning rate
  - we will use SGD learning rate of 0.01 and momentum

* Kernels and how do we decide the number of kernels?
  - We mostly use 3x3 kernel, one of the core reason was axis of symmetry, if we use 4x4 we don't get finite details as we have in 3x3. The alternative way to get 4x4 behavior is we can combine 3x3 with another 3x3 which can then become 5x5, we can then delete (replace last column with zero) to become a 4x4
  - The kernels that we use for convolution have no relation with the channels that we have in the input image, but these kernels have a relation on the number of channels that we would have in the output image
  - Most of the time we double the number of channels in every convolution (i.e 32 -> 64 -> 128 -> 256 -> 512) and mostly 2^n kind of number is followed because its much optimized and GPU's can handle this number much better.

* Batch Normalization,
  - they are really great!
  - it helps make all the features prominent and speak boldly, so that the next kernel can look at the feature and find easily.
  - it going to make sure irrespective of background, feature quality and amplitude of the feature colors, it would gives similar output doesn't matter what. 
  - used after every layer
  - its should never be used before last layer.
  - nn.BatchNorm2d()


* Image Normalization,
* Position of MaxPooling,

* Concept of Transition Layers,
  - Transition layer is 1x1 and max pooling together

* Position of Transition Layer,
  - Its used right after the block when a block has finished the work of making edges&gradient ,  textures and patterns, part of objects have been made  

* Global Average Pooling (Global here indicates the whole channel)
  - GAP would be sum of all pixels / divided by average (Eg. imagine a 5 x5, GAP would be sum of all pixels / divided by 25  (which is the Average))
  - We always use GAP to convert 2d to 1d image 
  - We convert 2d to 1d using GAP at the point where we think all the features are extracted and each of the channels representing a object or a big chunk of an object which when combined can talk about a particular object.

* DropOut
  - drop out is a kind of regularization (i.e the network will be able to work with the unseen images i.e images which it has never been trained on)
  - we will be using this after every layer
  - we are always going to be using drop out with small values, like 5% or 6%. 20% or more will not be helpful.
  - its should never be used before last layer (we don't use anything (maxpool, batchnorm etc) before the last layer)
  - nn.Dropout2d()

* When do we introduce DropOut, or when do we know we have some overfitting
  - we should not be adding drop out till we see a need for it, we add drop out when we have exhausted all the possibilities of making our test accuracy closer to training accuracy i.e when we see training accuracy at high number and test accuracy at a lower number, that is when we need to introduce drop out to reduce the gap, in this process its okay for dropout to reduce the training accuracy so as to reduce the gap and get it closer to test accuracy.

* The distance of MaxPooling from Prediction,
  - Max pooling should never be used before last layer.

* The distance of Batch Normalization from Prediction,
  - Batch normalization should never be used before last layer.

* When do we stop convolutions and go ahead with a larger kernel or some other alternative (which we have not yet covered)

* Why Fully connected layer should not be used to convert 2d data into 1d data?
  - By doing that we are destroying the data about 2d by using 1d so there is nothing that we can extract about 2d, while converting to 1d if the image moves slightly or changes shape, the pattern would change completely and fully connected layers cannot represent that perfectly that is why we do not have FC layer in convolution NN kind of layer or image based network.
  - Never use FC to convert 2d into 1d, convert that using some technique without using FC layers and after that 1d conversion use FC layers for some advancements.

* Why do we not use 1x1 to increase the number of channels?
  - We do use 1x1 to increase the number of channels but we need to have a purpose for that. 1x1 will be used to increase the number of channels, if and only if it was used to reduce the number of channels or if there was something else which was used to reduce the number of channels.
  - One thing to note is when we want to expand using 1x1 its going to reuse the component which are there and just make new channels, it will not be able to increase the overall number of features available, it can just merge them and mix them and create new combinations. Creating new features tasks always goes to 3x3

* How do we know our network is not going well, comparatively, very early
  - if the neural network is not going good for the first 5 epoch then it indicates that the network is not going well, we should not assume it going to magically pickup

* Batch Size, and effects of batch size
  - batch size is of less benefit as it does not give any significant improvement in the validation accuracy by increasing the batch size.
  - the only effect is on the training speed, if we can fill the gpu upto the brim then the network will be able to learn faster as it will be able to process more images
  - normally used with data loader.
