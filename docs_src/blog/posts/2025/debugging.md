---
slug: pinn-debug
date: 2025-09-01
categories:
  - PINN
--- 

# Why Is My AI Failing at Physics? A Guide to Debugging Physics-Informed Neural Networks

Physics-Informed Neural Networks, or PINNs, represent a fascinating intersection of artificial intelligence and the natural sciences. Imagine you want to model a complex physical system, like the flow of water in a river or the formation of a traffic jam. You might have a few real-world measurements, but not nearly enough to train a traditional AI model. This is where PINNs shine. They are neural networks trained not just on data, but also on the mathematical equations that govern the system (Raissi et al., 2019). By baking the laws of physics directly into the training process, PINNs can often find accurate solutions even with very sparse data.

<!-- more -->

One classic test for these models is the Burgers' equation, a relatively simple formula that describes how waves can steepen and form shocks, much like a wave breaking on a shore (Wight & Zhao, 2020). The challenge gets even harder with the *inverse problem*: what if we have a few measurements of the wave, but we don't know a key physical property, like the water's viscosity? We want the PINN to not only model the wave but also to *discover* the missing viscosity parameter for us.

This is where many practitioners hit a wall. The PINN fails to train, the loss stagnates, and the results are physically nonsensical. This isn't just a random bug; it's often a predictable series of failures. This post will walk you through why your PINN might be failing and provide a clear, actionable toolkit to fix it.

## How a PINN "Learns" Physics

At its heart, a PINN learns by trying to minimize a multi-part objective, or "loss function." Think of it as a final grade composed of scores from different subjects. The network must do well in all of them to pass.

* **Data Fidelity ($\mathcal{L}_{data}$):** This part of the score measures how well the network's prediction, $u(x, t)$, matches the actual, sparse measurements we have. This is the traditional "supervised learning" component.

* **Physics Residual ($\mathcal{L}_{PDE}$):** This is the core of a PINN. It checks how well the network's output satisfies the governing physical law—in our case, the Burgers' equation. The network uses a technique called automatic differentiation to calculate the derivatives from the equation within the network itself, effectively "testing" its own solution against the laws of physics at thousands of random points in space and time (Lu et al., 2021). The 1D viscous Burgers' equation is written as:

$$\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} - \nu \frac{\partial^2 u}{\partial x^2} = 0$$

* **Boundary and Initial Conditions ($\mathcal{L}_{BC}, \mathcal{L}_{IC}$):** These terms ensure the solution respects the problem's constraints, such as the state of the system at the beginning ( $t=0$) and at its spatial edges.

The total loss is a sum of these components. The optimizer's job is to adjust the network's internal parameters (its weights and biases) and the unknown physical parameter ($\nu$, the viscosity) to find the lowest possible total loss.

$$\mathcal{L}_{total} = \mathcal{L}_{data} + \mathcal{L}_{PDE} + \mathcal{L}_{IC} + \mathcal{L}_{BC}$$

## The Cascade of Failure: Why Good Models Go Bad

When a PINN fails on a problem like the Burgers' equation, it's rarely due to a single cause. Instead, it's a chain reaction of interconnected issues.

#### The Root Cause: Spectral Bias

Neural networks have a well-documented "spectral bias." They are inherently good at learning smooth, low-frequency patterns first and struggle immensely with sharp, high-frequency features (Rahaman et al., 2019). The solution to the Burgers' equation often starts as a smooth wave but quickly develops a very sharp "shock front"—a near-discontinuity. This creates a fundamental conflict: the physics of the problem produces exactly the kind of feature that the neural network is naturally bad at learning.

#### The Consequence: Unbalanced and Pathological Gradients

Because the network cannot easily approximate the sharp shock front, the physics loss ($\mathcal{L}_{PDE}$) in that small region of spacetime becomes enormous. This, in turn, creates a massive, unbalanced gradient—the signal that tells the optimizer how to update the network's parameters. The optimizer becomes fixated on this single, huge source of error, effectively ignoring the boundary conditions, the initial conditions, and the rest of the physical domain. The training process becomes "stiff," where different parts of the loss function are evolving at vastly different rates, leading to instability and stagnation (Wang et al., 2021).

#### The Result: Overfitting and Poor Generalization

Faced with this difficult optimization landscape, the network often finds an "escape route." If it can't satisfy the difficult physics of the shock, it might discover that it's easier to reduce the total loss by simply memorizing the few data points it was given. This leads to a classic case of overfitting. The model's output may pass exactly through the data points, but it violates the physical laws everywhere else. The resulting solution is physically meaningless, and the discovered value for the unknown parameter, $\nu$, is completely wrong (Krishnapriyan et al., 2021).

## A Practical Toolkit for Fixing a Failing PINN

Understanding this failure cascade allows us to intervene strategically. The following techniques are designed to break the chain reaction and guide the network toward a physically correct solution.

#### 1. Use a Hybrid Optimization Strategy

This is one of the most impactful changes you can make. Instead of relying on a single optimizer, use a two-phase approach.

