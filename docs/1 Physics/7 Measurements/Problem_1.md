# Problem 1

## 📘 Measuring Earth's Gravitational Acceleration with a Pendulum

### 🔬 Objective
The aim is to estimate Earth's gravitational acceleration \( g \) using a simple pendulum and analyze how uncertainties in measurements affect the final result. This classic physics experiment demonstrates the relationship between the pendulum's period and gravitational pull using the formula:

\[
g = \frac{4\pi^2 L}{T^2}
\]

Where:
- \( L \): Length of the pendulum (m)
- \( T \): Period of one complete oscillation (s)

---

### 🧪 Materials & Setup
- **String Length**: ~1.00 m
- **Pendulum Mass**: Small weight (e.g., keys, washers)
- **Timer**: Stopwatch or smartphone timer
- **Length Measurement Tool**: Ruler or tape (typical resolution ±0.5 cm)
- **Setup**: The pendulum is fixed to a sturdy support and released at a small angle (<15°) to ensure simple harmonic motion.

---

### 📏 Measurements
We perform 10 independent trials, each recording the time for 10 oscillations (\( T_{10} \)). Measuring multiple oscillations reduces errors from human reaction time.

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# Measurement Data: 10 trials of 10 oscillations each
T_10_trials = np.array([20.0, 20.1, 19.9, 20.0, 20.2, 20.0, 19.8, 20.1, 20.0, 19.9])
L = 1.00  # Pendulum length in meters
delta_L = 0.005  # Uncertainty in length (e.g., ruler resolution 1 cm / 2 = 0.5 cm = 0.005 m)

# Calculations
n = len(T_10_trials)  # Number of trials (10)
T_10_mean = np.mean(T_10_trials)  # Mean time for 10 oscillations
sigma_T_10 = np.std(T_10_trials, ddof=1)  # Standard deviation
delta_T_10 = sigma_T_10 / np.sqrt(n)  # Uncertainty in mean T_10

T = T_10_mean / 10  # Period of one oscillation
delta_T = delta_T_10 / 10  # Uncertainty in period

g = (4 * np.pi**2 * L) / (T**2)  # Gravitational acceleration
delta_g = g * np.sqrt((delta_L / L)**2 + (2 * delta_T / T)**2)  # Uncertainty in g

# Output Results
print(f"Pendulum Length (L): {L:.2f} ± {delta_L:.3f} m")
print(f"Mean T_10: {T_10_mean:.2f} ± {delta_T_10:.3f} s")
print(f"Standard Deviation (σ_T_10): {sigma_T_10:.3f} s")
print(f"Period (T): {T:.2f} ± {delta_T:.3f} s")
print(f"Gravitational Acceleration (g): {g:.2f} ± {delta_g:.2f} m/s²")

# Markdown Table
data = {
    "Measurement": [f"T_10_{i+1}" for i in range(n)] + ["Mean T_10", "σ_T_10", "L", "ΔL", "T", "ΔT", "g", "Δg"],
    "Value": [f"{t:.1f}" for t in T_10_trials] + [
        f"{T_10_mean:.2f}", f"{sigma_T_10:.3f}", f"{L:.2f}", f"{delta_L:.3f}",
        f"{T:.2f}", f"{delta_T:.3f}", f"{g:.2f}", f"{delta_g:.2f}"
    ],
    "Unit": ["s"]*n + ["s", "s", "m", "m", "s", "s", "m/s²", "m/s²"]
}
df = pd.DataFrame(data)
markdown_table = df.to_markdown(index=False)
print("\nMarkdown Table:\n")
print(markdown_table)

# Plot 1: Time for 10 Oscillations
plt.figure(figsize=(8, 5))
plt.plot(range(1, n+1), T_10_trials, 'bo-', label='T_10 Measurements')
plt.axhline(T_10_mean, color='r', linestyle='--', label=f'Mean T_10 = {T_10_mean:.2f} s')
plt.fill_between(range(1, n+2), T_10_mean - delta_T_10, T_10_mean + delta_T_10, color='r', alpha=0.2, label=f'Uncertainty ±{delta_T_10:.3f} s')
plt.xlabel('Trial Number')
plt.ylabel('Time for 10 Oscillations (s)')
plt.title('Pendulum Oscillation Time Measurements')
plt.legend()
plt.grid(True)
plt.savefig('plot1_T10_trials.png')
plt.close()

# Plot 2: Histogram of T_10 Measurements
plt.figure(figsize=(8, 5))
plt.hist(T_10_trials, bins=5, edgecolor='black', alpha=0.7)
plt.axvline(T_10_mean, color='r', linestyle='--', label=f'Mean T_10 = {T_10_mean:.2f} s')
plt.xlabel('Time for 10 Oscillations (s)')
plt.ylabel('Frequency')
plt.title('Histogram of Pendulum Oscillation Times')
plt.legend()
plt.grid(True)
plt.savefig('plot2_histogram.png')
plt.close()

# Plot 3: Estimated g per Trial
g_per_trial = (4 * np.pi**2 * L) / ((T_10_trials / 10)**2)
plt.figure(figsize=(8, 5))
plt.plot(range(1, n+1), g_per_trial, 'go-', label='g per Trial')
plt.axhline(g, color='r', linestyle='--', label=f'Mean g = {g:.2f} m/s²')
plt.xlabel('Trial Number')
plt.ylabel('Gravitational Acceleration (m/s²)')
plt.title('Estimated g per Trial')
plt.legend()
plt.grid(True)
plt.savefig('plot3_g_per_trial.png')
plt.close()
