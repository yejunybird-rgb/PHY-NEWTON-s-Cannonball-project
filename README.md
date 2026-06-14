# The Flight of Newton's Cannonball: From Classical Orbits to Subterranean Transport

This project provides a comprehensive, pedagogical breakdown and numerical verification of the foundational paper **"The flight of Newton’s cannonball"** by W. Dean Pesnell, published in the *American Journal of Physics* (2018). 

## 1. Project Overview
We modernize Isaac Newton’s 300-year-old thought experiment by exploring what happens when a cannonball is fired at a speed that allows it to pierce the Earth's surface rather than just orbiting or crashing. This study transitions from external celestial mechanics to **internal subterranean kinematics**, conceptualizing a high-speed orbital tunnel transport system.

## 2. Key Physics Principles
The project analyzes the "Gravitational Paradigm Shift" that occurs when moving from the Earth's surface to its interior:

*   **Gravitational Force:** Outside the Earth, gravity follows the **Inverse Square Law** ($g \propto 1/r^2$). Inside our idealized homogeneous Earth model, gravity **decreases linearly** with radius ($g \propto r$).
*   **Orbit Types:** Exterior paths are **Standard Keplerian Orbits** (Earth's center is one focal point), while interior paths are **Quasi-Keplerian Orbits** (Earth's center is the absolute center of the ellipse).
*   **The Master Dial ($f$):** We use the **Muzzle Velocity Ratio ($f = v_M^2 / v_c^2$)** to determine the trajectory. Lower velocities lead to direct paths through the core, while moderate velocities create curved paths through the mantle.
*   **Orbital Patching:** We derive the total trajectory by "patching" these two distinct elliptical models using the conservation of specific angular momentum ($L$) and total energy ($E_T$).

## 3. Case Study: The "Burrito Delivery Service"
We verified the proposed delivery route from California to New York (an angular distance of $38^\circ$).
*   **Speed:** Utilizing planetary gravity as the primary propellant, a "burrito" can be delivered across the country in approximately **20 minutes**.
*   **Coriolis Deflection:** Because the Earth rotates during flight, an eastward-moving payload would miss its target by approximately **300 km to the West**. Our findings emphasize that real transport tunnels must be curved or launches must be compensated to account for this rotation.

## 4. Simulation Code
The project includes a Python simulation to visualize trajectories and verify analytical models for travel angle ($\Delta u_s$) and travel time ($\Delta s$).

```python
import numpy as np
import matplotlib.pyplot as plt

# --- System Parameters ---
R_earth = 6371.0  # Earth Radius (km)
a = 0.7           # Normalized radius factor (R/rA)
f = 0.823         # Muzzle velocity ratio (near f_max for 38-degree delivery)

# --- Calculations ---
rA = R_earth / a 
cos_theta0 = (f - a) / (a * (1 - f))
theta0 = np.arccos(cos_theta0)

# Exterior Orbit
theta_ext = np.linspace(np.pi, theta0, 500)
r_ext = (rA * f) / (1 + (1 - f) * np.cos(theta_ext))

# Interior Orbit
term1 = 3 - a * (2 - f)
term2 = np.sqrt(term1**2 - 4 * f / a)
phi_s = 0.5 * np.arccos((term1 - 2 * f / a) / term2)
phi_int = np.linspace(phi_s, np.pi - phi_s, 500)
x_sq = (2 * f / a) / (term1 - term2 * np.cos(2 * phi_int))
r_int = np.sqrt(x_sq) * R_earth

# --- Plotting ---
plt.figure(figsize=(8,8))
earth = plt.Circle((0, 0), R_earth, color='lightgray', label='Earth (Homogeneous)')
plt.gca().add_patch(earth)
plt.plot(r_ext * np.sin(theta_ext), r_ext * np.cos(theta_ext), 'b-', label='Exterior')
plt.plot(r_int * np.sin(phi_int - phi_s + theta0), r_int * np.cos(phi_int - phi_s + theta0), 'r--', label='Interior Tunnel')
plt.gca().set_aspect('equal')
plt.legend()
plt.show()
```

## 5. Theoretical Limitations
Our model relies on the **"Three Pillars of Idealization"**:
1.  **Constant Density:** Assumes a perfectly homogeneous sphere.
2.  **Perfect Vacuum:** Ignores all air resistance and friction.
3.  **Non-Rotating Earth:** Initial calculations assume an inertial frame, requiring massive post-calculation corrections for the Coriolis effect.

## 6. Contributors (Team Resonance)
*   **Do-hyeon Kwon:** Derivations & Mathematical Layout
*   **Yejun Jang:** Computational Modeling & Plots
*   **Minyoung Joo:** Derivations & Mathematical Layout

## 7. References
*   Pesnell, W. D. (2018). “The flight of Newton’s cannonball.” *American Journal of Physics*, 86(5), 338–343.
*   Halliday, D., Resnick, R., & Walker, J. (2014). *Fundamentals of Physics*. 10th Edition, Wiley.
