# Convolution background and basics</br>

## Channels and Kernels

<b> Channels </b> are feature bags where each channel can be thought of as a container that provides a distinct information on a particular feature.

Lets say you have been to a animal sanctuary and would like to capture the sounds of an animal seperately for some of your research work, you capture and store the sound of a lion seperately, sound of elephant seperately and so on. Storing the sound of every animal in seperate file (container) is nothing but a channel. Now, the channel for sound of a lion can be further divided into seperate channels based on the frequency, wavelength, speed etc. So the channel of lion sound is in itself going to be collection of multiple channels in between. More the number of channels, it helps better capture the nuances of the sound in every animal. 

<img src="https://user-images.githubusercontent.com/42609155/116164047-a5efb000-a716-11eb-9990-10d6c815f5c6.png" width="500">

<b> Kernels </b> also called as Feature Extractors or Filters or 3x3 matrix are used to identify and extract a particular feature or similar kind of information. 

_Colored digital images are mostly represented using three channels RGB and kernels(Mostly 3*3) can be applied on the images to extract any particular information. Also, we should be aware that only colors don't represent the channels. There are other features like edges, texture and pattern, part of objects which contributes to  particular channel._

Note on edges and gradients - </br>
Basically both edges and gradients are same, edges are nothing but (black, white, black - i.e we see some contrast changing) and gradients are (white, light grey, grey, dark grey, black etc), if we look at the gradient from far its going to be a egde.

## Why should we (nearly) always use 3x3 kernels?
This question can be answered in two parts. One part is to answer why we are not using even kernels (2x2, 4x4) and the second part is why we are not using bigger kernels (5x5, 7x7, 9x9..). 

First, with even kernels the problem is its difficult to find axis of symmetry. Without centre point, it is difficult to depict information in a symmetric way. 

Second, using a higher size kernel increases the computation cost with more number of parameters and also the amount of information or features extracted are considerably lesser (as the dimension of next layer reduces greatly). Using a lower size kernel like 1x1 does not account of features from the neighbouring pixels, 1x1 is used only in cases of reducing the dimensions. 

3x3 is the smallest unit which can be used to compute any kernel size output and seems to be a best fit. If we need 5x5 kernel output, we can convolve with 3x3 twice (3x3 + 3x3 = 18 parameter) and if we need 7x7 output, we can convolve using 3x3 thrice (3*3 + 3*3 + 3*3 = 27 parameters) and so on. And GPUs have accelerated 3x3 operation, so it is much faster to perform the convolution using 3x3 kernel.
Using a single 5x5 and 3x3 twice, doesnot lead to any loss of information they both lead to similar accuracy (might not be exactly same)

## How many times do we need to perform 3x3 convolution operation to reach 1x1 from 199x199 (show calculations) 

Each time, when a 3x3 convolution is performed, we end up with 2 pixels lesser output channel. When we perform 3x3 on 5x5 image, we get a 3x3 image.

<img src="https://user-images.githubusercontent.com/42609155/116170219-01746a80-a724-11eb-8666-6ff98fb8cd19.gif" width="500">

In the above pictue:
- dark purple is a kernel, convolving over light purple image
- green is the channel extracted by the kernel
- each square in the green is a feature
- each square (pixel) in the light purple is a neuron and kernel is storing that information, all neurons (squares) combined together is a channel.

Without Max-pooling, **99 times** 3x3 convolution needs to be performed on 199x199 to reach 1x1 image!


