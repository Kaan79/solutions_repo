# Problem 3
# Trajectories of a Freely Released Payload Near Earth
## 1. Introduction

Understanding how a payload behaves after being released from a moving spacecraft near Earth is a critical aspect of **orbital mechanics**, with direct applications in **satellite deployment**, **reentry trajectory design**, and **interplanetary mission planning**. Once the payload is released, its subsequent trajectory is governed by classical mechanics—primarily Newton's laws of motion and universal gravitation.

The nature of the trajectory depends on:
- The **initial position** and **velocity vector** of the payload at the moment of release,
- The **gravitational field** of Earth, modeled as a central inverse-square force,
- The **mechanical energy** and **angular momentum** of the system.

These factors determine whether the object will:
- Remain in a **circular** or **elliptical orbit**,
- Follow a **parabolic** trajectory, barely escaping Earth’s gravity,
- Move along a **hyperbolic path**, escaping the Earth-Sun system altogether,
- Or fall back to Earth on a **suborbital or ballistic** path.

This document aims to build a complete and hands-on understanding of these orbital behaviors, starting from the physical laws and progressing toward visual simulations using code. The concepts explored here are foundational to:
- Launch vehicle stage separation and satellite injection into orbit,
- Deorbit maneuvers and safe reentry computations,
- Trajectory design for deep-space probes (e.g., Voyager, New Horizons),
- And collision avoidance or decay prediction for space debris.

---

### In this study, we will:

- **Explain** the fundamental physics and mathematical equations that govern gravitational motion and orbital dynamics.
- **Classify** possible trajectories based on the total mechanical energy and angular momentum of the payload.
- **Derive and implement** numerical integration methods (e.g., Euler and Runge-Kutta) to simulate motion under Earth's gravity.
- **Perform Python-based simulations** that allow us to visualize these trajectories under various initial conditions (velocity, altitude, angle).
- **Generate plots and diagrams** using `matplotlib` that are compatible with **Google Colab**, making this project interactive and reproducible for others.
- **Explore real-world mission scenarios**, such as payload release from low-Earth orbit or reentry capsule guidance.

---

This notebook/document is intended as both an educational and practical tool. Whether you're a student of aerospace engineering, a researcher in astrodynamics, or an enthusiast of orbital simulations, this guide will provide a clear pathway from theoretical laws to code-based implementation.

Let’s begin by laying the physical and mathematical foundation for orbital motion in Section 2.


---

## 2. Physical Principles

To analyze the trajectory of a payload released near Earth, we must first understand the fundamental physical laws that govern its motion. These are primarily Newton’s Law of Universal Gravitation and Newton’s Second Law of Motion. Together, they allow us to model the gravitational force acting on the payload and predict its acceleration and motion in space.

---

### 2.1 Newton's Law of Universal Gravitation

Sir Isaac Newton’s law of gravitation describes the attractive force between two point masses:

$$
F = \frac{G M m}{r^2}
$$

Where:
- ( F ): gravitational force (in Newtons, N)
- $$( G = 6.67430 \times 10^{-11} \, \text{m}^3/\text{kg} \cdot \text{s}^2 ):$$
 universal gravitational constant 
- ( M ): mass of the larger body (in this case, Earth)  
  $$( M = 5.972 \times 10^{24} \, \text{kg} )$$
- ( m ): mass of the payload (e.g., a satellite or a capsule)
- ( r ): distance between the **center of Earth** and the payload (in meters)

####  Example:

If a payload is located at an altitude of ( h = 300 \, \text{km} ) above Earth’s surface:

- Radius of Earth: ( R_E = 6.371 \times 10^6 \, \text{m} )
- Total distance from Earth's center:  
 $$( r = R_E + h = (6.371 + 0.300) \times 10^6 = 6.671 \times 10^6 \, \text{m} )$$

Let’s calculate the gravitational force acting on a 100 kg payload:

$$
F = \frac{(6.67430 \times 10^{-11}) \cdot (5.972 \times 10^{24}) \cdot (100)}{(6.671 \times 10^6)^2} \approx 895.6 \, \text{N}
$$

This force pulls the payload toward the center of Earth, and its magnitude decreases with increasing altitude.

---

### 2.2 Newton’s Second Law of Motion

Newton’s Second Law relates the **net force** acting on an object to its **acceleration**:

