## **💡 Mathematical Insight**
Using the chain rule and vectorization, backprop does this efficiently:

- Gradients of cost w.r.t. output layer are calculated.
    
- Then, errors are **propagated backwards**, layer by layer, using matrix operations:
    
    - δ^l = ((W^{l+1})^T δ^{l+1}) ⊙ σ′(z^l)
        
    
- Weight gradients are just:
    
    - ∂C/∂W = a^{l-1} * δ^l

> **Why do you think the errors (δ) are defined in terms of ∂C/∂z rather than ∂C/∂a (activations)?**

> Think from a chain rule and gradient flow perspective. How does that simplify computations?

### 🎯 Goal

Minimize the cost function $C$ by adjusting weights and biases using gradient descent.

---
### 🧪 Given
- Training examples: $x$
- Learning rate: $\eta$
- Cost function: $$ C = \frac{1}{2} \| y - a^L \|^2 $$
- Activation function: $\sigma$
- Derivative: $\sigma'$
- Total layers: $L$
- Mini-batch size: $m$

---

### 🧮 Step-by-Step (Per Example $x$)

#### 1. 🚀 Feedforward

For each layer $$ l = 2, 3, \dots, L $$
$$
z^x_l = W_l a^x_{l-1} + b_l
$$
$$
a^x_l = \sigma(z^x_l)
$$
---
#### 2. 🎯 Output Error (Final Layer)
$$
\delta^x_L = \nabla_a C_x \odot \sigma'(z^x_L)
$$
For quadratic cost:
$$
\nabla_a C_x = a^x_L - y
$$
So,
$$
\delta^x_L = (a^x_L - y) \odot \sigma'(z^x_L)
$$
---
#### 3. 🔁 Backpropagate Error
For $$ l = L-1, L-2, \dots, 2 $$
$$
\delta^x_l = \left( (W^{l+1})^T \delta^x_{l+1} \right) \odot \sigma'(z^x_l)
$$

---
#### 4. 🧷 Gradient Descent Update
For each layer $$ l = L, L-1, \dots, 2 $$
- **Weights**:
$$
W^l \leftarrow W^l - \frac{\eta}{m} \sum_x \delta^x_l (a^x_{l-1})^T
$$

- **Biases**:
$$
b^l \leftarrow b^l - \frac{\eta}{m} \sum_x \delta^x_l
$$

---
### What are 4 fundamental equations?

**(BP1) Error at the output layer**

$$\delta^L = \nabla_a C \odot \sigma’(z^L)$$

- $\delta^L$: error vector at output layer
- $\nabla_a C$: how cost changes w.r.t. output activations
- $\sigma’(z^L)$: slope of activation at weighted input
- Meaning: error = “how wrong the output was” × “how sensitive the neuron is”
---

**(BP2) Error at hidden layers**

$$\delta^l = \big( (W^{l+1})^T \delta^{l+1} \big) \odot \sigma’(z^l)$$
- Pushes the error backward through weights
- Multiplies by local derivative
- Meaning: hidden neurons inherit error signals from later layers, scaled by their influence
---
### **(BP3) Gradient w.r.t. biases**

$$\frac{\partial C}{\partial b^l_j} = \delta^l_j$$

- Each bias learns directly from its neuron’s error
- Meaning: bias update is simply the error itself
---

### **(BP4) Gradient w.r.t. weights**

$$\frac{\partial C}{\partial w^l_{jk}} = a^{l-1}_k \, \delta^l_j$$

- Weight update = input activation × output error
- Meaning: connection strengthens/weakens in proportion to how active the input was and how wrong the output turned out

---

### **🔗 Putting it together**
1. Compute output error (BP1).
2. Propagate error backward (BP2).
3. Use deltas to compute gradients for biases (BP3) and weights (BP4).
4. Update parameters with gradient descent.

---

**Why is learning so slow?**
To understand the origin of the problem, consider that our neuron learns by changing the weight and bias at a rate determined by the partial derivatives of the cost function, ∂C/∂w and ∂C/∂b. So saying "learning is slow" is really the same as saying that those partial derivatives are small. The challenge is to understand why they are small.

When we look closely, we'll discover that the different layers in our deep network are learning at vastly different speeds. In particular, when later layers in the network are learning well, early layers often get stuck during training, learning almost nothing at all.

### The vanishing gradient problem
Neurons in the earlier layers learn much more slowly than neurons in later layers. Its a vanishing gradient problem.
what goes wrong when we try to train a deep network?
The gradient in deep neural networks is _unstable_, tending to either explode or vanish in earlier layers. This instability is a fundamental problem for gradient-based learning in deep neural networks.

### Why later neuron layers learn faster compared to earlier layers, how and why this happens

**Later layers learn faster because they receive a much stronger, cleaner teaching signal (gradient) than earlier layers.**

As the loss is backpropagated through many layers, the signal is repeatedly multiplied by Jacobians and activation derivatives. Those products usually shrink, so by the time the signal reaches the first layers it’s tiny → tiny updates → “stuck” early layers. Meanwhile, layers near the loss get large, well-conditioned gradients and move quickly.

### **Why do early layers learn more slowly than later layers?**

This is the **vanishing gradient problem** :
- In backpropagation, error $\delta$ is propagated backwards using multiplications with derivatives of activation functions.
- If those derivatives are < 1 (e.g., sigmoid’s max slope is 0.25), the signal **shrinks** layer by layer as you go backward.
- By the time it reaches the early layers, the gradient may be almost zero.

👉 Later layers (closer to the output) get “fresher” error signals → **strong gradients, faster learning**.
👉 Earlier layers (closer to input) get “diluted” error signals → **weak gradients, slower learning**.

Imagine a classroom:
- The **teacher (output layer)** gives direct feedback on mistakes. Students sitting **near the teacher (later layers)** get clear feedback, adjust quickly.
- Students sitting **far in the back (earlier layers)** hear muffled, weak instructions. They learn, but much more slowly.

---

How to address the vanishing gradient problem.