## Neural Network Architecture

#### Colors don't play a signification role

We don't need a lot of color to process or visualize color. Neural networks are color blind, colors are not important for object recognition or object detection, as colors don't tell a lot of information. It's mostly edges, gradients, curves and features that play a key role in identifying the objects.

Images are normally represented with three channel RGB, however we can also have a image represented with a fourth channel i.e alpha channel or depth channel:
* Alpha channel [RGBA] talks about what is the transparency of the pixel on top of which it is. Alpha channel determines how transparent the pixel should be.
for instance consider this red pixel [255, 0, 0 , 0.5], the fourth channel (alpha channel) indicates the transparency level (i.e 50% transparency in this case) 

* Depth channel, is specified as RGBD, where d indicates depth of an image i.e each pixel relates to a distance between the image plane and the corresponding object in rgb image


### What is Receptive Field?

Receptive field is the field of view that would be covered in a image, i.e how much of the pixel in the first image a pixel has seen. If we convolve on 7x7 image 3 times we get 1x1, so the final layer's or channel's pixel would have seen every pixel in the first image. So the receptive field is basically how much area of the image can we see in the last layer. The last layer should have see the entire whole image or most of the image, if that's true then the network can do a much better prediction, in the above case the receptive field is 7x7 

#### Blocks of Neural Network 
There are 4 blocks in a Neural Network, edges and gradients, textures and patterns, parts of objects and objects.
- Since our brain also structured with 4 layers, they wanted to follow the same architecture
- and its proven to be very effective 
Each block will have multiple layers in between, we add layers to improve the receptive fields of the neural network.

Receptive field don't explode it increases slowing, that's the reason we add layers. Every time we add a layer the receptive field increases by two pixels. Receptive field of 1 pixel is always 1, because each pixel knows about itself, so we have to add a lot of layers so that finally we are at the receptive field which is equal to the size of the object/image

### Filters
[Image Filter Playground](https://setosa.io/ev/image-kernels/) can be explored to understand how filters can be used to extract features from images

### Max Pooling

Max Pooling is added to reduce the number of layers. We rarely (rather never) use MaxPooling with 3x3, but rather use 2x2. 

Maxpooling adds invariance (like shift invariance, rotational invariance , scale invariance etc...), invariance basically means not dependent on the variness of the input coming in i.e even if the data changes a bit we get the same results.

### Few things to note:
* Feature is not a channel. 
* Kernel is not visible. 
* Kernels are mostly in odd numbers.
* kernel can be thought of as a function along with the activation function, it is a set of 9 numbers which we use to multiply the neuron and create new kind of neurons after activating them. Kernels have nothing to do with neurons, when we turn the computer off all the channels are going to be destroyed, but the kernels are going to be saved on the computer. Kernel is the weight we are going to train and get better feature detector out of the channels
* Number of channels depends on the number of features we would like to extract
* RGB- we don't see colors. RYB - we see color sensitivity.
* Neuron is a pixel and channel is a collection of all the pixels when our kernel works (convolves) on a particular image and its creates a new channel and let say the size of the channel is 7x7 channel then we have 49 neurons or pixels. This channels is the input to next kernel, the kernel is going to process that and make new neurons (or set of channels)
* A layer is a combination of channels, in a layer we can have many channels. If we had 14 kernels, 14 kernels are going to give 14 channels, the set of these 14 channels is called a layer.
* A Network means all the layers together, basically, anything between input and output is a network, all weights , channels, kernel convolving on image etc 
* Every time we colvolve on 3x3, we loose two pixels on both side. Suppose we have 16 x 17 and we convolve using 3x3 we get 14x15 (loss of two pixels), this is not always true as we don't want to reduce our image to vey small number because we want to be able to identity what is there in the image.
* For images which are of size between 224 and uptill 400, we find that nearly all the edges and gradients can be identified under a receptive field of 11x11
* Never ever decide to initialize any values to zero's, for eg: when we add padding, we don't initialize the padded block with zero, rather we will use the same number as neighboring pixel (expand the pixel to next position) or average of the pixels which are there in the image. So that network is not biased with human decisions.
* While deploying the model, never scale the dataset down or up always use the image as is, whenever we scale the dataset its going to use something called bilinear interpolation and its going to mess up the statistics of the dataset.
* Max pooling should be done as far away from the output


### Important Links:

[Convolution Playground](http://scs.ryerson.ca/~aharley/vis/conv/flat.html) <br/>
[Neural Networks](https://youtu.be/aircAruvnKk)