$$
\vec{F}_{\text{net}} = m \cdot \vec{a}
$$

In our case, the only force acting on the payload (neglecting atmospheric drag) is gravity. So we combine this with the gravitational force equation:

$$
m \cdot \vec{a} = -\frac{G M m}{r^2} \cdot \hat{r}
$$

Where:
- ( vec{a} ): acceleration vector of the payload
- ( hat{r} ): unit vector pointing radially away from Earth's center
- The negative sign indicates that the force (and hence acceleration) is **attractive**, i.e., directed toward Earth

Canceling the mass ( m ) from both sides:

$$
\vec{a} = -\frac{G M}{r^2} \cdot \hat{r}
$$

This is the **acceleration due to gravity** at a distance ( r ) from Earth’s center. Note that this is a **vector** equation — the direction of ( \vec{a} ) always points toward Earth, while its magnitude is:

$$
|\vec{a}| = \frac{G M}{r^2}
$$

---

####  Vector Form in Cartesian Coordinates

In simulations, we often work in 2D (or 3D) Cartesian coordinates. If the position of the payload is given by:

$$
\vec{r} = (x, y)
$$

Then:

- The distance to Earth's center is:  
  $$( r = \sqrt{x^2 + y^2} )$$
- The unit vector pointing toward Earth:  
  $$( \hat{r} = \frac{\vec{r}}{r} = \left( \frac{x}{r}, \frac{y}{r} \right) )$$
- The acceleration components become:

$$
a_x = -\frac{G M}{r^3} \cdot x \\
a_y = -\frac{G M}{r^3} \cdot y
$$

These equations are essential for implementing numerical simulations using Python. By computing the acceleration at each time step, we can update the velocity and position of the payload over time using methods like **Euler** or **Runge-Kutta integration**.

---

###  Summary

- Gravitational force decreases with the square of the distance.
- Acceleration due to gravity also follows an inverse-square law.
- The equations must be written in vector or component form to simulate trajectories in 2D or 3D.
- These principles form the core of orbital mechanics and will guide our numerical simulations in the next sections.

In the next chapter, we will explore **energy-based classification of orbits**, and how total mechanical energy determines the shape of a payload’s trajectory.


## 3. Orbital Energy and Trajectory Types

To understand the motion of a payload released near Earth, it is important to analyze its **total mechanical energy**. This energy determines the **nature of the trajectory** the object will follow under the influence of gravity.

---

### 3.1 Total Mechanical Energy

The total mechanical energy \( E \) of an object in a gravitational field is the sum of its **kinetic** and **gravitational potential** energy:

$$
E = \frac{1}{2}mv^2 - \frac{GMm}{r}
$$

Where:
- \( m \): mass of the payload
- \( v \): speed of the payload at a given moment
- \( G \): universal gravitational constant
- \( M \): mass of the Earth (or central body)
- \( r \): distance from the center of the Earth to the payload

The **kinetic energy** increases with velocity, while the **gravitational potential energy** is always negative (since gravity is an attractive force).

---

### 3.2 Classification of Trajectories by Energy

The **sign of the total energy \( E \)** determines the type of orbit or trajectory:

---

#### 1. **Elliptical Orbit (Bound Orbit)**

$$
E < 0
$$

- The object is **gravitationally bound** to Earth.
- The trajectory is an **ellipse** (including the special case of a **circular orbit**).
- The object will keep orbiting the Earth unless acted upon by another force.

**Special Case — Circular Orbit:**

If the speed is exactly:

$$
v = \sqrt{\frac{GM}{r}}
$$

Then the orbit is circular and:

$$
E = -\frac{GMm}{2r}
$$

---

#### 2. **Parabolic Trajectory (Escape Threshold)**

$$
E = 0
$$

- This represents the **minimum energy** needed to escape Earth’s gravity.
- The trajectory is a **parabola**, and the object just barely escapes without returning.
- The required speed is called the **escape velocity**:

$$
v = \sqrt{\frac{2GM}{r}}
$$

At this speed, the kinetic energy exactly cancels out the gravitational potential energy.

---

#### 🛰 3. **Hyperbolic Trajectory (Unbound Escape)**

$$
E > 0
$$

