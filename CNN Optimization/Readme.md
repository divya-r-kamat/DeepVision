## CNN Optimization

Using the MNIST image dataset and CNN, follow a step by step approach to finetune the network to achieve 99.4% validation accuracy with less than *10k Parameters* in 15 Epochs.


## Step1 : Basic Setup

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_BasicSetup_Step1.ipynb)

### Target:

- Get the set-up right
- Set Transforms
- Set Data Loader
- Set Basic Working Code
- Set Basic Training  & Test Loop

### Results:
- Parameters: 6.3M
- Best Training Accuracy: 99.93
- Best Test Accuracy: 99.28

### Analysis:
- Extremely Heavy Model for such a problem
- Model is over-fitting because the training accuracy is 99.93, but we are changing our model in the next step

## Step2 : Basic Skeleton

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_BasicSkeleton_Step2.ipynb)

### Target:

- Get the basic skeleton interms of convolution and placement of transition blocks (max pooling, 1x1's)
- Reduce the number of parameters as low as possible

### Results:
- Parameters: 8k
- Best Training Accuracy: 99.04
- Best Test Accuracy: 98.94%

### Analysis:
- We have structured our model in a readable way
- The model is lighter with less number of parameters 
- Its a good model, the overfitting gap is less and the model has capability to learn.  
- Next, we will be tweaking this model further and increase the capacity to push it more towards the desired accuracy.

## Step3 : Add BatchNormalization

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_BatchNorm_Step3.ipynb)

### Target:

- Add Batch-norm to increase model efficiency.

### Results:
- Parameters: 8328
- Best Training Accuracy: 99.71
- Best Test Accuracy: 99.17

### Analysis:
- There is slight increase in the number of parameters, as batch norm stores a specific mean and std deviation for each layer  
- We still see over-fitting 
- We need to add some regularization technique to reduce overfitting and also increase the model capacity, so that model can learn those additional features in test images.

## Step4 : Add GAP Layer

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_GAP_Step4.ipynb)

### Target:

- Add GAP and remove the last BIG kernel.

### Results:
- Parameters: 4709
- Best Training Accuracy: 99.23
- Best Test Accuracy: 99.15

### Analysis:
- With GAP the number of parameters have decreased
- The performace is slighly reduced compared to previous models. Since we have reduced model capacity, this is expected.


## Step 5: Add Regularisation

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_Regularization_Step5.ipynb)

### Target:

- Add Regularization Dropout to each layer except last layer

### Results:
- Parameters: 4709
- Best Training Accuracy: 98.35
- Best Test Accuracy: 98.98

### Analysis:
- There is no overfitting at all. With dropout training will be harder, because we are droping the pixels randomly.
- The performance has droppped, we can further improve it. 
- We can possibly increase the capacity of the model by adding a layer after GAP! 

## Step 6 : Increase Capacity

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_IncreaseCapacity_Step6.ipynb)

### Target:

- Increase model capacity at the end (add layer after GAP)

### Results:
- Parameters: 6,124
- Best Training Accuracy: 99.07
- Best Test Accuracy: 99.26

### Analysis:
- The model parameters have increased
- There is no overfitting rather slight underfitting, thats fine dropout is doing its work , because we are adding dropout at each layer the model is able to capture the training accuracy
- However, we haven't reached 99.4 accuracy yet.
- Observing the missclassified images its good to try out some augmentation techniques as few images seems to be slightly rotated, and also image contrast needs to be considered

## Step 7 : Image Augmentation

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_Augmentation_Step7.ipynb)

### Target:

- Add various Image augmentation techniques, image rotation, randomaffine, colorjitter .

### Results:
- Parameters: 6124
- Best Training Accuracy: 97.61
- Best Test Accuracy: 99.24%

### Analysis:
- he model is under-fitting, that should be ok as we know we have made our train data harder. 
- However, we haven't reached 99.4 accuracy yet.
- The model seems to be stuck at 99.2% accuracy, seems like the model needs some additional capacity towards the end.

## Step 8 : LR Scheduler

