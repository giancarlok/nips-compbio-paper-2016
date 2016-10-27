# Idea 

* use LSTM only to learn long-term dependencies as opposed to local dependencies. 
* Do a step by step process where you filter out local dependencies of various radii and look at the performance. (very similar to fourier transforms)

# Step 1

* Take your 32 dimensional embedded amino acids for each amino acid in the sequence, and 32 dimensional average vector Z_0. 
* Feed that vector into the hidden layer of the shallow net and see how well you perform.

# Step 2

* take convolutional filter of size 2 and average the n-1 32 dimensional vectors, call this vector Z_{1}. n being the peptide length
* Take the flattened vector [Z_0,Z_1] and feed it into the shallow net and see how well you perform.

# Step 3

* take convolutional filter of size 3 and average the n-2 32 dimensional vectors, call this vector Z_{2}.
* Take the flattened vector [Z_0,Z_1,Z_2] and feed it into the shallow net and see how well you perform.
* keep going until you find reasonable k so that [Z_0,Z_1,...,Z_k] gives good performance. one of k=2,3,4 should be enough
* note that Z_8 gives something similar to a simple shallow net applications (with 9mer-index-encoding)

# Step 4

* So far we have learned local dependencies with different radii. 
* If there is still more complex long-term dependencies, we might use LSTM to complement what shallow nets cant learn. 
* instead of feeding one embedded amino acid at a time into the LSTM, we would feed Z_0, Z_1,..., this would allow the LSTM to forget the right information at the right time, as otherwise the LSTM gets an overload of information and might start forgetting information that turns out useful when looking at the global picture.


# Ressources

https://arxiv.org/abs/1408.5882


https://arxiv.org/abs/1610.03017


http://nlpers.blogspot.com/2016/08/fast-easy-baseline-text-categorization.html
