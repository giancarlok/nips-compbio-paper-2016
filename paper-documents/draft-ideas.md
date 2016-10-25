# Study 1: NN vs LSTM vs kmerLSTM vs LSTM44 vs NN44

Nomenclature

* NN: a shallow neural net with one hidden layer of size 10, varying length peptides are encoded via kmer-index-encoding 

* LSTM: bidirectional LSTM of hidden size 50, followed by a sigmoid layer.
kmerLSTM: kmer-index-encoded LSTM 

* LSTM44, NN44: same models as above, only encoded via 44 encoding, which consists of the first 4 and last 4 amino acids of the peptide. 

In the following table, we calculated the AUC scores in percentages for a cutoff of 500 nM. The scores are taken to be the average from epochs 20 to 40

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/table.png)


* Conclusion 1: LSTM is worse than NN, especially on non 9 mers

* Conclusion 2: kmer LSTM is better than LSTM, especially on non 9 mers

* Conclusion 3: 44-encoding decreases overall performance of LSTM and NN

* Conclusion 4: LSTM44 is better than NN44 on non-9mers but is worse than NN44 on 9mers.

We should analyse the effects of internal architecture of the respective models and the effect of the kmer-index-encoding separately. Trained only on 9 mers, LSTM and NN still show significant difference in performance. We thus assume that the difference of performance between LSTM and NN are partially due to effects of the architecture and then enhanced by the effects of the kmer-index-encoding. 

# Study 2: New encoding for LSTM


The output of the LSTM layer consists of 100 neurons (50 coming form forward LSTM and 50 coming form backward LSTM), followed by a dense sigmoid layer. The shallow net consists of 10 hidden neurons followed by dense sigmoid layer as well. It is quite surprising that 10 hidden neurons outperform 100 hidden neurons! Now why is that? 

As a recap here the equations of the LSTM. In particular let's look at the output equation o_t.  
![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/Screen%20Shot%202016-10-25%20at%2009.25.43.png)

At each time-step, one amino acid is being fed to the embedding layer of the LSTM in the shape of a index encoded vector, which in turn is then transformed into a 32-dimensional vector (32 being the embedding dimension). In the equations the 32-dimensional vector at time step t is x_t. The matrices W map x_t linearly to a 50 dimensional vector. Note that the matrics W are the same for each time-step t , aka for each embedded amino acid x_t. Whereas for the shallow net each amino acid is being mapped with a different linear transformation to each of each hidden neuron! In particular this means that the same amino acid is being transformed by W in the same way no matter the position.

The only flexbility of the LSTM in changing the coefficients of the linear transformation are the hidden states h_t. Our only hope is that the hidden states are smart enough to learn how to modify the coefficients for each time-step. As the matrices U are also time-step independent, it seems that there is only so much hidden states can correct. We thus need to come up with something else. 

It would indeed be nice if the W were so large that the embedding layer of the LSTM can learn how to map amino acids from different positions to orthogonal subspaces and thus allowing the embedding 32-dimensional vector to have different coefficients at the positions corresponding to the respective subspace. This would only work prefectly if we had fixed length encoding. However what we can do is index-encode the same amino acid at different positions with different indices, leaving it up to the LSTM to learn how much amino acids at different positions should be embedded orthogonally, aka how much they should be treated differently in their linear transformation.  

The encoding: map amino acids at position 1 to integers 1 to 20, at position 2 to integers 21 to 40, at position 3 to integers 41 to 60 etc etc

## new encoding trained on and evaluated 9 mers

In order to take out the effects of kmer-index-encoding we first look LSTM and NN when trained only on 9 mers, and compare their performance when given the new encoding vs the old encoding:

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/new%20embedding%20.png)

Here we see in navy the LSTM with the new encoding, the NN with old and new encoding in orange and red respectively, as well as the LSTM with the old encoding in blue. LSTM with new encoding clearly improves vs LSTM with old encoding. For the NN the encoding doesnt make much difference, as the NN already has flexible coefficients in the linear combination of each coordinate of the embedded amino acids from different time steps. 

When trained on all mers, we only compare NN old encoding (as kmer-index-encoding messes up the new encoding), with LSTM new and old encoding repsectively. We see that LSTM improves with new encoding and has similar performance than NN (with old encoding). 

## new encoding trained on allmers evaluated on all mers

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/new%20embedding%20trained%20on%20all%20mers.png)


## new encoding trained on allmers evaluated on 9 mers

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/new%20encoding%20trained%20on%20all%20mers%20evaluated%20on%209%20mers.png)

## new encoding trained on allmers evaluated on non 9 mers

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/new%20encoding%20trained%20on%20all%20mers%20evaluated%20on%20non%209%20mers.png)
