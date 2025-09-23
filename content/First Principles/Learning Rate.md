[[Gradient descent]]
In practice, each step will look like −η∇C where the constant η is known as the learning rate. The larger it is, the bigger your steps, which means you might approach the minimum faster, but there’s a risk of overshooting and oscillating a lot around that minimum.

 Our neuron learns by changing the weight and bias at a rate determined by the partial derivatives of the cost function, ∂C/∂w and ∂C/∂b.

So saying "learning is slow" is really the same as saying that those partial derivatives are small. The challenge is to understand why they are small.