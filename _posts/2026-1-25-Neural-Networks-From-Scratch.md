How machines actually work

**From Rules to Pattern**
<b>Traditional Programming</b>

<b>Rules don't scale:</b> Humans cannot write rules for every possible scenario in complex tasks.
<b>Rigid Logic:</b> Systems fail when encountering data that doesn't fit predefined rules.

<b>Machine Learning</b>
<b>Pattern Discovery:</b> Finding consistent mathematical relationships withing large datasets
<b>The Goal:</b> Understanding the fundamental mechanics of how this discovery happens.

**Machine Learning in one sentence**
Training is simple, iterative loop of guessing and correcting.

1. <b>Start a guess</b> Initialize with random value.
2. <b>Measure error</b> How far is the guess from the truth.
3. <b>Adjust</b> Modify the guess to be sligtly less wrong.
4. <b>Repeat</b> Do this million of times until error is minimized.


**The Neuron: A Tiny Decison Maker**
The building block of AI is simple mathematical function, not biological mystery. It takes numbers in, performs basic arithmetic, and outputs a signal

1. <b>Input</b> Numerical data points representing features.
2. <b>Weights & Summations</b> Importance assigned to each input, combined into single weighted sum.
3. <b>Activation</b> A non-linear function that decides whether the signal should "fire" or be suppressed.


**Anatomy of Neuron**

output = activation($\sum w_i x_i + b$)

<b>Weights (w)</b>  Determine the strength and importance of each input.
<b>Bias (b)</b>  An offset that allows the neuron to shift its decision boundary.
<b>Activation</b>  A non-linear function that decides if the signal should fire.