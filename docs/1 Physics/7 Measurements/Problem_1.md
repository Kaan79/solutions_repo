# Problem 1
# Measuring Earth's Gravitational Acceleration with a Pendulum

---

##  Motivation

The acceleration $g$ due to gravity is a fundamental constant that influences a wide range of physical phenomena. Measuring $g$ accurately is crucial for:

- Understanding gravitational interactions.
- Designing structures.
- Conducting experiments in various fields.

One classic method for determining $g$ is through the oscillations of a simple pendulum, where the period of oscillation depends on the local gravitational field.

---

## Task

**Objective**:
- Measure the acceleration $g$ due to gravity using a pendulum.
- Analyze uncertainties in the measurements in detail.

This exercise emphasizes:
- Rigorous measurement practices.
- Uncertainty analysis.
- Their role in experimental physics.

---

##  Procedure

### 1️⃣ Materials

- A string (1 or 1.5 meters long).
- A small weight (e.g., bag of coins, sugar, key chain) mounted on the string.
- Stopwatch (or smartphone timer).
- Ruler or measuring tape.

---

### 2️⃣ Setup

- Attach the weight to the string and fix the other end to a sturdy support.
- Measure the length of the pendulum, $L$, from the suspension point to the center of the weight.

#### Uncertainty in Length:
$$ \Delta L = \frac{\text{Ruler Resolution}}{2} $$

---

### 3️⃣ Data Collection

1. Displace the pendulum slightly ($<15^\circ$) and release.
2. Measure the time for 10 full oscillations ($T_{10}$), repeat 10 times.
3. Record all 10 measurements.
4. Calculate:
   - Mean time for 10 oscillations: $\overline{T_{10}}$
   - Standard deviation: $\sigma_T$
5. Determine uncertainty in mean time:
$$ \Delta T_{10} = \frac{\sigma_T}{\sqrt{n}} $$
where $n = 10$.

---

##  Calculations

### 1️⃣ Calculate the Period
Compute the period of one oscillation:
$$ T = \frac{\overline{T_{10}}}{10} $$
Compute its uncertainty:
$$ \Delta T = \frac{\Delta T_{10}}{10} $$

---

### 2️⃣ Determine $g$
From the pendulum formula:
$$ g = \frac{4\pi^2 L}{T^2} $$

---

### 3️⃣ Propagate Uncertainties
Compute the combined uncertainty in $g$:
$$ \Delta g = g \sqrt{ \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \frac{\Delta T}{T} \right)^2 } $$

---

##  Analysis

### 1️⃣ Compare Measured $g$
- Compare measured $g$ with standard value:
$$ g_{\text{standard}} = 9.81 \, m/s^2 $$

### 2️⃣ Discuss:
- Effect of measurement resolution on $\Delta L$.
- Variability in timing and its impact on $\Delta T$.
- Experimental limitations and assumptions.

---

##  Deliverables

1. Tabulated data in Markdown:
   - $L$, $\Delta L$, $T_{10}$ measurements.
   - $\overline{T_{10}}$, $\sigma_T$, $\Delta T$.
   - Calculated $g$ and $\Delta g$.
2. Discussion on sources of uncertainty and their impact on results.

---

## 📊 Example Data Table

| Measurement | Value     | Uncertainty |
|-------------|-----------|-------------|
| $L$         | 1.200 m   | 0.005 m     |
| $\overline{T_{10}}$ | 20.50 s | 0.12 s       |
| $T$         | 2.050 s   | 0.012 s     |
| $g$         | 9.02 m/s² | 0.20 m/s²   |

---

## Detailed Derivation

### Pendulum Motion
The motion of a simple pendulum is governed by:

$$ T = 2\pi \sqrt{\frac{L}{g}} $$

Rearranged to solve for $g$:

$$ g = \frac{4\pi^2 L}{T^2} $$

---

### Error Propagation
For multiplicative and power relationships, relative uncertainties combine as:

$$ \frac{\Delta g}{g} = \sqrt{ \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \frac{\Delta T}{T} \right)^2 } $$

Absolute uncertainty:

$$ \Delta g = g \cdot \frac{\Delta g}{g} $$

---

##  Practical Notes

- Use a light yet dense weight for minimal air resistance.
- Measure $L$ from the suspension point to the center of mass.
- Ensure the angle is small ($<15^\circ$) to maintain simple harmonic motion assumptions.
- Repeat timing multiple times to reduce human reaction time error.
- Average results for reliable values.

---

##  Uncertainty Sources

### Length Measurement ($\Delta L$):
- Instrument resolution.
- Positioning error.

### Time Measurement ($\Delta T$):
- Human reaction time.
- Environmental factors (e.g., air currents).

### Other Factors:
- Small-angle approximation validity.
- Neglecting damping effects.
- Flexibility of the string.

---

##  Conclusion

By following this procedure, you can accurately estimate the local acceleration due to gravity. The accuracy depends on:

- Precise measurements.
- Careful uncertainty analysis.
- Understanding physical limitations of the experimental setup.

---

##  Bonus Equations Reference

### Full Period Formula:
$$ T = 2\pi \sqrt{\frac{L}{g}} $$

### Gravitational Acceleration:
$$ g = \frac{4\pi^2 L}{T^2} $$

### Uncertainty in $g$:
$$ \Delta g = g \sqrt{ \left( \frac{\Delta L}{L} \right)^2 + \left( 2 \frac{\Delta T}{T} \right)^2 } $$

---

## Final Remarks

This exercise connects fundamental physics principles with practical lab techniques. By propagating uncertainties, students learn how experimental errors affect calculated constants.

> **Note**: Ensure all measurements, calculations, and discussions are documented properly in your lab notebook or markdown report.

---

