---
slug: pinn-burgers
date: 2025-08-30
categories:
  - PINN
---


# Overcoming Generalization Challenges in 2D Burgers' Equation with Physics-Informed Neural Networks

Physics-Informed Neural Networks (PINNs) have emerged as a powerful paradigm in "scientific machine learning," enabling the integration of physical laws, expressed as Partial Differential Equations (PDEs), into neural network training (Wang et al., 2021, p. 1; Lu et al., 2021, p. 208). While PINNs offer a promising avenue for solving complex, non-linear systems like the 2D Burgers' equation, ensuring robust generalization remains a critical challenge, particularly for solutions exhibiting multi-scale features, anisotropic behavior, or in data-poor environments. The 2D Burgers' equation is a fundamental non-linear PDE often used as a benchmark for such challenges (Raissi et al., 2019, p. 686). This post explores several cutting-edge approaches aimed at enhancing PINN generalization capabilities, directly addressing the underlying limitations that hinder their performance.

<!-- more -->

## The Core Problem: Gradient Pathologies and Numerical Stiffness

A significant obstacle to PINN generalization stems from "gradient pathologies," characterized by "unbalanced back-propagated gradients during model training" (Wang et al., 2021, p. 1). This imbalance frequently leads to "numerical stiffness" in the gradient flow dynamics (Wang et al., 2021, p. 1), where gradients from different components of the loss function (e.g., PDE residual versus boundary conditions) can vary by orders of magnitude (Wang et al., 2021, p. 4). This disparity can cause the network to prioritize minimizing the PDE residual within the domain while neglecting crucial boundary or initial conditions, resulting in "erroneous predictions" (Wang et al., 2021, p. 4). For the 2D Burgers' equation, which can involve complex flow patterns and potential shock formation, such stiffness is particularly problematic.

Furthermore, the importance of "sampling collocation points on the performance of PINNs has largely been overlooked" (Daw et al., 2022, p. 1). Failures can arise when the "correct solution must propagate from the initial and/or boundary points to the interior points" (Daw et al., 2022, p. 3). If this propagation is hindered, PINNs can converge to "trivial solutions" (Daw et al., 2022, p. 1), especially in regions with "highly imbalanced PDE residual fields" (Daw et al., 2022, p. 1).

## Promising Strategies for Enhanced Generalization

Recent research has yielded several innovative techniques that directly combat these limitations, leading to substantial improvements in PINN accuracy and generalization.

### Adaptive Causal Sampling and Evolutionary Sampling

The selection and sampling of collocation points are "quite crucial in training PINNs" (Guo et al., 2022, p. 1). A key insight is that "sampling should obey temporal causality" to avoid "sampling confusion and trivial solution of PINNs" (Guo et al., 2022, p. 1, 5). The **Adaptive Causal Sampling Method (ACSM)** introduces temporal causality into adaptive sampling, significantly improving PINN performance with "few collocation points" and "almost no extra computation cost," achieving up to "two orders of magnitude" improvement in prediction accuracy (Guo et al., 2022, p. 1, 5, 33).

Building on this, the **Evolutionary Sampling (Evo)** method and its **Causal Evo extension** directly address "propagation failures" by incrementally accumulating collocation points in regions of high PDE residuals (Daw et al., 2022, p. 1, 4). Causal Evo, specifically designed for time-dependent PDEs, also incorporates a "causal formulation of the PDE loss" (Daw et al., 2022, p. 6), enabling faster convergence to accurate solutions (Daw et al., 2022, p. 8). These methods demonstrate a "two orders of magnitude improvement in sampling efficiency" for scenarios with very few collocation points, making them highly beneficial for high-dimensional problems like the 2D Burgers' equation where sampling efficiency is critical (Daw et al., 2022, p. 8).

### Adaptive Learning Rate Annealing

