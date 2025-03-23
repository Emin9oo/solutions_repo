# Problem 2
# Investigating the Dynamics of a Forced Damped Pendulum

## Theoretical Foundation

### Governing Equation:
The equation of motion for a forced damped pendulum is given by:

\[
\frac{d^2\theta}{dt^2} + q \frac{d\theta}{dt} + \sin(\theta) = F \cos(\omega t)
\]

where:
- \( \theta \) is the angular displacement,
- \( q \) is the damping coefficient,
- \( F \) is the driving force amplitude,
- \( \omega \) is the driving frequency,
- \( t \) is time.

### Small-Angle Approximation:
For small \( \theta \), we approximate \( \sin(\theta) \approx \theta \), leading to the linearized equation:

\[
\frac{d^2\theta}{dt^2} + q \frac{d\theta}{dt} + \theta = F \cos(\omega t)
\]

The solution consists of a transient term that decays over time and a steady-state oscillation at the driving frequency \( \omega \). 

### Resonance Condition:
Resonance occurs when the driving frequency \( \omega \) matches the system’s natural frequency \( \omega_0 \), given by:

\[
\omega_0 = \sqrt{1 - \frac{q^2}{4}}
\]

For weak damping (small \( q \)), the system exhibits large oscillations near this frequency.

## Computational Analysis

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Define the forced damped pendulum equation
def forced_damped_pendulum(t, y, q, F, omega):
    theta, omega_dot = y
    dtheta_dt = omega_dot
    domega_dt = -q * omega_dot - np.sin(theta) + F * np.cos(omega * t)
    return [dtheta_dt, domega_dt]

# Parameters
q = 0.5  # Damping coefficient
F = 1.2  # Driving force amplitude
omega = 2/3  # Driving frequency

t_span = (0, 100)
t_eval = np.linspace(0, 100, 10000)

# Initial conditions
y0 = [0.2, 0]

# Solve the differential equation
sol = solve_ivp(forced_damped_pendulum, t_span, y0, t_eval=t_eval, args=(q, F, omega))

# Plot the time series
plt.figure(figsize=(10, 4))
plt.plot(sol.t, sol.y[0], label='Theta (Angle)')
plt.xlabel('Time')
plt.ylabel('Angle (radians)')
plt.title('Time Series of Forced Damped Pendulum')
plt.legend()
plt.grid()
plt.show()

# Phase Portrait
plt.figure(figsize=(6, 6))
plt.plot(sol.y[0], sol.y[1], label='Phase Space')
plt.xlabel('Theta (Angle)')
plt.ylabel('Angular Velocity')
plt.title('Phase Portrait of Forced Damped Pendulum')
plt.legend()
plt.grid()
plt.show()

# Poincaré Section
poincare_times = np.arange(0, 100, 2 * np.pi / omega)
poincare_theta = []
poincare_omega = []

for t_val in poincare_times:
    idx = np.argmin(np.abs(sol.t - t_val))
    poincare_theta.append(sol.y[0][idx])
    poincare_omega.append(sol.y[1][idx])

plt.figure(figsize=(6, 6))
plt.scatter(poincare_theta, poincare_omega, s=10, label='Poincaré Section')
plt.xlabel('Theta (Angle)')
plt.ylabel('Angular Velocity')
plt.title('Poincaré Section of Forced Damped Pendulum')
plt.legend()
plt.grid()
plt.show()

# Bifurcation Diagram (Varying F)
F_values = np.linspace(0.5, 1.5, 50)
bifurcation_theta = []
bifurcation_omega = []

for F_val in F_values:
    sol = solve_ivp(forced_damped_pendulum, t_span, y0, t_eval=t_eval, args=(q, F_val, omega))
    bifurcation_theta.append(sol.y[0][-1000::50])  # Sample last part of the solution
    bifurcation_omega.append(sol.y[1][-1000::50])

plt.figure(figsize=(10, 6))
for i in range(len(F_values)):
    plt.scatter([F_values[i]] * len(bifurcation_theta[i]), bifurcation_theta[i], s=1, color='black')

plt.xlabel('Driving Force Amplitude F')
plt.ylabel('Theta (Angle)')
plt.title('Bifurcation Diagram of Forced Damped Pendulum')
plt.grid()
plt.show()
```

## Practical Applications
1. **Energy Harvesting**: Used in vibrational energy harvesting systems to generate electricity from periodic motion.
2. **Suspension Bridges**: Forced oscillations can lead to resonance, causing catastrophic failures like the Tacoma Narrows Bridge.
3. **Electrical Circuits**: Analogous to driven RLC circuits where the voltage and current oscillate.

## Limitations and Extensions
- **Nonlinear Damping**: The model assumes linear damping, but real-world systems may have air resistance and other nonlinear effects.
- **Non-Periodic Forcing**: External forces may be irregular in real applications, requiring stochastic or chaotic modeling.

This document covers the theoretical background, computational approach, practical implications, and limitations of the forced damped pendulum model.
