
[[Roadmap]]



A token can be a character, a word or even a part of word eg-can't-Can and "t" are two sepeate tokens

![[tokenisation.png]]

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
	
  
![[types of LM.png]]

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
2. Embeddings: -
	1. each token gets converted into a list of numbers (vector) that represent its meaning. 
	2. we have positional vectors here so as to know which word come when in the sentence. so basically the embedding will contain what the word means along with where it is in the sentence. 
	3. the number of indices in the positional vector define the context length of the model
3. the embeddings are passed through the transformer layers (which contain weights):  
	1. Attention Layers : the model runs multiple round of attention - each word looks at every other word and figures out the relationship and context
		eg: [1,2,3] gets transformed into [2,1,4] after passing through this layer i.e it absorbs the context from surrounding word and gets converted into a vector similar to the overall context
	2. fed forward layers : after attention, each token passes through a neural network layer that further processes the information further. this takes only a single vector transformed in the previous attention layer and then multiplies by weights, applies transformations and then we get a vector which captures a better meaning in the given context
4. Repeat 3.1 and 3.2 multiple times
5. Output Unembedding layer
	1. it is reverse of a embedding matrix
	2. takes in a matrix and asks "which word in the vocabulary does the vector resemble"
	3. outputs a probability for each word and then the model picks one and outputs it as the output token.

# How does the attention layer work ? 
![[full_transformer_flow.svg|697]]

for a word to know which other words to pay attention to, it needs to do 3 things:
1. ask a question about what it is looking for (Query weigh)
2. advertise what information it contains (Key weight)
3. actually provide that information if asked (wrt pt. 2) (Value weigh)

let's understand by using a example
`The cat sat on the mat because it was tired`

we want to figure out what "it" means here. 

### Step 1 - Every word creates Q, K, V

each word multiplies it's embeddings vector by three weight matrices to create three vectors.

- "it" creates Q (its question)
- every word creates - K (their advertisement)
- every word creates - V (their actual content)

### Step 2 - "it" asks it question using Q

"It" asks question using Q ie. basically it asks I am a pronoun, who or what am i referring to  
### Step 3 - Match Q against every word's K

Now "it" compares it's query against every word's key:

`
```
"it" Q  vs  "The"  K  →  score: 0.1
"it" Q  vs  "cat"  K  →  score: 0.8  ← high match
"it" Q  vs  "sat"  K  →  score: 0.1
"it" Q  vs  "on"   K  →  score: 0.0
"it" Q  vs  "the"  K  →  score: 0.1
"it" Q  vs  "mat"  K  →  score: 0.2
"it" Q  vs  "because" K → score: 0.0
"it" Q  vs  "was"  K  →  score: 0.1
"it" Q  vs  "tired" K →  score: 0.1
```

"cat" scores highest because its key advertises : 
> I am a living creature, subject of the sentence

that matches with what "it" is looking for.

### Step 4 - Convert scores to probabilities

The scores get converted so they all add up to 1

### Step 5 - Multiply weights by values

Now "it" collects actual information from each word using these wights

```
Final vector = 
  0.6 × cat(V)    ← takes 60% of cat's information
+ 0.2 × mat(V)    ← takes 20% of mat's information  
+ 0.1 × tired(V)  ← takes 10% of tired's information
+ 0.1 × others(V)
```

### Step 6 - The vector for "it" has been updated

```
Before attention: "it" = [0.5, 0.5, 0.5]  (generic pronoun)
After attention:  "it" = [0.8, 0.3, 0.6]  (heavily influenced by "cat")
```

Now feed forward takes over i.e. the vector for "it" which now carries cat information gets passed into feed forward layer
> "Okay, this is a pronoun referring to a living creature that is tired. let me encode that meaning properly"

## Model Size

- based on the model parameters
- a higher size model generally outperforms a lesser size model
- a newer gen model is likely to outperform a older gen model of the same size
- the num of parameters help us estimate the compute resource required for training and running the model. 
  e.g. : A model with 7B params -> each param is stored using 2 bytes (16 bits)
  = 7B x 2 bytes = 14B bytes ~ 14 GB
- a sparse model has a large percentage of its parameters set to 0 value. this allows it to store and compute data more efficiently. this also means large sparse model can require less compute then a small dense model
- *MOE* (Mixture of Experts) Model is divided into different group of parameters and each group is an expert. also only a subset of params are active for processing each token
	- eg : Mistral 8x7B model is a mixture of eight experts, each expert taking around 7B params. if no two experts share parameters, it should have around 56B parameters but as some params are shared, it has only 46.7B params
	- at each layer, for each token, only 2 experts are active. this means that only 12.9B params are active for each token out of the total 46.7B params, so the cost and speed of this model is same as 12.9B param model
- when discussing model size, we have to account the size of the data it was trained on. a small model which was trained on large amount of data can outperform a large model which was trained on small amount of data
- a better measurement of how good a model is trained can be the number of tokens in the dataset because at the EOD, models operate on the tokens
- pre-training model requires compute and one way to measure this compute is using FLOP or floating point operation. it measures the number of floating point operations performed for a certain task.

	Summary : 
		Number of params is a proxy for the model's learning capacity
		Number of tokens a model is trained on, is a proxy for how much a model learned
		number of FLOPs is a proxy for training cost

# Post-Training

- why is post-training required ? 
	- in pre-training, the models are trained by self supervision and hence are optimized for text completion and not conversations. also the model is trained on data indiscriminately, so the output can be racist, sexist, rude or just wrong. the goal of post-training is to address these issues
- consist of two steps
	- supervised finetuning : training on high quality instruction data to optimize the model for conversations
	- preference finetuning : further finetune the model to output responses that align with human preferences. preferrable done using reinforcement learning (RL). techniques include : 
		- RLHF - Reinforcement learning from human feedback
		- DPO - Direct preference optimization
		- RLAIF - reinforcement learning from AI Feedback