To counteract the "unbalanced gradients," an **adaptive learning rate annealing algorithm** dynamically adjusts the weights ($\lambda_i$) of different loss terms during training (Wang et al., 2021, p. 1, 10). This algorithm "utilizes gradient statistics" to "balance the interplay between the different terms" in the composite loss function (Wang et al., 2021, p. 1, 10), ensuring that all physical constraints are adequately considered (Wang et al., 2021, p. 11). For the Helmholtz equation, this method improved prediction error by "more than one order of magnitude" (Wang et al., 2021, p. 11). This technique has been shown to "consistently improve the predictive accuracy of physics-informed neural networks by a factor of 50-100x" across various computational physics problems (Wang et al., 2021, p. 1, 13, 22).

### Novel Neural Network Architectures

Architectural innovations also play a crucial role. A **novel neural network architecture** has been proposed that is inherently "more resilient to such gradient pathologies and presents less stiffness in the gradient flow dynamics" (Wang et al., 2021, p. 2). This design incorporates "multiplicative interactions between different input dimensions" and "enhances the hidden states with residual connections," drawing inspiration from attention mechanisms (Wang et al., 2021, p. 12). Such architectures can lead to a "decrease in the magnitude of the leading Hessian eigenvalue" by approximately three times, indicating reduced gradient flow stiffness (Wang et al., 2021, p. 13). This "better capacity to represent complicated functions" is highly advantageous for capturing the detailed dynamics of the 2D Burgers' equation (Wang et al., 2021, p. 14, 15).

### First-Order Physics-Informed Neural Networks (FO-PINNs)

A significant challenge for standard PINNs, especially for parameterized systems, is the "accuracy decreases significantly" and the "soft implementation of boundary conditions" (Gladstone et al., 2023, p. 1). **First-Order Physics-Informed Neural Networks (FO-PINNs)** address this by reformulating higher-order PDEs into first-order systems (Gladstone et al., 2023, p. 1, 5). For a second-order PDE like:

$$ a \frac{\partial^2 u}{\partial x^2} + b \frac{\partial^2 u}{\partial x \partial y} + c \frac{\partial^2 u}{\partial y^2} + d \frac{\partial u}{\partial x} + e \frac{\partial u}{\partial y} + fu = g(x, y) $$

FO-PINNs define first-order derivatives $u_x = \frac{\partial u}{\partial x}$ and $u_y = \frac{\partial u}{\partial y}$ as new output variables, transforming the PDE loss function into a first-order one and introducing compatibility loss terms:

$$ J_{PDE} = \left(a \frac{\partial \hat{u}_x}{\partial x} + b \frac{\partial \hat{u}_y}{\partial y} + c \frac{\partial \hat{u}_x}{\partial y} + d\hat{u}_x + e\hat{u}_y + f\hat{u} - g\right)^2 $$

$$ J_{COMP} = \left(\hat{u}_x - \frac{\partial \hat{u}}{\partial x}\right)^2 + \left(\hat{u}_y - \frac{\partial \hat{u}}{\partial y}\right)^2 $$

$$ J = J_{PDE} + J_{COMP} $$

This approach offers "significantly higher accuracy in solving parameterized systems" and enables the "exact imposition of boundary conditions" through approximate distance functions, overcoming the "exploding Laplacian issue" associated with higher-order derivatives (Gladstone et al., 2023, p. 1, 2, 5). For the 2D Burgers' equation, where second-order diffusion terms are often present, this reformulation could lead to improved accuracy and boundary enforcement.

### Residual-Based Adaptive Refinement (RAR)

For PDEs with "steep gradients," such as the Burgers' equation, simply using randomly distributed residual points may not be efficient (Lu et al., 2021, p. 217). The **Residual-Based Adaptive Refinement (RAR)** method dynamically adds more collocation points "in the locations where the PDE residual... is large" during training (Lu et al., 2021, p. 217). This adaptive strategy significantly improves "training efficiency" (Lu et al., 2021, p. 217). For the 1D Burgers' equation, PINNs with RAR "can capture the discontinuity much better" (Lu et al., 2021, p. 222). The effectiveness of RAR has also been "demonstrated" for the **2D Burgers' equation** (Lu et al., 2021, p. 222, 223), highlighting its utility in accurately resolving sharp fronts or complex features.

