# nips-compbio-paper-2016

Here I am outlining the ideas for a potential workshop paper for NIPS 2016. 

Paper ideas:

1st paper
* compare FFN + index encoding vs. FFN(seq[:4] + seq[-4:]) vs. LSTM
* the effect of learning rates on all these architectures

2nd paper
* pimp LSTM so that it comes close to or outperforms FFNN.

3rd paper
* apply LSTM to MHC class II prediction


Things to include in the paper(s)
* a page of exposition on the data + problem
* the challenge of reducing short sequences into fixed input size models
* talk about how LSTM _would_ be ideal, and why it is a candidate
* the importance of the learning rate

Some random ideas:
* view LSTM as encoding technique, get LSTM to mimick `kmer_index_encoding()` to get comparabale result, then tweak it to beat it. 
* add attentional gate to LSTM
* consider deep LSTMs
* check what is going on with forget, and input gate.
