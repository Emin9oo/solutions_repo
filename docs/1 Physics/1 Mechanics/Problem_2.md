# Problem 2: Investigating the Dynamics of a Forced Damped Pendulum

import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# 1. Motivation: Investigating the Dynamics of a Forced Damped Pendulum
# The forced damped pendulum is an important example of a physical system that demonstrates 
# complex dynamics, which arise from the interplay of damping, restoring forces, and external 
# periodic driving forces. The motion of such a system can display a wide range of behaviors 
# from simple harmonic motion to chaotic dynamics depending on the parameters.

# The pendulum’s behavior is influenced by factors like the damping coefficient, driving force 
# amplitude, and driving frequency. Understanding these behaviors is essential for applications 
# in engineering and physics, such as energy harvesting, mechanical resonance, and vibration isolation.

# 2. Parameters for the forced damped pendulum
q = 0.5    # Damping coefficient
omega_0 = 1.0  # Natural frequency (rad/s)
F = 1.0      # Amplitude of external force
omega = 1.5    # Frequency of external force

# 3. Theoretical Foundation

# The equation of motion for the forced damped pendulum is given by:
# theta''(t) + q * theta'(t) + sin(theta) = F * cos(omega * t)
# where:
# - theta is the angular displacement
# - q is the damping coefficient
# - F is the amplitude of the external force
# - omega is the frequency of the external force

# For small theta, we can linearize the equation by approximating sin(theta) ≈ theta:
# theta''(t) + q * theta'(t) + theta = F * cos(omega * t)

# The solution will have two components: a transient term that decays over time and a steady-state oscillation at frequency omega.

# Resonance occurs when the driving frequency omega matches the natural frequency of the system (omega_0), 
# which is given by the formula:
# omega_0 = sqrt(1 - (q^2 / 4))

# 4. Differential equation for the forced damped pendulum
def forced_damped_pendulum(t, y, q, F, omega):
    theta, omega_theta = y
    dtheta_dt = omega_theta
    domega_theta_dt = -q * omega_theta - np.sin(theta) + F * np.cos(omega * t)
    return [dtheta_dt, domega_theta_dt]

# 5. Time range for simulation
t_span = (0, 100)               # From 0 to 100 seconds
t_eval = np.linspace(0, 100, 10000)  # 10000 points in time for a smooth solution

# 6. Initial conditions: [theta_0, omega_0] (small initial displacement, zero velocity)
y0 = [0.1, 0]

# 7. Solve the ODE using scipy's solve_ivp (Runge-Kutta method)
solution = solve_ivp(forced_damped_pendulum, t_span, y0, t_eval=t_eval, args=(q, F, omega))

# 8. Extract solution
theta = solution.y[0]
omega_theta = solution.y[1]

# 9. Plot the motion of the pendulum (Theta vs Time)
plt.figure(figsize=(10, 6))
plt.plot(solution.t, theta)
plt.title("Forced Damped Pendulum Motion")
plt.xlabel("Time (s)")
plt.ylabel("Angle (theta)")
plt.grid(True)
plt.show()

# 10. Phase plot: Theta vs Omega (Phase Space Plot)
plt.figure(figsize=(10, 6))
plt.plot(theta, omega_theta)
plt.title("Phase Space Plot")
plt.xlabel("Theta (radians)")
plt.ylabel("Angular Velocity (omega)")
plt.grid(True)
plt.show()

# 11. Poincaré Section (crossing at theta = 0)
poincare_crossings = solution.t[1:][(theta[:-1] * theta[1:] < 0)]  # Zero-crossings in theta
poincare_values = omega_theta[:-1][(theta[:-1] * theta[1:] < 0)]  # Corresponding omega values

# 12. Plot the Poincaré section
plt.figure(figsize=(10, 6))
plt.plot(poincare_crossings, poincare_values, 'o', markersize=2)
plt.title("Poincaré Section")
plt.xlabel("Time (s)")
plt.ylabel("Angular Velocity (omega)")
plt.grid(True)
plt.show()

# 13. Bifurcation Diagram (Varying F)
F_values = np.linspace(0.5, 1.5, 50)
bifurcation_theta = []
bifurcation_omega = []

for F_val in F_values:
    sol = solve_ivp(forced_damped_pendulum, t_span, y0, t_eval=t_eval, args=(q, F_val, omega))
    bifurcation_theta.append(sol.y[0][-1000::50])  # Sample last part of the solution
    bifurcation_omega.append(sol.y[1][-1000::50])

# 14. Plot bifurcation diagram
plt.figure(figsize=(10, 6))
for i in range(len(F_values)):
    plt.scatter([F_values[i]] * len(bifurcation_theta[i]), bifurcation_theta[i], s=1, color='black')

plt.xlabel('Driving Force Amplitude F')
plt.ylabel('Theta (Angle)')
plt.title('Bifurcation Diagram of Forced Damped Pendulum')
plt.grid()
plt.show()

# **Explanation of Visualizations:**
# 1. The first plot shows the motion of the pendulum (Theta vs Time), which reveals how the pendulum's displacement changes over time.
# 2. The second plot is the Phase Space Plot (Theta vs Angular Velocity), showing the relationship between the angle and its velocity. This helps in understanding the system's periodic behavior.
# 3. The third plot, the Poincaré Section, displays the system's dynamics in terms of periodic crossings at theta = 0, indicating periodic or chaotic behavior.
# 4. The Bifurcation Diagram shows how the system's dynamics change as the driving frequency (omega) is varied, which can reveal resonance and chaotic regions.

# 15. Practical Applications
# 1. Energy Harvesting: Used in vibrational energy harvesting systems to generate electricity from periodic motion.
# 2. Suspension Bridges: Forced oscillations can lead to resonance, causing catastrophic failures like the Tacoma Narrows Bridge.
# 3. Electrical Circuits: Analogous to driven RLC circuits where the voltage and current oscillate.

# 16. Limitations and Extensions:
# 1. Nonlinear Damping: The model assumes linear damping, but real-world systems may have air resistance and other nonlinear effects.
# 2. Non-Periodic Forcing: External forces may be irregular in real applications, requiring stochastic or chaotic modeling.
