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


**Why we need Activation Functions**

<b>The Linearity Problem</b>
Layer 1: $y = W_1 x + b_1$
Layer 2: $z = W_2 y + b_2$

<b>Combined Equation</b>
Combined:
$$z = W_2(W_1 x + b_1) + b_2$$
$$z = (W_2 W_1)x + (W_2 b_1 + b_2)$$   

This simplifies to:
$$z = W'x + b'$$
where $ W' = W_2W_1 $ and $ b' = W_2b_1 + b_2 $.

<b>Mathematical Collapse:</b> Stacking linear layers just results in another linear function. 100 layers are equivalent to just one.

<b>The Non-linear solution</b>

<b>Breaking Linearity:</b> Activation functions add "bends" to the math, preventing layers form collapsing into each other.

<b>Enabling Depth:</b> This is the "secret sauce" that allows deep networks to learn complex, multi-layered represantations.

<b>Universal Approximation:</b> Non-linearity allows networks to model any continuous function, no matter how complex.


**The Activation Function Menu**

<b>Sigmoid</b>
Classic S-curve (0 to 1). Historically the default, ideal for probability outputs.

<b>Tanh</b>
Zero-centered (-1 to 1). Often provides faster convergence than sigmoid.

<b>ReLU</b>
The breakthrough: max(0,x). Simple, efficient and enabled deep networks.

<b>Modern Variants</b>
<b>GELU & Swish:</b> Smooth versions of ReLU used in state-of-art LLMs 


**The Loss Fuction**
Loss = A single number measuring how wrong we are.

Lower loss = better predictions
Training goal = minimize loss

Mean Squared Error (MSE):
L = (1/n) summation(prediction - target)2

- Squares make all error positive
- Big errors penalized more than small errors
- Good for regression tasks

Example:
Predicted: $350,000
Actual: $400,000
Error: $50,000
Squared: 2,500,000,000



**Backpropogation: Assigning Blame**

<b>The Chain of Responsibility</b>
<b>The Question:</b> "The prediction was wrong. Which specific weights are responsible for this error?"
<b>The Flow:</b> Error signal travel backward from the output layer through the hidden layers to the input.
<b>Adjustment:</b> Each weight is adjusted proportionally to its contribution to the final mistake.