- The object has **more than enough energy** to escape Earth's gravitational pull.
- The trajectory is a **hyperbola**, and the object will **never return**.
- This is typical for **interplanetary** or **interstellar missions** after planetary flybys (e.g., Voyager 1 and 2).

---

### 3.3 Role of Velocity Vector Direction

Even if two payloads have the **same speed**, their trajectories can differ based on the **direction** of the velocity vector:

- A payload launched **tangentially** is more likely to enter orbit.
- A payload launched **radially outward** may go straight up and fall back down (suborbital).
- A payload launched at an **angle** can follow an elliptical, parabolic, or hyperbolic arc depending on speed and direction.

Therefore, the **magnitude and direction** of the initial velocity vector together determine the final shape of the trajectory.

---

### 3.4 Example: Energy Comparison

Suppose a 100 kg payload is launched from a height of 300 km with speed $$( v = 8,000 \, \text{m/s} )$$:

- $$( r = R_E + 300 \times 10^3 = 6.671 \times 10^6 \, \text{m} )$$
- Calculate energy:

$$
E = \frac{1}{2}(100)(8000)^2 - \frac{(6.674 \times 10^{-11})(5.972 \times 10^{24})(100)}{6.671 \times 10^6}
$$

- Kinetic energy:  
  $$( = 3.2 \times 10^9 \, \text{J} )$$  
- Potential energy:  
  $$( \approx -5.97 \times 10^9 \, \text{J} )$$  
- Total energy:  
  $$( E \approx -2.77 \times 10^9 \, \text{J} \Rightarrow \text{Elliptical orbit} )$$

---

###  Summary Table

| Total Energy ( E ) | Trajectory Type     | Description                            |
|---------------------|----------------------|----------------------------------------|
| ( E < 0 )         | Elliptical orbit     | Bound orbit, object returns            |
| ( E = 0 )         | Parabolic trajectory | Escape with exact required energy      |
| ( E > 0 )         | Hyperbolic trajectory| Unbound escape, object never returns   |

---

In the next section, we will translate these physical principles into **mathematical models** and prepare for **numerical simulation** using Python.

---

## 4. Equations of Motion in 2D

To simulate the motion of a payload released near Earth, we model its dynamics in **two-dimensional Cartesian coordinates**. This allows us to track its position and velocity over time as it moves under the influence of gravity.

We assume:
- The Earth is a **point mass** located at the origin: (0, 0) 
- The payload moves in a **plane**, defined by its position and velocity vectors

---

### 4.1 Position and Velocity Vectors

Let the position and velocity vectors be:

$$
\vec{r}(t) = \begin{bmatrix} x(t) \\ y(t) \end{bmatrix}, \quad
\vec{v}(t) = \begin{bmatrix} v_x(t) \\ v_y(t) \end{bmatrix}
$$

The magnitude of the radial distance from Earth’s center is:

$$
r(t) = \sqrt{x(t)^2 + y(t)^2}
$$

---

### 4.2 Gravitational Acceleration in Vector Form

From Newton's law of universal gravitation and second law of motion:

$$
\vec{a}(t) = -\frac{GM}{r(t)^3} \cdot \vec{r}(t)
$$

Which expands to component form:

$$
a_x(t) = -\frac{GM}{r(t)^3} \cdot x(t) \\
a_y(t) = -\frac{GM}{r(t)^3} \cdot y(t)
$$

These are the second-order differential equations governing the payload's motion in 2D.

---

### 4.3 Full System of Differential Equations

To simulate the system numerically, we convert the second-order equations into a set of **first-order** differential equations by defining the state vector:

$$
\vec{u}(t) = \begin{bmatrix}
x(t) \\
y(t) \\
v_x(t) \\
v_y(t)
\end{bmatrix}
$$

Then, the system becomes:

$$
\frac{d\vec{u}}{dt} =
\begin{bmatrix}
\frac{dx}{dt} = v_x(t) \\
\frac{dy}{dt} = v_y(t) \\
\frac{dv_x}{dt} = -\frac{GM}{r(t)^3} \cdot x(t) \\
\frac{dv_y}{dt} = -\frac{GM}{r(t)^3} \cdot y(t)
\end{bmatrix}
$$

This system can now be solved using numerical integration methods such as:
- **Euler's method**
- **Runge-Kutta (RK4)**
- **SciPy’s `solve_ivp` function**

---

### 4.4 Initial Conditions

