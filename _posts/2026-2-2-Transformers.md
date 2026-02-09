From Text to Number

**The Character-level Approach**

Idea 1: One number per character

The most basic way to achieve numerical representation is to assign a unique integer to every individual character in the alphabet.

While simple, this approach operates at the smallest possible unit of language, ignoring words and concepts entirely.



**Why Character-level Models Struggle**

The hidden cost of extreme granularity

<b>The Step Problem</b>

A word like "understanding" needs 13 separate steps to process. The model must work many time harder to see one cocept.

- <b>Manual Pattern Learning:</b> The model must learn prefixes and roots from individual characters rather than recieving those units directly.
- <b>High Cognitive Load:</b> It's like teaching reading by showing letters only, building grammar and meaning becomes much harder.
- <b>Extreme Effeciency:</b> Large amounts of data are spent learning basic vocabulary instead of higher-level login.


**The Word-level Approach**

Idea 2: Assigning one unique number to every whole world 

<b>The Concept</b>
The cat sat     [1,2,3]

<b>Concept Mapping</b> Each word represents a single, discrete unit of meaning.
Much more intiutive than character-level processing.

<b>The Challenges</b>
- <b>The Form Problem:</b> Are "cat", "cats", and "cat's" three different concepts?
- <b>The Unknown:</b> How to handle typos like "teh" or rare technical terms.
- <b>Out of vocabulary:</b> Any word not in training sets become a "hole" in understanding.


**The Vocabulary Explosion**

Scaling Challenges of word-level models

**170,000+** common english words

**3 Billion** Parameters for Lookup

Computational Bottleneck

- <b>Parameter Bloat</b> Each unique word needs it's own embedding row; huge vocabularies create large memory overhead
- <b>The Unknown Proble</b> Fixed vocabulary fail on names, slang and new terms, reducing robustness
- <b>Multilingual Scaling</b> Adding languages multiplies vocabulary size, driving up compute and storage costs

**The Subword Solution**
Finding the "GoldiLocks" balance between character and words

<b>Strategic Splitting</b>

- <b>The Middle Ground:</b> Bigger than character(Too granular) but smaller than whole words(Too many)
- <b>Effieciency:</b> Common words stay whole; rare words break into recognizable, meaningful pieces.
- <b>Example: <b>"understanding" --> ["under", "stand", "ing"]
- A vocabulary of 30k -100k tokens can represent almost any text in any language.

**Byte Pair Encoding**
Algorithmic construction of modern tokenizers

1. <b>Initialization:</b> Begin with each character as individual token
2. <b>Iterative Merging:</b> Merge the most frequent adjacent token pair into one.
3. <b>Optimization:</b> Repeat merges until reaching the target vocabulary size.
4. <b>Self-Organizing:</b> Frequent patterns become single tokens; rare ones stay fragmented.

**Tokenization in modern practice**

How GPT-style tokenizers process real-world text

<b>Space Handling</b>

"Hello World" --> ["Hello"," world"]
Spaces are often attached to the beginning of the following word, not treated as separate token.

<b>Contraction</b>

"don't" --> ["don","'t"]
Common contractions are split into specific markers the model has learned to associate with negation or possesion.

<b>Complex Words</b>

"artificial intelligence" -> ["art","ificial"," intelligence"]
Long and rare words are broken into subword units that might not always align with linguistic syllables.

<b>Impact: Understanding these splits is crucial for managing context limits and API costs</b>


**Why Tokenization Matters**
The practical implications of how models "see" text

<b>Operation Impact</b>

- <b>Context limits: </b> GPT-4's 128k limit refers to tokens, not words. In English, this is roughly 1.3 tokens per word, but code can be much higher.
- <b>Financial Costs: </b> API billing is calculated based on token counts, not character counts. Efficient prompting requires understanding token density.

<b>Model Behaviour</b>

- <b>Logic Failure: </b> Strange failure in counting letters and reversing words often stem from the model seeing subword units rather than individual characters.
- <b>Language Equity: </b> Different languages require different tokens to express the same concept, affecting both cost and performance globally.


**Summary: The model's "Eyes"**

Tokenization as the foundation of transformer processing

<b>The Granularity Balance</b>

Characters -> Too Granular
Words -> Too many
Subwords -> Just Right

