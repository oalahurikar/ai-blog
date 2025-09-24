### What is overfitting
**Overfitting** means the model learns not just the underlying “signal” in the training data, but also the random noise or idiosyncrasies. So it does very well on training data, but fails to generalize to new, unseen data.
- **Signal** = the true underlying pattern or relationship in the data.  
    Think of it as the “law of nature” that actually governs how inputs relate to outputs.
- **Noise** = random fluctuations, measurement errors, or quirks in the data that don’t reflect the true pattern.

Imagine you’re measuring the height of a person:
- The **signal** is their true height (say, 175.0 cm).
- The **noise** is what sneaks in if your measuring tape bends, or the person’s shoes add 1 cm, or you misread by a few millimeters.

If you collect many measurements, the true signal is consistent, but the noise scatters around it.

![[Pasted image 20250924114413.png]]
- **Blue curve** = the **signal** (the true underlying law, y=sin⁡(x).
- **Green X’s** = what the signal would be at sampled points, if there were no measurement error.
- **Red dots** = the **noisy observations** we actually see after adding random fluctuations.

👉 Overfitting happens when a model tries too hard to chase those red dots — including their random wiggles — instead of capturing the smoother blue curve.

### How overfitting captures noise?


### How to _identify_ overfitting?

![[Pasted image 20250924125309.png]]
- **Train vs Validation/Test performance gap**
    - Training error or loss is very low (model fits training well)
    - Validation/test error starts to worsen (or doesn’t improve) while training error keeps improving
    - On a plot of loss vs epochs: validation loss bottoms out then begins increasing, while training loss continues downward. [Google for Developers+1](https://developers.google.com/machine-learning/crash-course/overfitting/overfitting?utm_source=chatgpt.com)

- **High variance**    
    - Predictions are unstable: small changes in input cause large changes in output
    - The model is overly sensitive to small fluctuations in input

- **Complexity metrics**    
    - A model with far more parameters relative to data (i.e. high capacity) is more prone to overfitting. [Wikipedia+1](https://en.wikipedia.org/wiki/Overfitting?utm_source=chatgpt.com)
    - If the model’s effective degrees of freedom are too high.

- **Cross‐validation behavior**
    - You might see that folds of cross-validation produce very different models/performance
    - The average validation error is much worse than training error.

- **Unexpected feature importances or coefficients**    
    - The model may latch onto spurious features that make no intuitive sense, especially in small datasets
    - Sometimes looking at weights or feature importances can reveal over reliance.
### Why use validation_data to prevent overfitting rather than test_data?