To solve the equations, we need to specify the initial state of the payload:

- Initial position:  
  $$
  ( x_0, y_0 )
  $$
- Initial velocity components:  
  $$
  ( v_{x0}, v_{y0} )
  $$

These values will determine the trajectory of the payload (elliptical, hyperbolic, etc.), as previously discussed in Section 3.

---

###  Example Setup

Suppose we release a payload from 300 km above Earth's surface directly eastward:

- Earth's radius: $$( R_E = 6.371 \times 10^6 \, \text{m} )$$
- Altitude: $$( h = 3 \times 10^5 \, \text{m} )$$
- Initial position:  
  $$
  ( x_0 = R_E + h, \quad y_0 = 0 )
  $$
- Initial velocity:  
  $$
  ( v_{x0} = 0, \quad v_{y0} = 7.8 \times 10^3 \, \text{m/s} )
  $$

With this setup, the motion takes place in the $$( x\text{-}y )$$ plane, and we can numerically integrate the equations forward in time to trace the payload’s path.

---

### Summary

- Motion in 2D can be expressed using position, velocity, and acceleration vectors.
- The gravitational force acts radially inward, with a magnitude that decreases as $$( \frac{1}{r^2} ).$$
- Converting to first-order ODEs allows us to use numerical solvers.
- The outcome is fully determined by initial conditions.

In the next section, we will implement this system in **Python** and simulate the resulting trajectories for various initial configurations.

---

## 5. Python Simulation Setup

We will simulate trajectories using numerical integration (Euler or Runge-Kutta methods).


```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Constants
G = 6.67430e-11       # m^3/kg/s^2
M = 5.972e24           # kg (Earth)
R_earth = 6.371e6      # m

# Equations of motion
def equations(t, y):
    x, y_pos, vx, vy = y
    r = np.sqrt(x**2 + y_pos**2)
    ax = -G * M * x / r**3
    ay = -G * M * y_pos / r**3
    return [vx, vy, ax, ay]

# Initial conditions
def simulate_trajectory(x0, y0, vx0, vy0, t_max=10000):
    y0_vector = [x0, y0, vx0, vy0]
    t_eval = np.linspace(0, t_max, 5000)
    sol = solve_ivp(equations, [0, t_max], y0_vector, t_eval=t_eval, rtol=1e-9)
    return sol
```

---

## 6. Sample Trajectories

### 6.1 Circular Orbit

Initial velocity: \( v = \sqrt{GM / r} \)

```python
r0 = R_earth + 300e3  # 300 km altitude
v_circular = np.sqrt(G * M / r0)
sol = simulate_trajectory(r0, 0, 0, v_circular)

plt.plot(sol.y[0]/1e3, sol.y[1]/1e3)
plt.xlabel("x (km)")
plt.ylabel("y (km)")
plt.title("Circular Orbit")
plt.axis("equal")
plt.grid()
plt.show()
```

---

### 6.2 Escape Trajectory

```python
v_escape = np.sqrt(2 * G * M / r0)
sol = simulate_trajectory(r0, 0, 0, v_escape)

plt.plot(sol.y[0]/1e3, sol.y[1]/1e3)
plt.xlabel("x (km)")
plt.ylabel("y (km)")
plt.title("Escape Trajectory")
plt.axis("equal")
plt.grid()
plt.show()
```

---

### 6.3 Elliptical Orbit

Try a velocity less than circular:

```python
v_elliptical = 0.9 * v_circular
sol = simulate_trajectory(r0, 0, 0, v_elliptical)

plt.plot(sol.y[0]/1e3, sol.y[1]/1e3)
plt.xlabel("x (km)")
plt.ylabel("y (km)")
plt.title("Elliptical Orbit")
plt.axis("equal")
plt.grid()
plt.show()
```

---

## 7. Application to Mission Scenarios

Understanding the relationship between velocity, energy, and gravitational interaction is not only important in theory—it is **fundamental in real-world mission design**. From launching satellites to escaping the Solar System, the ability to control and predict trajectories is central to aerospace engineering and astrodynamics.

Here are key mission scenarios where orbital mechanics and trajectory analysis are applied:

---

###  7.1 Orbital Insertion

