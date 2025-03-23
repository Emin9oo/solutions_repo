# Problem 2: Investigating the Dynamics of a Forced Damped Pendulum

# Motivation
# The forced damped pendulum is a captivating example of a physical system 
# that showcases complex dynamics arising from the interplay of damping forces, 
# restoring forces, and external periodic forcing. This system transitions from 
# simple harmonic motion to phenomena like resonance, chaos, and quasiperiodicity. 
# These behaviors not only illuminate fundamental principles of physics, but also have 
# applications in engineering and biology, such as energy harvesting, vibration isolation, 
# and the study of rhythmic biological systems.

# By systematically varying parameters such as the amplitude and frequency of the external 
# force, we uncover a wide range of dynamics, including synchronized oscillations, chaotic 
# motion, and resonance phenomena. These insights can guide the design of better mechanical 
# systems, vibration absorbers, and electrical circuits, as well as help us understand 
# complex real-world systems like climate dynamics and biological oscillators.

# Theoretical Foundation

# The Governing Equation
# The forced damped pendulum is governed by the following nonlinear differential equation:
#
# d²θ/dt² + q * dθ/dt + sin(θ) = F * cos(ω * t)
#
# where:
# - θ: Angular displacement (rad)
# - q: Damping coefficient
# - F: Amplitude of the driving force
# - ω: Driving frequency
# - t: Time

# Small-Angle Approximation:
# For small angles, we use the approximation sin(θ) ≈ θ, resulting in a linearized form:
#
# d²θ/dt² + q * dθ/dt + θ = F * cos(ω * t)

# Steady-State Solution:
# The steady-state solution, after transients decay, exhibits oscillations with amplitude A and phase φ:
#
# A = F / √((1 - (ω²))² + (qω)²)
# φ = tan⁻¹(qω / (1 - ω²))

# Resonance Condition:
# Resonance occurs when the driving frequency ω matches the natural frequency of the pendulum. At resonance, 
# the amplitude of oscillations becomes significantly large and is inversely proportional to the damping coefficient.

# Analysis of Dynamics

# Effect of Damping Coefficient:
# - Underdamped (q < 1): The system oscillates with decreasing amplitude.
# - Critically damped (q = 1): The system returns to equilibrium without oscillation in the minimum time.
# - Overdamped (q > 1): The system returns to equilibrium slowly, without oscillating.

# Effect of Driving Amplitude:
# - Small F: Regular, periodic motion.
# - Large F: Can lead to chaotic behavior, especially with low damping.

# Effect of Driving Frequency:
# - At resonance (ω ≈ natural frequency), oscillations reach maximum amplitude.

# Computational Analysis

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

# Visualization Results

# 1. Time Series Plot
plt.figure(figsize=(10, 4))
plt.plot(sol.t, sol.y[0], label='Theta (Angle)')
plt.xlabel('Time')
plt.ylabel('Angle (radians)')
plt.title('Time Series of Forced Damped Pendulum')
plt.legend()
plt.grid()
plt.show()

# 2. Phase Portrait (Angular position vs. Angular velocity)
plt.figure(figsize=(6, 6))
plt.plot(sol.y[0], sol.y[1], label='Phase Space')
plt.xlabel('Theta (Angle)')
plt.ylabel('Angular Velocity')
plt.title('Phase Portrait of Forced Damped Pendulum')
plt.legend()
plt.grid()
plt.show()

# 3. Poincaré Section Analysis
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

# 4. Bifurcation Diagram (Varying F)
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

# Practical Applications

# 1. Energy Harvesting
# - Exploit the resonance phenomenon for energy harvesting by tuning the natural frequency of mechanical systems to match environmental vibrations.

# 2. Structural Engineering
# - Prevent resonance disasters in bridges (e.g., Tacoma Narrows collapse) and design vibration-damping systems for buildings in seismic zones.

# 3. Electrical Circuits
# - Analogous to RLC circuits, where resonance and damping are crucial to the behavior of driven circuits.

# 4. Biological Systems
# - Biological oscillators such as heart rhythms, neural oscillations, and circadian clocks can be modeled as forced damped systems.

# Limitations and Extensions

# Model Limitations:
# - Linear damping assumption: Real-world damping could be nonlinear, especially at high speeds.
# - Rigid pendulum assumption: The pendulum is modeled as a rigid body, neglecting elasticity or flexibility.
# - Constant parameters: Real systems might have time-varying parameters (e.g., changing damping over time).
# - Simplified forcing: The external force is assumed to be purely sinusoidal; real-world forces may have irregular characteristics.

# Potential Extensions:
# - Nonlinear Damping: Including terms like velocity squared (air resistance at higher speeds).
# - Non-periodic Forcing: Exploring the system's response to random, quasi-periodic, or pulsed excitation.
# - Multiple Pendulums: Studying coupled pendulum systems for synchronization, collective dynamics, and energy transfer.
# - Control Strategies: Investigating feedback control to stabilize or suppress chaos in forced damped systems.

# Conclusion
# The forced damped pendulum is a remarkable example of how nonlinear dynamics can emerge from simple mechanical systems. Through this analysis, we observed 
# a wide variety of behaviors, including periodic oscillations, resonance phenomena, and chaotic motion. The results highlight the importance of parameters 
# such as damping, driving force amplitude, and driving frequency in determining the system's dynamics.

# Our computational analysis, along with visualization tools like phase portraits, Poincaré sections, and bifurcation diagrams, has provided deep insights into 
# the behavior of this system. These insights have broad applications in various fields, such as mechanical and structural engineering, energy harvesting, and biology.

