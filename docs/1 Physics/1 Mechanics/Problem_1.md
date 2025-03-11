# Problem 1
import math

def projectile_motion(v0, theta):
    g = 9.81  # Acceleration due to gravity (m/s^2)
    
    # Convert angle to radians
    theta_rad = math.radians(theta)
    
    # Horizontal and vertical components of velocity
    v_x = v0 * math.cos(theta_rad)
    v_y = v0 * math.sin(theta_rad)
    
    # Time to reach max height
    t_max = v_y / g
    
    # Total time of flight
    T = 2 * t_max
    
    # Maximum height
    h_max = (v_y ** 2) / (2 * g)
    
    # Range (horizontal distance)
    R = v_x * T
    
    return {
        "Time to Max Height (s)": t_max,
        "Total Time of Flight (s)": T,
        "Maximum Height (m)": h_max,
        "Range (m)": R
    }

# Example usage
if __name__ == "__main__":
    v0 = float(input("Enter initial velocity (m/s): "))
    theta = float(input("Enter launch angle (degrees): "))
    results = projectile_motion(v0, theta)
    
    for key, value in results.items():
        print(f"{key}: {value:.2f}")
