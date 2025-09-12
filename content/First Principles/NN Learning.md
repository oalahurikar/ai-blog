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