* **Phase 1: Adam Optimizer.** Start the training with the Adam optimizer for several thousand iterations. Adam is excellent at navigating the rough, global loss landscape to quickly find a promising region for a good solution.
* **Phase 2: L-BFGS Optimizer.** After Adam has done the initial exploration, switch to a quasi-Newton optimizer like L-BFGS. L-BFGS is much more precise and can rapidly converge to the bottom of the minimum that Adam identified (Raissi et al., 2019). Many reported PINN failures are simply due to omitting this crucial second step.

#### 2. Focus the Network's Attention Where It Matters

A key issue is that the network struggles with specific "stubborn" spots, like the shock front. We can force the network to pay more attention to these difficult areas.

* **Adaptive Resampling (RAR):** Instead of training on a fixed set of random points, periodically pause training, evaluate where the physics loss ($\mathcal{L}_{PDE}$) is highest, and add new training points to those high-error regions. This technique, known as Residual-based Adaptive Refinement (RAR), forces the network to focus its capacity on the parts of the problem it finds most challenging (Lu et al., 2021).
* **Self-Adaptive Weights:** A more advanced and powerful approach is to let the network learn how to balance the loss function on its own. In Self-Adaptive PINNs (SA-PINNs), each individual training point in the loss function is assigned its own trainable weight. The network learns to automatically increase the weights for points where the error is high, creating a dynamic "attention mechanism" that forces it to focus on difficult regions (McClenny & Braga-Neto, 2020). This directly counteracts the problem of unbalanced gradients.

#### 3. Simplify the Problem with Hard Constraints

Often, we ask the network to learn the boundary and initial conditions by penalizing them in the loss function (so-called "soft constraints"). A more effective method is to build these conditions directly into the network's architecture, enforcing them by construction ("hard constraints"). For example, you can use a transformation of the network's output that mathematically guarantees it will satisfy the boundary conditions. This removes the $\mathcal{L}_{BC}$ and $\mathcal{L}_{IC}$ terms from the loss function, simplifying the optimization problem and reducing the potential for competing gradients (Krishnapriyan et al., 2021).

## Your Setup for Success

To put these ideas into practice, you don't need a supercomputer.

* **Hardware:** A single modern consumer or professional GPU (e.g., an NVIDIA RTX series or Titan X) is typically sufficient for problems like the 1D or 2D Burgers' equation (Raissi et al., 2019).
* **Software & Libraries:** The standard environment is Python. You can build PINNs using core deep learning libraries like **PyTorch** or **TensorFlow**. For a more streamlined experience, consider specialized libraries like **DeepXDE**, which is built on top of these backends and provides a high-level API that closely resembles the mathematical formulation of the problem (Lu et al., 2021).
* **Network Architecture:** A good starting point for the Burgers' equation is a standard fully-connected neural network with **4 to 8 hidden layers** and **20 to 50 neurons per layer**. For the activation function, **hyperbolic tangent (tanh)** is often preferred over the more common ReLU, as its smoothness and non-zero derivatives are beneficial for calculating the physics-based loss terms (Wang et al., 2021).

## Conclusion

The failure of a PINN is rarely a mystery. It is often a predictable outcome of the fundamental mismatch between a network's natural tendencies and the complex, sharp features present in many physical systems. But by understanding this cascade of failure—from spectral bias to gradient pathologies to overfitting—we can employ a targeted set of powerful techniques to fix it. By using hybrid optimizers, focusing the network's attention on difficult regions, and simplifying the problem with hard constraints, you can transform a failing model into one that successfully solves the underlying physics and discovers the hidden parameters you seek.

## References

Krishnapriyan, A. S., Gholami, A., Zhe, S., Kirby, R. M., & Mahoney, M. W. (2021). *Characterizing possible failure modes in physics-informed neural networks*. arXiv. [https://arxiv.org/abs/2109.01050](https://arxiv.org/abs/2109.01050)

Lu, L., Meng, X., Mao, Z., & Karniadakis, G. E. (2021). DeepXDE: A deep learning library for solving differential equations. *SIAM Review*, *63*(1), 208–228.

McClenny, L., & Braga-Neto, U. (2020). *Self-adaptive physics-informed neural networks using a soft attention mechanism*. arXiv. [https://arxiv.org/abs/2009.04544](https://arxiv.org/abs/2009.04544)

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hampacher, F., Courville, A., & Bengio, Y. (2019, May). On the spectral bias of neural networks. In *International Conference on Machine Learning* (pp. 5301–5310). PMLR.

Raissi, M., Perdikaris, P., & Karniadakis, G. E. (2019). Physics-informed neural networks: A deep learning framework for solving forward and inverse problems involving nonlinear partial differential equations. *Journal of Computational Physics*, *378*, 686–707.

Wang, S., Teng, Y., & Perdikaris, P. (2021). Understanding and mitigating gradient flow pathologies in physics-informed neural networks. *SIAM Journal on Scientific Computing*, *43*(5), A3055–A3081.

Wight, C., & Zhao, J. (2020). *Solving the Burgers' equation with physics-informed neural networks*. arXiv. [https://arxiv.org/abs/2007.08914](https://arxiv.org/abs/2007.08914)
