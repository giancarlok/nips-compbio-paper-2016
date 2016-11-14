# A) Preliminary remarks

I ran everything in keras. I was not entirely sure about the keras methods, but I used the method `GlobalAveragePooling1D()`for taking the average of each coordinate across all the 32 dimensional embedded amino acid vectors. 

For convolution of length k, I used `Convolution1D(nb_filters = 32, input_shape(peptide_length ,32), filter_length = k) ` on each 32 dimensional embedded amino acid vector, before taking the average with GlobalAveragePooling1D()

I split my experiments in two parts: first, I trained everything on 9 mers (amino acid sequences of length 9) only to separate the effect of the kmer_encoding of the shallow net that is being used in the state-of-the-art method. secondly, I trained everything on allmers.

What is kmer_encoding ? 9 mers are just read in normally with training weight 1, whereas non 9 mers are broken down into a bunch of 9 mers with according training weight. Example: 10 mer is broken into all 10 subsequences of 9 mers, all of which are carrying the same label as the initial 10 mer; every such 9 mer having training weight 1/10. When doing the prediction of a 10 mer, we predict the label for all 9 mer subsequences and take the arithmetic mean, which comes down to geometric mean of the initial IC50 considering the fact that the labels have been (1-log_50000)-transformed to be between 0 and 1. 

All results are expressed in terms of AUC, where the cutoff corresponds to initial IC 50 value of 500 nM. 

We used 3 fold cross validation. 

# B) Training only on 9 mers

## Experiment 1: finding the right filter length cutoff [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN_k%20's%20trained%20on%209%20mers.ipynb)

Z_0: average 32 dimensional vector of the 9 embedded vectors 
Z_1: average of the convolutions with filter length = 2
...
Z_k: average of the convolutions with filter length = k+1

NN_k: shallow net with one hidden layer of size 10, where inputs are the concatenation of Z_0,...,Z_k

NN_8 outperforms the initial shallow net NN , whereas NN_6 doesnt. 
NN_8 > NN >  NN_6 > NN_4 > NN_2 > NN_0.

(Also note that there is a significant gap between NN_2 and NN_4.)

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN_k%20trained%20on%209%20mers.png)

## Experiment 2: involving the LSTM [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN8%20vs%20NN%20trained%20on%209%20mers.%20.ipynb)

the LSTM model that we will run against NN_8 and NN, will have as input the concatenated (and then (9,32)-reshaped) vector [Z_0,Z_1..., Z_8]. 
It has hidden size 50.

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN8%20vs%20NN%20trained%20on%209%20mers.png)

# C) Training on allmers

## Experiment 1: NN vs NN_k's (NN_k's WITHOUT kmer-encoding) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN_k's%20trained%20on%20allmers%20(without%20kmer).ipynb)

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NNk%20allmers%20on%20allmers%20without%20kmer%20.png)

Clearly NN outperforms everybody else. Note that here NN has kmer encoding while all NNk's dont have kmer-encoding. Note that kmer-encoding is not necessary for NNk models due to taking average (aka `GlobalAveragePooling1D()` method), so we might be tempted to first try without kmer-encoding. Now that we saw that is gives worse performance without, let us now try NNk models with kmer encoding.  

## Experiment 2: NN vs NN_k's (NN_k's WITH kmer-encoding) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN_k's%20trained%20on%20all%20mers%20(with%20mer%20encoding%20).ipynb)

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NNk%20allmers%20on%20allmers.png)

NN8 clearly outperforms NN, when trained on all  mers. All NN_k's as well as NN having kmer-encoding. 

In the notebook, the performance is then further analysed on 9 mers and non 9 mers. The gap between NN_8 and NN becomes larger on non 9 mers.

## Experiment 3a: LSTM vs NN vs NN_8 (LSTM WITHOUT kmer-encoded) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN8%20vs%20NN%20trained%20on%20all%20mers%20(lstm%20without%20kmer).ipynb)

As in the previous section, let's add the LSTM to the whole story. A priori LSTM doesnt require a fixed length encoding, so let us first try without. 

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN%20vs%20NNk%20(lstm%20without%20kmer)%20.png)

We see that LSTM takes forever to come up to speed with the other models. So let us try kmer-encoding. 

## Experiment 3b: LSTM vs NN vs NN_8 (LSTM WITHOUT kmer-encoded) (longer epochs) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN%20vs%20NN8%20(longer%20epochs).ipynb)

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/LSTM%20vs%20NN%20vs%20NN8%20(longer%20epochs).png)


## Experiment 4: LSTM vs NN vs NN_8 (all models WITH kmer-encoded ) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/new%20LSTM%20vs%20NN%20vs%20NN8%20(lstm%20without%20kmer).ipynb)

not good idea, performance of LSTM drops like crazy.

## Experiment 5: new LSTM vs NN vs NN_8 (lstm WITHOUT kmer-encoded) [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/new%20LSTM%20vs%20NN%20vs%20NN8%20(lstm%20without%20kmer).ipynb)

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/new%20LSTM%20vs%20NN%20vs%20NN8.png)

## Experiment 6: NN vs NN_8 vs 2 layer conv NN_8 [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN8%20vs%20NN8w2convlayers_without_averaging.ipynb)

Stacking two convolutional layers while not doing any pooling doesnt seem to be helpful. NN8 still performs best.  

## Experiment 7: NN vs NN_8 vs NN_8 without averaging [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/convolution-idea/NN%20vs%20NN8%20vs%20NN8_without_averaging.ipynb)

without pooling performs similar to pooling