|  Operation-No | Image O/P	|
|---------------|-----------|
|		1		|	197X197	|
|		2		|	195X195	|
|		3		|	193X193	|
|		4		|	191X191	|
|		5		|	189X189	|
|		6		|	187X187	|
|		7		|	185X185	|
|		8		|	183X183	|
|		9		|	181X181	|
|		10		|	179X179	|
|		11		|	177X177	|
|		12		|	175X175	|
|		13		|	173X173	|
|		14		|	171X171	|
|		15		|	169X169	|
|		16		|	167X167	|
|		17		|	165X165	|
|		18		|	163X163	|
|		19		|	161X161	|
|		20		|	159X159	|
|		21		|	157X157	|
|		22		|	155X155	|
|		23		|	153X153	|
|		24		|	151X151	|
|		25		|	149X149	|
|		26		|	147X147	|
|		27		|	145X145	|
|		28		|	143X143	|
|		29		|	141X141	|
|		30		|	139X139	|
|		31		|	137X137	|
|		32		|	135X135	|
|		33		|	133X133	|
|		34		|	131X131	|
|		35		|	129X129	|
|		36		|	127X127	|
|		37		|	125X125	|
|		38		|	123X123	|
|		39		|	121X121	|
|		40		|	119X119	|
|		41		|	117X117	|
|		42		|	115X115	|
|		43		|	113X113	|
|		44		|	111X111	|
|		45		|	109X109	|
|		46		|	107X107	|
|		47		|	105X105	|
|		48		|	103X103	|
|		49		|	101X101	|
|		50		|	99X99	|
|		51		|	97X97	|
|		52		|	95X95	|
|		53		|	93X93	|
|		54		|	91X91	|
|		55		|	89X89	|
|		56		|	87X87	|
|		57		|	85X85	|
|		58		|	83X83	|
|		59		|	81X81	|
|		60		|	79X79	|
|		61		|	77X77	|
|		62		|	75X75	|
|		63		|	73X73	|
|		64		|	71X71	|
|		65		|	69X69	|
|		66		|	67X67	|
|		67		|	65X65	|
|		68		|	63X63	|
|		69		|	61X61	|
|		70		|	59X59	|
|		71		|	57X57	|
|		72		|	55X55	|
|		73		|	53X53	|
|		74		|	51X51	|
|		75		|	49X49	|
|		76		|	47X47	|
|		77		|	45X45	|
|		78		|	43X43	|
|		79		|	41X41	|
|		80		|	39X39	|
|		81		|	37X37	|
|		82		|	35X35	|
|		83		|	33X33	|
|		84		|	31X31	|
|		85		|	29X29	|
|		86		|	27X27	|
|		87		|	25X25	|
|		88		|	23X23	|
|		89		|	21X21	|
|		90		|	19X19	|
|		91		|	17X17	|
|		92		|	15X15	|
|		93		|	13X13	|
|		94		|	11X11	|
|		95		|	9X9	    |
|		96		|	7X7	    |
|		97		|	5X5	    |
|		98		|	3X3	    |
|		99		|	1X1	    |
				
### How are kernels initialized? 

Kernels or Filter or Feature Extractors are used to extract the features from an image and store the data in neurons.Initializing the Kernel's weights in the begining of a tranining process is a random choice. Kernel weights can be anywhere between 0 and 1 or -1 and 1 depending on the activation function chosen across the NN layers. There are variety of methods including simple ones like zero initialization , random initialization and Gaussian. If we assign zero as initial weights,then neurons across all the layers tend to get stuck during the training process and if the neurons start with the same weights, then all the neurons will follow the same gradient, and will always end up doing the same thing as one another.

However, its always recommended to use some more advanced techniques like He initialization, Xavier initialization/Glorot initialization so that the weights in the network dont start too small or too large leading to vanishing or exploding of gradients.

Kernels can also be initialised from the weights of another network. This is popularly called transfer learning and is used successfully for better and faster convergence of many problems.

### What happens during the training of a DNN?</br>

A deep neural network (DNN) is an artificial neural network (ANN) with multiple hidden layers between the input and output layers. The inspiration behind the creation of Deep Neural Networks is the human brain. 

Training a DNN is the procedure of adjusting the weight/values of the kernel.
Given an input, all the layers nodes effectively constitute a transformation of this input to a predicted output. The measure of variation between this predicted output and the actual output is defined as loss. The value of this loss is then passed backwards through these filters (kernels) and used to adjust the values in the filters to effectively minimize the difference between predicted and actual output. This way the value (weights) of the filters are adjusted during training and system is said to have converged when the loss is minimized.
