From Text to Number

**The Character-level Approach**
Idea 1: One number per character

The most basic way to achieve numerical representation is to assign a unique integer to every individual character in the alphabet.

While simple, this approach operates at the smallest possible unit of language, ignoring words and concepts entirely.

**Why Character-level Models Struggle**
The hidden cost of extreme granularity

<b>The Step Proble</b>
A word like "understanding" needs 13 separate steps to process. The model must work many time harder to see one cocept.

<b>Manual Pattern Learning:</b> The model must learn prefixes and roots from individual characters rather than recieving those units directly.
<b>High Cognitive Load:</b> It's like teaching reading by showing letters only, building grammar and meaning becomes much harder.
<b>Extreme Effeciency:</b> Large amounts of data are spent learning basic vocabulary instead of higher-level login.


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

<b>Parameter Bloat</b> Each unique word needs it's own embedding row; huge vocabularies create large memory overhead
<b>The Unknown Proble</b> Fixed vocabulary fail on names, slang and new terms, reducing robustness
<b>Multilingual Scaling</b> Adding languages multiplies vocabulary size, driving up compute and storage costs

**The Subword Solution**
Finding the "GoldiLocks" balance between character and words

<b>Strategic Splitting</b>

- <b>The Middle Ground:</b> Bigger than character(Too granular) but smaller than whole words(Too many)
- <b>Effieciency:</b> Common words stay whole; rare words break into recognizable, meaningful pieces.
- <b>Example: <b>"understanding" -> ["under", "stand", "ing"]
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

"Hello World" -> ["Hello"," world"]
Spaces are often attached to the beginning of the following word, not treated as separate token.

<b>Contraction</b>

"don't" -> ["don","'t"]
Common contractions are split into specific markers the model has learned to associate with negation or possesion.

<b>Complex Words</b>

"artificial intelligence" -> ["art","ificial"," intelligence"]
Long and rare words are broken into subword units that might not always align with linguistic syllables.

<b>Impact: Understanding these splits is crucial for managing context limits and API costs</b>


**Why Tokenization Matters**
The practical implications of how models "see" text

<b>Operation Impact</b>

<b>Context limits: </b> GPT-4's 128k limit refers to tokens, not words. In English, this is roughly 1.3 tokens per word, but code can be much higher.
<b>Financial Costs: </b> API billing is calculated based on token counts, not character counts. Efficient prompting requires understanding token density.

<b>Model Behaviour</b>

<b>Logic Failure: </b> Strange failure in counting letters and reversing words often stem from the model seeing subword units rather than individual characters.
<b>Language Equity: </b> Different languages require different tokens to express the same concept, affecting both cost and performance globally.