The tokenizer determines the fundamental units the model uses for "thought".

<b>Operation Impact</b>
Poor tokenization makes the model work harder to understand the context and can lead to strange failure in basic tasks.

If the model can't "see" a pattern in the tokens, it can't learn the underlying meaning of the text.


**From Arbitrary IDs to Meaning**

Moving beyond simple integer labels

<b>The Problem</b>
"Hello World" -> [15496, 995]
The IDs are arbitrary. The 15496 doesn't know it represents a greeting, and 995 doesn't mean anything about Earth.

<b>The Goal</b>
We need representations that capture MEANING.

We need to transform these discreet, arbitrary integers into continuous, dense vectors where the numbers themselves encode semantic relationships.

In this new space, "Hello" and "Hi" should be mathematically similar, while "Hello" and "Banana" should be distant.

**The Concept of Embeddings**

Representing tokens as points in in high-dimensional space

- <b>Spatial Logic: </b> Tokens with similar meanings are placed near each other in vector space. Proximity equals semantic similarity.
- <b>Vector Representaion: </b> Each token becomes a list of numbers (a vector) instead of a single ID, allowing for mathematical operations on meaning.
- <b>Multidimensionality: </b> Modern models use hundred of thousands of dimensions to capture the subtle nuance of human language.


**Visualizing Semantic Space**

How models organize concepts across dimensions

- <b>Clustering: </b> Related concepts (e.g. royalty, gender, animal) naturally group together in the high-dimensional vector space.
- <b>Emergent Properties: </b> Dimensions like "gender" or "royalty" emerge automatically during training without manual labeling.
- <b>Semantic Navigation: </b> The mathematical "distance" between points represents the semantic similarity of the tokens.


**Semantic Vector Arithmetic**

Mathematical logic embedded in language

king - man + woman $ \approx $   queen

- <b>Directional Meaning</b> The man -> woman vector mirros king -> queen.
- <b>Relational Encoding</b> Embeddings encode relations as consistent offsets.
- <b>Universal Patterns</b> These offsets recur across large vocabularies.


**Embeddings in the Pipeline**

How token transform into dense vectors

<b>The Lookup Table</b>

ID 15496 -> [0.12, -0.45, 0.88, ...]

Each token ID acts as an index to a specific row in the embedding matrix. This is simple, fast lockup operation.
The result is a 768-dimensional vector (for base models) that represents the token's core semantic profile.

<b>Information Density</b>

A single vector packs multiple layers of semantic information, capturing nuance that a single integer ID never could.

The "Raw" Starting Point
This vector is the isolated representation of the word. It contains the general meaning of the token before any context from the surrounding sentence is applied.


**Summary: Embeddings**

Converting discrete tokens into meaningful continuous space

Core Functions
- Transform arbitrary integer IDs into points in high-dimensional semantic map.
- Ensure proximity in vector space equals similarity in meaning.
- Pack multiple layers of semantic nuance into a single dense vector.

<b>Foundational Layer</b>
Every transformer starts by looking up these pre-trained meanings. It is the bedrock of all subsequent processing.

<b>The Limitations</b>
Embeddings are 'bag-of-words' by default. They represent isolated meaning but ingonre the order or words.


**The Problem of Sequence**

Attentions is "bag-of-words" by default

<b>Order Neutrality</b>

The dog bit the man     vs      The man bit the dog

Without extra information, attentiona treats these two sentences identically. The mathematical operations don't care about position.

This is known as permutaion invariance. The model sees a set of words, not a sequence.

<b>The Loss of Logic</b>
In human language, the position is often the primary driver of meaning. Order determines who did what to whom.

The Requirement -> We must find a way to inject where a word is into what a word is. We need to make the model aware of the dimension of time in language.


**Positional Encodings**

Adding a sense of time and order to the model

- <b>Injecting Sequence</b> Transformers process token in parallel, so we add tags to show where each token sits in the sequence.
- <b>Vector Addition</b> A small positional vector is added to each embeddings, encoding both meaning and position.
- <b>Spatial Awareness</b> This helps the model tell apart different word orders (e.g. who did what) using position tags.


**How Positional Pattern Works**

Using mathematical waves to encode location

<b>Periodic Functions</b> 