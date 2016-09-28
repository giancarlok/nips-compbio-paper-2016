As we have already seen in [notebook](https://github.com/giancarlok/nips-compbio-paper-2016/blob/master/FFNN%20vs%20FFNN22%20vs%20FFNN33%20vs%20FFNN44%20vs%20LSTM.ipynb), LSTM performs worse than FFNN particularly on non 9 mers.
Now we investigate how the performance of LSTM changes when also encoded with kmer_index_encoding wrt plain LSTM and wrt to FFNN with kmer_index_encoding.

## kmerLSTM vs plainLSTM vs FFNN on all mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN.png)

## kmerLSTM vs plainLSTM vs FFNN on 9 mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN_9mers.png)

## kmerLSTM vs plainLSTM vs FFNN on non 9 mers 

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/kmerLSTM_vs_plainLSTM_vs_FFNN_non9mers.png)

## Conclusion 

We see a significant improvement in performance on non 9 mers  when applying the kmer-index-encoding to LSTM. This is probably due to the fact that a 9 mer sequence s can be weak binder but when appended another amino acid it can become a stronger binder.It seems that LSTM is not flexible enough to react well to those jumps when given only one shot to read this peptide, while FFNN gets 10 shots and the result gets average. So it seems that averaging is a powerful tool to increase performance on non 9 mers, and to cushion deviation from strong irregularities in binding affinity similar to the one we described.

## LSTM vs FFNN trained and tested only on 9 mers

![](https://raw.githubusercontent.com/giancarlok/nips-compbio-paper-2016/master/LSTM_vs_FFNN_trained_and_tested_on9mers.png)
