# Learning rates (with 1/epoch decay) ([notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM%20LR%20discovery.ipynb))

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LR%20exploration%20.png)




# Dropout regularization analysis ([notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM9%20vs%20LSTM9_U%20vs%20LSTM9_W%20vs%20NN.ipynb))

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM%20dropout%20regularity.png)

W dropout seems to be the right parameter to tune


# W dropout ([notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTMW%20analysis%20with%20W%20%3D%200.5%20part%201.ipynb))

Here we set W dropout to 0.5 in the hope to see even stronger effeect than above. 

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/W%20regularization.png)

# What happens with maximal filter length 26 ? ([notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTMW%20analysis%20W%20%3D%200.5%20part2.ipynb))
## on all mers


![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM26W%20%20allmers.png)

Here we see that the model is outperforming the NN model (at least for a while, until overfitting occurs). So we would have to impose stronger regularity to keep the performance steady. 

Let us dive into 9 mers vs non 9 mers analysis
## on 9 mers

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM26W%20on%209%20mers.png)



## on non 9 mers

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/regularization/LSTM26W%20non%209%20mers.png)

here we see that the model needs some time to understand what is happening with the empty postions, but eventually gets it.
