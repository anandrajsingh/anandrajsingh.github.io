<p style="font-size:24px;"><b>NLP Attempt</b></p>
Natural Language Processing

**#1 The Dictionary approach**
The dictionary definition is not enough for real-world language. Word meaning changes with context.

**#2 Statistical Patterns**
Pattern matching does not means understanding.
The machine could predict the next word based on frequency, but it had no concept of what words actually meant.

<p style="font-size:24px;"><b>The Breakthrough</b></p>
Words as Numbers

**Word Embeddings**
Transforming a word to it's vector form is known as embedding.

**Visualising Words**
Similar meaning words are placed close together in a multi-dimensional space. Proximity indicates shared meaning.

**#4 Sequence Models(RNNs)**
<b>Recurrent Neural Network</b>
Process the sentence one word at a time,building up understanding as you go. The model maintains a "memory" of what it has read so far.

**The Problem: Forgetting**
The cat, which was sitting on the mat which I bought from the store near the old Church on the corner was happy.

By the time model reaches "was" it as forgotten the subject at the beginning.

RNNs struggle with long sentence because they process information linearly and have limited memory capacity.

**The Transformer Idea**
<p style="font-size:18px;"><b>Sequential</b></p>
Process words one by one, like reading a book from start to finish.

<p style="font-size:18px;"><b>Simultaneous</b></p>
Look at each word in the sentence at the same time.

<p style="font-size:18px;"><b>The Secret: Attention</b></p>
The model attends to the most relevant words, no matter how far apart they are.

**Attention in Action**
The animal didn't cross the street because it was too tired.
When the model reaches at the word "it", the attention mechanism tells it to pay the most attention to "animal".
This is how the model "knows" what the pronoun refers to, building a context aware representation of every word.


<p style="font-size:24px;"><b>Why Attention is Powerful</b></p>

**More Memory**
Every word in sentence sees every other word simultaneously, no matter how far apart they are.

**Parallel Processing**
Unlike RNNs that read word by word. Transformers process all the word at once, making the training incredibly fast.

**Deep Understanding**
The model builds mathematical map of how every word relates to every other word in the specific context.


**The Transformer**
An architecture built entirely on the mechanism of attention.