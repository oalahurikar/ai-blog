The **softmax function** takes a vector of raw scores (called _logits_, e.g. outputs of the last linear layer of a neural network) and converts them into a **probability distribution**.

$$
\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^K e^{z_j}}, \quad i = 1,2,\dots,K
$$
### Where is softmax applied?
- **Only at the output layer** (in standard feedforward classifiers).
- Earlier layers use other activation functions (ReLU, sigmoid, tanh, etc.), because we don’t want _every hidden neuron’s outputs_ to behave like a probability distribution.
- At the **final layer**, we want to map raw scores → probabilities, because classification = probability assignment.
---
### Why not apply it everywhere?
If you applied softmax at every layer:
- Every hidden layer would force its outputs to sum to 1 → losing expressive power.
- Neurons couldn’t freely learn features, because they’d always be competing to form a probability distribution instead of just passing useful signals.
- Training would be unstable and inefficient.
👉 That’s why softmax is reserved for the **final classification layer**.
### How softmax converts raw score into probability distribution?
Suppose the network outputs arbitrary real numbers: $z=[2.1,  −1.5,  0.3]$
These numbers can be **negative or positive** and have **no constraint** (they don’t add to 1). Clearly, they can’t be treated directly as probabilities.

**Exponentiation**
Softmax first applies the exponential:
$ez=[e2.1,  e−1.5,  e0.3]≈[8.17,  0.22,  1.35]$
- This makes everything **positive** (good: probabilities can’t be negative).
- It also **amplifies differences** — larger logits grow disproportionately bigger.
### Normalization (divide by the sum)
Now divide each term by the total sum:
$$
\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^K e^{z_j}}, \quad i = 1,2,\dots,K
$$
Here, denominator = 8.17+0.22+1.35=9.74

So probabilities are: $[0.84,  0.02,  0.14]$

✅ **Summary intuition**:  
- Raw logits are “scores” saying how strong each class looks.
- Earlier layers are "feature extractors" → they shouldn’t be forced into probability distributions.  
- Final layer = "decision-maker"
- Softmax = "competition game" among output neurons.  
- Exponentiation sharpens competition.
- Normalization turns those sharpened scores into probabilities.
---