- When a payload is released at a velocity close to the **first cosmic velocity** $$( v_1 = \sqrt{GM/R} )$$, it enters a **stable circular or elliptical orbit** around Earth.
- This scenario applies to:
  - **Satellites in Low Earth Orbit (LEO)** (e.g., Starlink, ISS)
  - **Medium and High Earth Orbits (MEO, GEO)** (e.g., GPS, communication satellites)
- The orbit shape and altitude are controlled by adjusting the **magnitude and direction** of the velocity vector during release.
- **Launch vehicles** such as Falcon 9 or Ariane 5 are designed to precisely reach the target orbital insertion velocity to minimize fuel use and maximize payload efficiency.

---

### 7.2 Reentry and Deorbit Maneuvers

- Reentry occurs when a spacecraft **reduces its velocity** significantly, causing its trajectory to **intersect with Earth’s surface**.
- This is typically achieved using **retrograde burns** (firing the engine in the opposite direction of motion).
- Applications include:
  - **Crewed missions returning to Earth** (e.g., SpaceX Dragon, Soyuz, Orion)
  - **Deorbiting defunct satellites or space debris**
  - **Reusable launch systems**, such as boosters returning from suborbital flight
- During reentry, atmospheric drag becomes dominant and requires thermal protection systems to shield the vehicle from extreme heating.

---

###  7.3 Escape Missions (Interplanetary & Interstellar)

- If a payload reaches a velocity **greater than the escape velocity** $$( v_2 = \sqrt{2GM/R} )$$, it can leave Earth’s gravitational field and enter a **heliocentric (Sun-centered)** orbit.
- This condition is critical for:
  - **Interplanetary missions** (e.g., Mars rovers, Juno to Jupiter)
  - **Lunar transfer orbits**
  - **Gravity-assist maneuvers**, where planetary flybys are used to gain energy and change trajectory
- A higher speed, exceeding not only Earth's escape velocity but also the **Sun's escape velocity**, can place spacecraft on **interstellar trajectories**.
  - **Voyager 1 and 2** have both achieved this status using gravity assists from outer planets.

---

### 🔁 Summary Table

| Mission Type       | Velocity Condition         | Trajectory Type              | Examples                             |
|--------------------|-----------------------------|-------------------------------|--------------------------------------|
| Orbital Insertion  | $$( v \approx v_1 )$$         | Circular / Elliptical Orbit   | ISS, Starlink, GPS, Hubble           |
| Reentry            | $$( v < v_1 )$$, intersects Earth | Ballistic or Decaying Orbit   | Apollo Return, Soyuz, Dragon Capsule |
| Escape Mission     | $$( v > v_2 )$$               | Hyperbolic / Interplanetary   | Voyager, New Horizons, Artemis       |

---

In all these scenarios, the underlying physics remains the same—the conservation of energy, Newton’s laws, and gravitational interactions. What differs is how engineers **intentionally manipulate initial conditions**, through controlled burns, orientation, and timing, to achieve mission-specific objectives.

In the next section, we will reflect on the importance of these concepts in the broader context of space exploration and education.


## 8. Conclusion

Through simulation and analysis of various initial velocity and position conditions, we have demonstrated how the motion of a freely released payload near Earth can be classified into distinct orbital regimes—**circular, elliptical, parabolic, and hyperbolic**.

These trajectory types are not merely mathematical curiosities; they are the **backbone of modern spaceflight mechanics**. By understanding how gravity, velocity, and position interact, mission designers can:

- Accurately plan **satellite launches and orbits**
- Safely conduct **deorbit and reentry maneuvers**
- Design **escape trajectories** for interplanetary and interstellar missions

Our numerical approach—solving Newton's equations of motion using methods like **Runge-Kutta integration**—provides powerful, flexible tools for simulating complex scenarios. With only a few lines of Python code, we can visualize how tiny changes in speed or altitude drastically alter the long-term path of an object.

Beyond its engineering applications, this study serves as a bridge between **classical physics and computational methods**, offering a hands-on way to explore fundamental laws of motion in a real-world context. Whether for academic research, satellite mission design, or educational purposes, such simulations are invaluable in deepening our understanding of **orbital mechanics**.

As humanity expands its presence in space—building lunar bases, sending humans to Mars, and launching probes to the stars—mastery of these gravitational principles becomes not just useful, but essential.

---
*“To go beyond is as wrong as to fall short — but to calculate the trajectory properly is to reach the stars.”*

---

