Tolkein Text
Tolkein Text is live here!

I trained an LSTM neural network language model on The Lord of the Rings, and used it for text generation.  
 

Motivation
The motivation of this project was to gain experience with:

Breaking down and completing an NLP task
Implementing machine learning in Pytorch
Modern architectures for neural network language models
Preprocessing and tokenizing raw text data
Applications of language models
 
 
Some Examples
"Arrows fell from the sky like lightning hurrying down."
"At that moment Faramir came in and gazed suddenly into the sweet darkness."
"Ever the great vale ran down into the darkness. Darker the leaves rolled suddenly through the summer mist."
"And the moon was in the dark, and they lay all, grey and fading, terrible and fair."

These sentences definitely capture the feel of Tolkein's writing, but are all original sentences! I especially love the fact that it creates its own simile, "Arrows fell from the sky like lightning hurrying down," that does not appear in the original text.
 
 

Preprocessing
The preprocessing stage can be broken down into these steps:

Load in the full Lord of the Rings text
Remove capitalization and some punctuation with Regex
Build vocabulary that associates each word to a unique id. This includes word_to_id (dictionary), and id_to_word (direct access table)
Map each word in the text to its id
Add each word_id in the text to dataset as a label with the n word_ids before it as its feature (currently using n=9)
This list of (Feature, Label) pairs is the training data
 
 
Network Architecture
The network has the following architecture: Input -> Embedding layer -> LSTM layers -> Dropout layer -> Fully Connected Linear Layer -> Output

Input is the n preceding word_ids I mentioned before.
Output is a list of vocab_size values, where a higher value means that that word is more likely to occur next.
 
 

Network Training
Using the network architecture stated above and cross entropy loss, I find the weights that minimize loss on the training data. These weights are calculated with gradient descent using Pytorch's Autograd package.
 
 

Text Generation
By applying the softmax function to the output of the network, I get a probability distribution over all words in the vocabulary. Then, I take a weighted random choice of these to decide the next word.

With the softmax function, I use a temperature parameter of 0.8. This value punishes lower probabilities slightly more than the default value of 1. This increases the coherence of the generated text, and improves its adherence to grammatical rules.  
 

Streamlit
After training the model, I set it up with simple web interface using Streamlit. I wanted to allow other people to try generating text with the model and running custom input through it. I chose Streamlit since it allowed me to create and host the model's web interface very easily.
