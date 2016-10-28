# A) Preliminary remarks

I ran everything in keras. I was not entirely sure about the keras methods, but I used the method `GlobalAveragePooling1D()`for taking the average of each coordinate across all the 32 dimensional embedded amino acid vectors. 

For convolution of length k, I used `Convolution1D(nb_filters = 32, input_shape(peptide_length ,32), filter_length = k) ` on each 32 dimensional embedded amino acid vector, before taking the average with GlobalAveragePooling1D()

I split my experiments in two parts: first, I trained everything on 9 mers (amino acid sequences of length 9) only to separate the effect of the kmer_encoding of the shallow net that is being used in the state-of-the-art method. secondly, I trained everything on allmers.

What is kmer_encoding ? 9 mers are just read in normally with training weight 1, whereas non 9 mers are broken down into a bunch of 9 mers with according training weight. Example: 10 mer is broken into all 10 subsequences of 9 mers, all of which are carrying the same label as the initial 10 mer; every such 9 mer having training weight 1/10. When doing the prediction of a 10 mer, we predict the label for all 9 mer subsequences and take the arithmetic mean, which comes down to geometric mean of the initial IC50 considering the fact that the labels have been (1-log_50000)-transformed to be between 0 and 1. 

All results are expressed in terms of AUC, where the cutoff corresponds to initial IC 50 value of 500 nM. 

We used 3 fold cross validation. 

# B) Training only on 9 mers

## Experiment 1: finding the right filter length cutoff

Z_0: average 32 dimensional vector of the 9 embedded vectors 
Z_1: average of the convolutions with filter length = 2
...
Z_k: average of the convolutions with filter length = k+1

NN_k: shallow net with one hidden layer of size 10, where inputs are the concatenation of Z_0,...,Z_k

NN_8 outperforms the initial shallow net NN , whereas NN_6 doesnt. 
NN_8 > NN >  NN_6 > NN_4 > NN_2 > NN_0

## Experiment 2: involving the LSTM [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN8%20vs%20NN%20trained%20on%209%20mers.%20.ipynb)

the LSTM model that we will run against NN_8 and NN, will have as input the concatenated (and then (9,32)-reshaped) vector [Z_0,Z_1..., Z_8]. 
It has hidden size 50.

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN8%20vs%20NN%20trained%20on%209%20mers.png)

# C) Training only on allmers

