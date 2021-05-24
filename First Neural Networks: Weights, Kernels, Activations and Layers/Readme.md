### Max Pooling

- If we want our Neural Network to be as small as possible we add max-pooling as early as possible. However, having said that, we don't use max-pooling without convolving twice or more. Max-Pooling is used to reduce the size of the channel and its always best practice to use max-pooling once most of the work is done or extracted some of the features, i.e. when we have edges and gradients have been created, we apply max-pooling and then apply another set of convolution to identify textures and patterns.
- When we use max-pooling we are basically throwing away the information (pixels)  intentionally at a point where the information is not required
- We also don't add max-pooling towards the output, it should be at least away from 2 or 3 convolution layers from output, because if we drop the pixels closer to output then the last layer will not have any much information to make the prediction.
- We will always not consider the last rows and columns in an odd resolution channel while performing max-pooling i.e if we have 7x7 when we perform max pooling the channel would be of size 3x3 we will ignore the last column.

### CONVOLUTION WITH STRIDES
We normally perform convolution mostly with stride 1 and don't do convolution with stride of 2 or more. As there some disadvantages of using stride of 2 or more:
- size of channel reduces drastically
- due to this it throws away lot of information
- it is not useful where we want to preserve lot of information
- when we convolve with a stride of more than 1, we would be covering some pixels more than once

If we use stride 2, either the pixels are covering once, twice or thrice there is no other number, there is no equal distribution of pixel getting covered. If the central pixel has very high value then its going to dissipate (disperse) the information from central pixel into the nearby or neighboring pixels unevenly also called feature accumulation, and cause the information to be distorted leading the image to be blurred.
Having said that, they are useful if we have a resource constraint hardware and the problem at hand is not complicated, in that case we can reduce the size of the channel and ensure memory is not used a lot.


When we convolve with the standard way (stride of 1), we cover most of the pixels 9 times. Its always good to cover as many number of pixels during convolution.
In 5x5 the total number of pixels covered 9 times is 1x1 (i.e (5-4) x (5-4), 4 because last two rows and columns don't get covered 9 times
|Image size with padding | Image size | Number of pixels covered 9 times | Percentage coverage|
| ------------- | ------------- | -------------| -------------|
|5x5|3x3|1x1|11% (1x1/3/3)|
|7x7|5x5|3x3|36%|
|9x9|7x7|5x5|50%
|11x11|9x9|5x5|60%|
|114x114|112x112|108x108|90% convolution is 9x9 (108x108/112/112)|


### ANT-MAN (1x1 convolution)

- 1x1 is computation less expensive. 
- 1x1 is not even a proper convolution, as we can, instead of convolving each pixel separately, multiply the whole channel with just 1 number
- 1x1 is merging the pre-existing feature extractors, creating new ones, keeping in mind that those features are found together (like edges/gradients which make up an eye)
- 1x1 is performing a weighted sum of the channels, so it can so happen that it decides not to pick a particular feature that defines the background and not a part of the object. This is helpful as this acts like filtering.

### Activation Function

In case of NN the portion which accumulates the data is called a neuron, the activation on top of it is called activated neuron. Activation adds non linearity and decides what information to pass. Non linearity basically means, for any input coming in, there is not a one function that can describes that, its not a simple linear relationship.

If we don't use activation function
- Its not possible to use backpropagation (gradient descent) to train the model—the derivative of the function is a constant and has no relation to the input, X. So it’s not possible to go back and understand which weights in the input neurons can provide a better prediction.

- All layers of the neural network collapse into one—with linear activation functions, no matter how many layers in the neural network, the last layer will be a linear function of the first layer (because a linear combination of linear functions is still a linear function). So a linear activation function turns the neural network into just one layer.

      L1 = w1x1 + w2x2
      L2 = w3(w1x1 + w2x2) + w4 (w1x1 + w2x2)
         = x1(w3w1 + w1w4) + x2(w3w2+w4w2)

 - we dont get benefit of adding more layers and neither do we get any non linearity to learn something complex, so its just going to collapse on to itself, if we make 100 layer NN and don't add any activation its just going to collapse into one single layer. 

Below are few activation functions:
- sigmoid causes vanishing gradient problem (all gradients of the weight becomes zero)
- tanh - also causes same vanishing gradient problem
- ReLu - only positive values are passed through (computationally less expensive)
- leaky ReLu

### Few things to remember:
- Earlier to Convolution NN, we used fast fouriers to do feature engineeing, but the problem was we had to manually identify the number of parameters required, frequency of the rotation etc. But with CNN algorithm could figure this out by itself.
- We mostly use 3x3 kernel, one of the core reason was axis of symmetry, if we use 4x4 we don't get finite details as we have in 3x3.
The alternative way to get 4x4 behavior is we can combine 3x3 with another 3x3 which can then become 5x5, we can then delete (replace last column with zero) to become a 4x4
- We don't want to loose the size of the channel by the virtue of convolution, so we add padding to have the size at least of some specific size
- All the neural network architecture have 224x224 as the stating image, due to legacy so that it can be compared against other architecture and generally in last layer we like to have a size of 7x7 and its pretty consistent nearly in all the neural network architecture we have.
- The kernels that we use for convolution have no relation with the channels that we have in the input image, but these kernels have a relation on the number of channels that we would have in the output image
- Most of the time we double the number of channels in every convolution (i.e 32 -> 64 -> 128 -> 256 -> 512) and mostly 2^n kind of number is followed because its much optimized and GPU's can handle this number much better.
- The number of layers in the neural network depends on the kind of hardware that we have, we should not push the layers to learn more and always let the network learn slowly rather than overloading the memory and putting a pressure on the kernel to extract more features.
- RelU's derivative is 0 when x is less than or equal to zero, 1 when x is positive. ReLu's derivative is not continuous at a particular point but its differentiable.
- Anything we use with kernel size of 3x3 and with a stride of 1, the receptive field will increase by 2.



### Important Links:
[CNN Explainer](https://poloclub.github.io/cnn-explainer/) <br/>
[Feature Visualization](https://distill.pub/2017/feature-visualization/)<br/>
[Computing Receptive Fields of Convolutional Neural Networks](https://distill.pub/2019/computing-receptive-fields/)<br/>
[Deconvolution and Checkerboard](https://distill.pub/2016/deconv-checkerboard/)
