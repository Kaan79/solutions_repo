# Problem 
# **Deriving Kepler's Third Law for Circular Orbits**

## **1. Theoretical Foundation**

Kepler's Third Law states that the square of a planet’s orbital period is proportional to the cube of its orbital radius:

$$
T^2 propto R^3
$$

We derive this relationship using Newton's Law of Gravitation and circular motion dynamics.

### **Newton's Law of Gravitation**
The gravitational force between two masses ( M ) and ( m ) is given by:

$$
F = \frac{G M m}{R^2}
$$

where:
- ( G ) is the gravitational constant,
- ( R ) is the orbital radius.

For a circular orbit, the gravitational force provides the necessary centripetal force:

$$
\frac{G M m}{R^2} = m \frac{v^2}{R}
$$

Canceling ( m ) and solving for orbital velocity ( v ):

$$
v^2 = \frac{G M}{R}
$$

Using the relation between velocity and period (( v = \frac{2\pi R}{T} )):

$$
left(\frac{2pi R}{T}right)^2 = \frac{G M}{R}
$$

Rearranging:

$$
T^2 = \frac{4 pi^2}{G M} R^3
$$

Thus, we confirm that ( T^2 \propto R^3 ).

## **2. Implications for Astronomy**
- Used to estimate planetary masses and distances.
- Helps predict satellite and exoplanet orbits.
- Important in celestial mechanics and space navigation.

## **3. Real-World Examples**
- **Moon's orbit around Earth**: Applying Kepler’s Law allows us to estimate the mass of Earth.
- **Planets in the Solar System**: All planets obey this law, confirming Newtonian gravity.

## **4. Python Simulation**
To verify the relationship, we simulate circular orbits.

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # Gravitational constant (m^3 kg^-1 s^-2)
M = 1.989e30  # Mass of the Sun (kg)

# Define orbital radii (in AU, converted to meters)
au = 1.496e11  # 1 Astronomical Unit in meters
radii = np.array([0.39, 0.72, 1.0, 1.52, 5.2, 9.58, 19.18, 30.07]) * au

# Compute periods using Kepler's Third Law
periods = np.sqrt((4 * np.pi**2 * radii**3) / (G * M))

# Convert periods to years
periods_years = periods / (60 * 60 * 24 * 365)

# Plot
plt.figure(figsize=(8,6))
plt.loglog(radii/au, periods_years, 'bo-', label='Simulated Data')
plt.xlabel('Orbital Radius (AU)')
plt.ylabel('Orbital Period (years)')
plt.title("Kepler's Third Law: T² vs R³")
plt.legend()
plt.grid(True, which='both', linestyle='--')
plt.show()
```
![alt text](image.png)

## **5. Extending to Elliptical Orbits**
- Kepler’s law applies to elliptical orbits by considering the **semi-major axis** instead of the radius.
- Used in astrodynamics to analyze non-circular planetary motion.

## **6. Conclusion**
- **Kepler’s Third Law** is fundamental in celestial mechanics.
- **Observational verification** confirms Newtonian gravity.
- **Extends to exoplanetary systems and astrophysics applications.**

