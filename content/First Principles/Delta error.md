[[Gradient descent]]
[[Back propagation Calculus]]
[[NN Learning]]

In backpropagation, we define the _error_ at a neuron in layer _l_ as

$$
\delta^l_j = \frac{\partial C}{\partial z^l_j}
$$

where C is the cost function and $z^l_j$ is the weighted input to neuron j in layer l

>So, delta error is not the cost itself. It’s a _local measure_ of how much changing that neuron’s input would change the overall cost. It’s the signal that tells each neuron how “wrong” it was, layer by layer.

 **How is it different from the cost?**
- **Cost** is a _global measure_: e.g., mean squared error or cross-entropy, telling us how far the network’s output is from the target .
- **Delta error** is a _local gradient_: it tells each neuron, “your weighted input contributed this much to the overall cost.”

Think of **cost** as the “final exam grade” (overall performance), while **delta error** is the “personal feedback” each student (neuron) gets on what they need to fix.

**Why is delta error significant?**

- It makes backpropagation possible: instead of recalculating global cost gradients for every weight from scratch, we propagate these local deltas backward efficiently.
- It allows learning to be distributed: each neuron only needs its own delta to update its weights and biases.

Formulas like

$$
\frac{\partial C}{\partial w^l_{jk}} = a^{l-1}_k \cdot \delta^l_j
$$

show that weight updates are simply the input activation times the delta of the output neuron

✅ **In short:**
- **Cost** = overall network error measure.
- **Delta error** = per-neuron gradient of cost w.r.t. weighted input.
- **Significance**: Delta is the bridge between the global cost and local weight updates, making backpropagation efficient and biologically intuitive.




