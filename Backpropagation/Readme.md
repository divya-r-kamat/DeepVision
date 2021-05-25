# Neural Network BackPropogation using Excel

Backpropagation is a common method for training a neural network. The goal of backpropagation is to optimize the weights so that the neural network can learn how to correctly map arbitrary inputs to outputs. We will see how it works with a concrete example with calculations using excel sheet to understand backpropagation correctly. Here, we’re going to use a neural network with two inputs, two hidden neurons, two output neurons and we are ignoring the bias.


![nn](https://user-images.githubusercontent.com/42609155/119422596-12160180-bd1f-11eb-90fa-9a0ded65b23f.png)

Here are the initial weights, for us to work with:

    w1 = 0.15	w2 = 0.2	w3 = 0.25	w4 = 0.3
    w5 = 0.4	w6 = 0.45	w7 = 0.5	w8 = 0.55



We’re going to work with a single training set: given inputs 0.05 and 0.10, we want the neural network to output 0.01 and 0.99.

## Forward Propogation

We will first pass the above inputs through the network by multiplying the inputs to the weights 
    
      h1=w1*i1+w2+i2
      h2=w3*i1+w4*i2
      
      a_h1 = σ(h1) = 1/(1+exp(-h1))
      a_h2 = σ(h2) = = 1/(1+exp(-h2))
      o1 = w5*a_h1 + w6*a_h2
      o2 = w7*a_h1 + w8*a_h2
      a_o1 = σ(o1)
      a_o2 = σ(o2)
      

## Calculating the Error (Loss)
      
    E1 = ½ * ( t1 - a_o1)²
    E2 = ½ * ( t2 - a_o2)²
    E_Total = E1 + E2
    
    
## Back Propogation

    δE_total/δw5 = (a_o1 - t1 ) *a_o1 * (1 - a_o1 ) * a_h1
    δE_total/δw6 = (a_o1 - t1 ) *a_o1 * (1 - a_o1 ) * a_h2
    δE_total/δw7 = (a_o2 - t2 ) *a_o2 * (1 - a_o2 ) * a_h1
    δE_total/δw8 = (a_o2 - t2 ) *a_o2 * (1 - a_o2 ) * a_h2

    δE_total/δa_h1 =(a_o1 - t1) * a_o1 * (1 - a_o1 ) * w5 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w7
    δE_total/δa_h2 =(a_o1 - t1) * a_o1 * (1 - a_o1 ) * w6 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w8
    
    δE_total/δw1 = ((a_o1 - t1) * a_o1 * (1 - a_o1 ) * w5 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w7) * a_h1 * (1- a_h1) * i1
    δE_total/δw2 = ((a_o1 - t1) * a_o1 * (1 - a_o1 ) * w5 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w7) * a_h1 * (1- a_h1) * i2
    δE_total/δw3 = ((a_o1 - t1) * a_o1 * (1 - a_o1 ) * w6 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w8) * a_h2 * (1- a_h2) * i1
    δE_total/δw4 = ((a_o1 - t1) * a_o1 * (1 - a_o1 ) * w6 + (a_o2 - t2) * a_o2 * (1 - a_o2 ) * w8) * a_h2 * (1- a_h2) * i2


