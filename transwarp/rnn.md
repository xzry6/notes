# Note: Recurrent Neural Network(RNN)
Recurrent Neural Network(RNN) is popular deep learning models and has shown great value in NLP tasks. Here is a learning note of RNN.

### Agenda
* [Introduction](#introduction)
*
*

### Introduction
To understand how RNNs work, let's first take a look at neural networks.
Say we have a normal and simple nerural network. When dealing with a NLP task, we'll mostly take care of the target word.
We'll encode the whole target word and feed it into this network.
Then how about the adjacent words?
We all know that nearby words are highly related to target words in a sentence, how do a deep network deal with these relations?<br>
Here comes out <strong> Recurrent Neural Network(RNN) </strong>.
RNN is a network with the idea of making use of sequential information.
RNN are called recurrent because they perform the same task for every element of a sequence, with the output being depended on the previous computations.
Here is what a typical RNN looks like: 
![rnnStructure](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/09/rnn.jpg)

