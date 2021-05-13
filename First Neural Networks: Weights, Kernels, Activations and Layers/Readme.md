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


When we convolve with the standard way (stride of 1), we cover most of the pixels 9 times. Its always good to cover as many number of pixels during convolution
|Image size  | Image size With Padding | Number of pixels covered 9 times | Percentage coverage|
| ------------- | ------------- | -------------| -------------|
|5x5|7x7|1x1|11% (1x1/5/5)|
|7x7|9x9|3x3|36%|

### Few things to remember:
- We mostly use 3x3 kernel, one of the core reason was axis of symmetry, if we use 4x4 we don't get finite details as we have in 3x3.
The alternative way to get 4x4 behavior is we can combine 3x3 with another 3x3 which can then become 5x5, we can then delete (replace last column with zero) to become a 4x4
- We don't want to loose the size of the channel by the virtue of convolution, so we add padding to have the size at least of some specific size
- All the neural network architecture have 224x224 as the stating image, due to legacy so that it can be compared against other architecture and generally in last layer we like to have a size of 7x7 and its pretty consistent nearly in all the neural network architecture we have.
- The kernels that we use for convolution have no relation with the channels that we have in the input image, but these kernels have a relation on the number of channels that we would have in the output image
- Most of the time we double the number of channels in every convolution (i.e 32 -> 64 -> 128 -> 256 -> 512) and mostly 2^n kind of number is followed because its much optimized and GPU's can handle this number much better.
- The number of layers in the neural network depends on the kind of hardware that we have, we should not push the layers to learn more and always let the network learn slowly rather than overloading the memory and putting a pressure on the kernel to extract more features.




### Important Links:
[CNN Explainer](https://poloclub.github.io/cnn-explainer/) <br/>
[Feature Visualization](https://distill.pub/2017/feature-visualization/)<br/>
[Computing Receptive Fields of Convolutional Neural Networks](https://distill.pub/2019/computing-receptive-fields/)<br/>
[Deconvolution and Checkerboard](https://distill.pub/2016/deconv-checkerboard/)
