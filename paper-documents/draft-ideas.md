# Study 1: FFNN vs LSTM vs kmerLSTM vs LSTM44 vs FFNN44

Nomenclature

FFNN: a shallow neural net with one hidden layer of size 10, varying length peptides are encoded via kmer-index-encoding 
LSTM: bidirectional LSTM of hidden size 50, followed by a sigmoid layer.
kmerLSTM: kmer-index-encoded LSTM 
LSTM44, FFNN44: same models as above, only encoded via 44 encoding, which consists of the first 4 and last 4 amino acids of the peptide. 


\begin{tabular}{lrrrrrrr}

{} &      3 CV &    9 mers &    fold 1 &    fold 2 &    fold 3 &  fold std &  non-9 mers \\
\midrule
NN        &  0.950406 &  0.958275 &  0.951209 &  0.951737 &  0.948270 &  0.001525 &    0.928339 \\
LSTM      &  0.948237 &  0.957045 &  0.946884 &  0.951718 &  0.946107 &  0.002482 &    0.922078 \\
kmer LSTM &  0.948563 &  0.954865 &  0.947756 &  0.950735 &  0.947199 &  0.001552 &    0.930895 \\
NN44      &  0.946067 &  0.956989 &  0.946564 &  0.947490 &  0.944147 &  0.001410 &    0.913283 \\
LSTM44    &  0.944936 &  0.954914 &  0.945201 &  0.947156 &  0.942450 &  0.001930 &    0.915253 \\

\end{tabular}


Conclusion 1: LSTM is worse than FFNN, especially on non 9 mers

Conclusion 2: kmer LSTM is better than LSTM, especially on non 9 mers

Conclusion 3: 44-encoding decreases overall performance of LSTM and FFNN

Conclusion 4: LSTM44 is better than FFNN44 on non-9mers but is worse than FFNN44 on 9mers.

We should analyse the effects of internal architecture of the respective models and the effect of the kmer-index-encoding separately. Trained only on 9 mers, LSTM and FFNN still show significant difference in performance. We thus assume that the difference of performance between LSTM and FFNN are partially due to effects of the architecture and then enhanced by the effects of the kmer-index-encoding. 

Study 2: New encoding for LSTM



