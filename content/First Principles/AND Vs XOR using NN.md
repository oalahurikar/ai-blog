#example 
### AND Gate (Linearly Separable):
Teach a neural network to mimic an AND gate:
A single neuron with sigmoid activation creates a linear decision boundary.
`y = σ(w₁x₁ + w₂x₂ + b)`

This creates a straight line that separates the input space into two regions.

```
Input: [0,0] → Output: 0
Input: [0,1] → Output: 0  
Input: [1,0] → Output: 0
Input: [1,1] → Output: 1

x₂
1 | 0  1
  |-------
0 | 0  1
  +--------
  0  1  x₁
```
### XOR Gate (NOT Linearly Separable)

```
Input: [0,0] → Output: 0
Input: [0,1] → Output: 1  
Input: [1,0] → Output: 1
Input: [1,1] → Output: 0

Try to draw ONE line that separates:
- (0,0) and (1,1) → class 0
- (0,1) and (1,0) → class 1
It's impossible! ❌

x₂
1 | 1  0
  |-------
0 | 0  1
  +--------
  0  1  x₁
```

Why XOR requires a hidden layer?

## How Hidden Layer Solves XOR
The hidden layer creates non-linear transformations that make XOR linearly separable in a higher-dimensional space.

```
Hidden Neuron 1: h₁ = σ(w₁₁x₁ + w₁₂x₂ + b₁)  # Detects "at least one is 1"
Hidden Neuron 2: h₂ = σ(w₂₁x₁ + w₂₂x₂ + b₂)  # Detects "both are 1"
```
### The Magic:
- h₁ learns to fire when either input is 1
- h₂ learns to fire when both inputs are 1
- Output combines these to create XOR logic

## Why This Matters for Deep Learning
This is the fundamental principle of deep learning:
1. Shallow networks can only learn linear patterns
2. Hidden layers create non-linear feature transformations
3. Each layer learns increasingly complex patterns

Think of it like feature engineering:
- Single neuron: Can only use raw inputs
- Hidden layer: Creates new "features" (h₁, h₂) from raw inputs
- Output layer: Uses these engineered features to make decisions

The hidden layer is like having a smart assistant who transforms the problem into something easier to solve!