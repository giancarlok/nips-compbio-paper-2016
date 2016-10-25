# Study 1: FFNN vs LSTM vs kmerLSTM vs LSTM44 vs FFNN44

Nomenclature

FFNN: a shallow neural net with one hidden layer of size 10, varying length peptides are encoded via kmer-index-encoding 
LSTM: bidirectional LSTM of hidden size 50, followed by a sigmoid layer.
kmerLSTM: kmer-index-encoded LSTM 
LSTM44, FFNN44: same models as above, only encoded via 44 encoding, which consists of the first 4 and last 4 amino acids of the peptide. 

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/table.png)


Conclusion 1: LSTM is worse than FFNN, especially on non 9 mers

Conclusion 2: kmer LSTM is better than LSTM, especially on non 9 mers

Conclusion 3: 44-encoding decreases overall performance of LSTM and FFNN

Conclusion 4: LSTM44 is better than FFNN44 on non-9mers but is worse than FFNN44 on 9mers.

We should analyse the effects of internal architecture of the respective models and the effect of the kmer-index-encoding separately. Trained only on 9 mers, LSTM and FFNN still show significant difference in performance. We thus assume that the difference of performance between LSTM and FFNN are partially due to effects of the architecture and then enhanced by the effects of the kmer-index-encoding. 

Study 2: New encoding for LSTM

![](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/paper-documents/Screen%20Shot%202016-10-25%20at%2009.25.43.png)