[Link to Notebook](https://github.com/divya-r-kamat/DeepVision/blob/main/CNN%20Optimization/MNIST_LRScheduler_Step8.ipynb)

### Target:

- Add some capacity towards the end after GAP layer and add LR Scheduler
- Removed Relu and BatchNorm , drop out from the transition block

### Results:
- Parameters: 6720
- Best Training Accuracy: 99.43
- Best Test Accuracy: 99.53

### Analysis:

- The model parameters have increased
- The model is under-fitting. This is fine, as we know we have made our train data harder.  
- LR Scheduler and the additional capacity after GAP helped getting to the desired target 99.4, Onecyclic LR is being used, this seemed to perform better than StepLR to achieve consistent accuracy in last few layers

### Training Log for final step

      EPOCH: 1
      Loss=0.32070186734199524 Batch_id=468 Accuracy=66.93: 100%|██████████| 469/469 [00:53<00:00,  8.85it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.1617, Accuracy: 9634/10000 (96.34%)

      EPOCH: 2
      Loss=0.23211365938186646 Batch_id=468 Accuracy=94.22: 100%|██████████| 469/469 [00:53<00:00,  8.75it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0673, Accuracy: 9802/10000 (98.02%)

      EPOCH: 3
      Loss=0.22090421617031097 Batch_id=468 Accuracy=95.92: 100%|██████████| 469/469 [00:53<00:00,  8.69it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0455, Accuracy: 9844/10000 (98.44%)

      EPOCH: 4
      Loss=0.05350199341773987 Batch_id=468 Accuracy=96.72: 100%|██████████| 469/469 [00:53<00:00,  8.73it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0350, Accuracy: 9895/10000 (98.95%)

      EPOCH: 5
      Loss=0.05736066773533821 Batch_id=468 Accuracy=97.06: 100%|██████████| 469/469 [00:53<00:00,  8.72it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0297, Accuracy: 9906/10000 (99.06%)

      EPOCH: 6
      Loss=0.056373003870248795 Batch_id=468 Accuracy=97.32: 100%|██████████| 469/469 [00:53<00:00,  8.72it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0252, Accuracy: 9924/10000 (99.24%)

      EPOCH: 7
      Loss=0.11534460633993149 Batch_id=468 Accuracy=97.50: 100%|██████████| 469/469 [00:53<00:00,  8.70it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0246, Accuracy: 9923/10000 (99.23%)

      EPOCH: 8
      Loss=0.04017015919089317 Batch_id=468 Accuracy=97.66: 100%|██████████| 469/469 [00:54<00:00,  8.65it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0219, Accuracy: 9936/10000 (99.36%)

      EPOCH: 9
      Loss=0.018773594871163368 Batch_id=468 Accuracy=97.77: 100%|██████████| 469/469 [00:54<00:00,  8.64it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0219, Accuracy: 9929/10000 (99.29%)

      EPOCH: 10
      Loss=0.05798463150858879 Batch_id=468 Accuracy=97.95: 100%|██████████| 469/469 [00:53<00:00,  8.73it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0169, Accuracy: 9953/10000 (99.53%)

      EPOCH: 11
      Loss=0.020612243562936783 Batch_id=468 Accuracy=98.03: 100%|██████████| 469/469 [00:54<00:00,  8.67it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0175, Accuracy: 9949/10000 (99.49%)

      EPOCH: 12
      Loss=0.02381170354783535 Batch_id=468 Accuracy=98.18: 100%|██████████| 469/469 [00:53<00:00,  8.71it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0195, Accuracy: 9946/10000 (99.46%)

      EPOCH: 13
      Loss=0.10838382691144943 Batch_id=468 Accuracy=98.31: 100%|██████████| 469/469 [00:53<00:00,  8.72it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0164, Accuracy: 9953/10000 (99.53%)

      EPOCH: 14
      Loss=0.1120440661907196 Batch_id=468 Accuracy=98.29: 100%|██████████| 469/469 [00:53<00:00,  8.72it/s]
        0%|          | 0/469 [00:00<?, ?it/s]
      Test set: Average loss: 0.0170, Accuracy: 9945/10000 (99.45%)

      EPOCH: 15
      Loss=0.08451732248067856 Batch_id=468 Accuracy=98.43: 100%|██████████| 469/469 [00:53<00:00,  8.72it/s]
      Test set: Average loss: 0.0168, Accuracy: 9950/10000 (99.50%)

<b>Finally, by fine tuning the model in a step by step approach, the model was able to reach best test accuracy of 99.53% in 15 epochs with just 6720 (6K parameters)!!!
