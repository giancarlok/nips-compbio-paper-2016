As we have already seen in [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/FFNN%20vs%20FFNN22%20vs%20FFNN33%20vs%20FFNN44%20vs%20LSTM.ipynb), LSTM performs worse than FFNN particularly on non 9 mers.
Now we investigate how the performance of LSTM changes when also encoded with kmer_index_encoding wrt plain LSTM and wrt to FFNN with kmer_index_encoding.

## kmerLSTM vs plainLSTM vs FFNN on all mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN.png)

## kmerLSTM vs plainLSTM vs FFNN on 9 mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN_9mers.png)

## kmerLSTM vs plainLSTM vs FFNN on non 9 mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN_non9mers.png)
