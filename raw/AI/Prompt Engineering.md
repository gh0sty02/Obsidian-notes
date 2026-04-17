
### In-Context Learning - ZeroShot and few Shot

In context learning basically means that the model learns from it's context before answering a question. 
eg : If a model is trained on the old Javascript documentation and it is asked a question, it will refer that old data for generating the answer. but suppose we provide the new rules and syntaxes in the context, the model will use this data for better answer.

Each Example provided in the prompt is called a shot. teaching a model to learn from examples in the prompt is also called `Few shot learning`.
5 examples - 5 shot learning
0 examples - zero shot learning.

the more the examples, the better the model is expected to perform. the number of examples in only limited by context length and large number of examples results in longer prompt and hence large inference cost.

## Prompt vs Context

Prompt - whole input to the model
Context - the information provided to the model so that it can perform a given task

## Context Length vs Context Efficiency

Context length is the amount of tokens of information in the prompt that a  model can handle. but not all parts of the prompts are equal, some models might better understand the instructions at the start of the prompt while some can do that at the end. 

A better way to evaluate the effectiveness is by using a test known as Needle in a haystack. the idea is to insert random piece of information (needle) in different location of the prompt (haystack) and ask the model to find it.
