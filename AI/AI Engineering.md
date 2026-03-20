
Roadmap
[[Roadmap]]



A token can be a character, a word or even a part of word eg-can't-Can and "t" are two sepeate tokens

![[Pasted image 20260304020420.png]]

Neural Networks are nothing but just prediction models that predict the next word so you provide a bunch of words to a LLM/Neural Network and it tries to predict a correct next word

The next word prediction forces Neural network to learn a lot about the world.


there are two types of language modes: 
- masked language models
	- this is trained to predict missing tokens anywhere in a sequence, using the context from both before and after the missing tokens. basically it is trained to be able to fill in the blanks
	- these are mostly used in non generative tasks such as sentimental analysis. these are also useful for tasks requiring understanding of overall context such as code debugging.
- autoregressive language models
	- this is trained to predict the next token in a sequence using only the preceding tokens. it predicts what comes next in "My fav color is ________"
	- it can continuously generate one token after another
	- used in text generation and are more popular than masked language models
	
  
![[Pasted image 20260311132716.png]]

# Supervision

A label is nothing but a correct answer.

Supervised learning involves labelling a data eg: for fraud detector, we might need to pass transactions with labels classifying them into fraud and not fraud and then passing them to classifier. we then test the classifier on new data to test how it works but this is time consuming and expensive, for a model like chatgpt, a technique called self-supervision is used.

# Self-Supervision

In this, instead of requiring explicit labels, the model can infer the labels from input data and use them. for language, the text is self labelling. given a piece of text, the model convert it into multiple training samples and use them for training. so basically it tries to guess the next word, correct itself if it is wrong repeatedly and gradually builds up the knowledge of language.
# Types of Models

There are three types of model
- task specific model such as a model used for sentimental analysis
- multimodal model used for variety of tasks and type of data, eg: Provide a image to the model and ask what it is, so this model now takes image and text as input.
- foundational model is a large model trained on massive data that can be a base for many different tasks. instead of training new model from scratch every time, you train one giant model once and then it adapts for specific use case. eg : GPT4

# Neural Network

A neuron is nothing but a mini calculator which
- takes input
- multiplies each input by a number (weight)
- adds them up
- adds one more number (bias)
- outputs the result

parameters or weights are adjusted millions of times during training to have them used to provide the most accurate prediction

# Model Parameters

Parameter can be considered as a numerical values which influences the decision of the prediction during generating the answer. parameters or weights are adjusted multiple times (millions) to provide the most accurate prediction. 
`Total Parameters = (number of weights) + (number of biases)`

A model file is a file which contains the parameters which is a number
`size of the model file = float number (4 bytes) * total parameters 
so size of a 7B parameter model will be  `4 x 7 = 28GB`



# Pretraining

- Pretraining refers to training a model from scratch. 
- model weights are set to random initially and then they are corrected as the model is trained. 
- this often involves training a model for text completion.
- most resource and time intensive

# Post Training or Fine-tuning

- means training on previously trained model and model weights
- requires fewer resources as the model is already trained

# Transformers

when a text like : 
`The animal did not cross the road because it was too tired`
are given to a model, how does it know that the 'it' refers to the animal and not the road. 

for gaining the full understanding of what the "It" is refering to, we have to read the full sentence and make sense out of it

here is where transformers come in, developed by google, transformers read the entire sentence and then establishes a relationship between them to answer the question

the main innovation in transformers is something called *Attention*:
- it basically asks "while processing this word, which other words in the sentence should i focus on"

for the "it" above:
- high attention on -> "Animal", 
- low attention on -> "road", "cross"

the model learns to pay attention to **relevant** words not just nearby ones.

the transformer can be broken down into two parts:
1. Encoder: 
   - Takes in your sentence
   - builds a rich understanding of it with attention
   - outputs a numerical representation of meaning
1. Decoders:
   - takes in the encoder's understanding
   - generates a response word by word
   - also uses attention

so basically this is what happens when you type a prompt

![[Flow with Attention layer]]

1. Tokenization: text is broken down into tokens
2. Embeddings: each token gets converted into a list of numbers (vector) that represent its meaning
3. Attention Layers : the model runs multiple round of attention - each word looks at every other word and figures out the relationship and context
4. fed forward layers : after attention, each token passes through a neural network layer that further processes the information further
5. Repeat 3 and 4 multiple times
6. output the final prediction ie token, pick it up, add it to input and repeat until response is complete

on pg. 137