### Multi-Fidelity Hierarchical Training and Data Complementarity

For "data-poor environments" and "parametric PINNs," **multi-fidelity hierarchical training** proves beneficial (Hassanaly et al., 2024, p. 7). This method uses solutions from simpler or lower-fidelity models to guide the training of more complex PINNs (Hassanaly et al., 2024, p. 7). This strategy can help mitigate the "curse of dimensionality" when PINNs are applied to parametric variations (Hassanaly et al., 2024, p. 9, 16).

Furthermore, while PINNs are data-efficient, "if data is available, it should be used" (Hassanaly et al., 2024, p. 10). The "physics loss is best used as a complement to data rather than as a complete replacement" (Hassanaly et al., 2024, p. 10). In scenarios with "low-data availability," combining physics and data losses leads to the "highest accuracy throughout the parameter space" (Hassanaly et al., 2024, p. 2, 10, 16). For the 2D Burgers' equation, even sparse data from traditional simulations or experimental observations could greatly assist in achieving more accurate and generalizable solutions.

## Conclusion

The generalization of PINNs for solving challenging non-linear PDEs, such as the 2D Burgers' equation, is continuously improving through targeted methodological advancements. By systematically addressing issues like gradient pathologies, numerical stiffness, and inefficient sampling, a comprehensive framework for more robust PINN training is emerging. The combination of **adaptive and causal sampling methods (ACSM, Evo, Causal Evo)**, **adaptive learning rate annealing**, **novel and resilient neural network architectures**, **first-order reformulations (FO-PINNs)** for improved accuracy and boundary condition imposition, **residual-based adaptive refinement (RAR)**, and the strategic integration of **multi-fidelity hierarchical training and observational data**, offers a powerful toolkit. These approaches are crucial for unlocking the full potential of PINNs in accurately and efficiently solving complex problems in computational fluid dynamics and beyond, paving the way for reliable "scientific machine learning" applications.

## References

Daw, A., Bu, J., Wang, S., Perdikaris, P., & Karpatne, A. (2022). *Rethinking the importance of sampling in physics-informed neural networks*. arXiv preprint arXiv:2207.02338.

Gladstone, R. J., Nabian, M. A., Sukumar, N., Srivastava, A., & Meidani, H. (2023). FO-PINN: A First-Order formulation for Physics-Informed Neural Networks. *arXiv preprint arXiv:2303.15681*.

Guo, J., Wang, H., & Hou, C. (2022). A Novel Adaptive Causal Sampling Method for Physics-Informed Neural Networks. *Global Science Preprint*.

Hassanaly, M., Weddle, P. J., King, R. N., De, S., Doostan, A., Randall, C. R., Dufek, E. J., Colclasure, A. M., & Smith, K. (2024). PINN surrogate of Li-ion battery models for parameter inference. Part II: Regularization and application of the pseudo-2D model. *NREL, University of Colorado Boulder, Northern Arizona University, Idaho National Laboratory (INL)*.

Lu, L., Meng, X., Mao, Z., & Karniadakis, G. E. (2021). DeepXDE: A deep learning library for solving differential equations. *SIAM Review, 63*(1), 208–228.

Raissi, M., Perdikaris, P., & Karniadakis, G. E. (2019). Physics-informed neural networks: A deep learning framework for solving forward and inverse problems involving nonlinear partial differential equations. *Journal of Computational Physics, 378*, 686–707.

Wang, S., Teng, Y., & Perdikaris, P. (2021). *Understanding and mitigating gradient pathologies in physics-informed neural networks*. SIAM Journal on Scientific Computing, 43(5), A3055–A3081.
