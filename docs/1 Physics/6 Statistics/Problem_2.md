# Problem 2


# Estimating $\pi$ using Monte Carlo Methods

## Motivation
Monte Carlo simulations are a powerful computational technique to estimate values using randomness. A classic example is estimating $\pi$ through geometric probability.

---

## Part 1: Estimating $\pi$ Using a Circle

---

### Theoretical Foundation

Given a unit circle inscribed inside a square:

- The square has side length $2$, so:
  $$
  A_{square} = (2r)^2 = 2^2 = 4
  $$
- The circle has radius $r = 1$, so:
  $$
  A_{circle} = \pi r^2 = \pi \cdot 1^2 = \pi
  $$
- Therefore, the ratio of the areas is:
  $$
  \frac{A_{circle}}{A_{square}} = \frac{\pi}{4}
  $$

---

This ratio $\frac{\pi}{4}$ represents the **probability** that a random point selected inside the square will fall inside the circle.

- Points inside the circle satisfy:
  $$
  x^2 + y^2 \leq 1
  $$
- Points outside the circle satisfy:
  $$
  x^2 + y^2 > 1
  $$

---

### Estimating $\pi$:

We can estimate $\pi$ by using the ratio of points inside the circle to the total number of points generated.

Let:
- $N$ = total number of points
- $N_{circle}$ = number of points that fall inside the circle

Then:
$$
\frac{N_{circle}}{N} \approx \frac{\pi}{4}
$$

Rearranging to estimate $\pi$:
$$
\pi \approx 4 \times \frac{N_{circle}}{N}
$$

---

### Why Multiply by 4?

We multiply by 4 because:
- The area ratio $\frac{\pi}{4}$ comes from comparing the circle to the square.
- To isolate $\pi$, we multiply both sides by 4.

Thus:
$$
\pi = 4 \times \frac{N_{circle}}{N}
$$

This formula becomes more accurate as $N$ increases, following the **Law of Large Numbers**.

---

### Key Formula (Summary):
$$
\pi \approx 4 \times \frac{\text{Points inside circle}}{\text{Total points}}
$$


### Python Simulation
```python
import numpy as np
import matplotlib.pyplot as plt

def monte_carlo_pi(num_points):
    x = np.random.uniform(-1, 1, num_points)
    y = np.random.uniform(-1, 1, num_points)
    inside = (x**2 + y**2) <= 1
    pi_estimate = 4 * np.sum(inside) / num_points
    return pi_estimate, x, y, inside
```
### Visualization
```python
pi_value, x, y, inside = monte_carlo_pi(10000)
plt.figure(figsize=(6,6))
plt.scatter(x[inside], y[inside], s=1, color='blue', label='Inside Circle')
plt.scatter(x[~inside], y[~inside], s=1, color='red', label='Outside Circle')
plt.gca().add_patch(plt.Circle((0,0), 1, fill=False, color='black'))
plt.title(f'Pi Estimate: {pi_value:.5f}')
plt.axis('square')
plt.legend()
plt.show()
```

### Convergence
```python
sizes = [100, 1000, 10000, 100000, 1000000]
estimates = [monte_carlo_pi(n)[0] for n in sizes]

plt.plot(sizes, estimates, marker='o', linestyle='--')
plt.axhline(np.pi, color='r', linestyle='-')
plt.xscale('log')
plt.xlabel('Samples')
plt.ylabel('Estimated Pi')
plt.title('Convergence of Pi Estimation')
plt.show()
```

---

## Part 2: Buffon's Needle Method

### Theory
Estimate $\pi$ using:
$$
\pi \approx \frac{2 \cdot L \cdot N_{throws}}{D \cdot N_{cross}}
$$

### Python Simulation
```python
def buffon_needle(num_throws, L=1.0, D=2.0):
    crosses = 0
    for _ in range(num_throws):
        dist = np.random.uniform(0, D/2)
        angle = np.random.uniform(0, np.pi/2)
        if dist <= (L/2) * np.sin(angle):
            crosses += 1
    if crosses == 0:
        return np.nan
    return (2 * L * num_throws) / (D * crosses)
```

### Convergence
```python
trials = [100, 1000, 10000, 100000]
estimates_buffon = [buffon_needle(n) for n in trials]

plt.plot(trials, estimates_buffon, marker='x', linestyle='--', color='purple')
plt.axhline(np.pi, color='r', linestyle='-')
plt.xscale('log')
plt.xlabel('Throws')
plt.ylabel('Estimated Pi')
plt.title("Buffon's Needle Convergence")
plt.show()
```

---

## Comparison
| Method | Accuracy | Convergence | Complexity |
|---------|----------|-------------|------------|
| Circle  | High     | Fast        | Simple     |
| Buffon  | Medium   | Slow        | Geometric  |

---

## Google Colab Notes
```python
# %matplotlib inline
# For interactive Colab plots: %matplotlib notebook
```

## References
- Buffon's Needle Problem
- Monte Carlo Methods
- Python Visualization
```
