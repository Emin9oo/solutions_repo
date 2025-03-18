# Problem 2
## **Example 1: Approximation of \( e^x \) at \( x = 0 \) (Maclaurin Series)**  
The Taylor series expansion of \( e^x \) around \( x = 0 \) (Maclaurin series) is:

\[
e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \dots
\]

For small \( x \), we can approximate \( e^x \) using a few terms.

### **Python Code for \( e^x \) Approximation**
```python
import math

def taylor_exp(x, n):
    """Approximate e^x using the Taylor series expansion up to n terms."""
    result = sum((x**i) / math.factorial(i) for i in range(n+1))
    return result

# Example: Approximate e^x for x = 1 using 5 terms
x_value = 1
n_terms = 5
approx = taylor_exp(x_value, n_terms)
exact = math.exp(x_value)

print(f"Approximate e^{x_value} using {n_terms} terms: {approx}")
print(f"Exact e^{x_value}: {exact}")
print(f"Error: {abs(approx - exact)}")
```

---

## **Example 2: Approximation of \( \sin(x) \) at \( x = 0 \)**  
The Taylor series for \( \sin(x) \) around \( x = 0 \) (Maclaurin series) is:

\[
\sin(x) = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \dots
\]

### **Python Code for \( \sin(x) \) Approximation**
```python
def taylor_sin(x, n):
    """Approximate sin(x) using the Taylor series expansion up to n terms."""
    result = sum(((-1)**i * x**(2*i+1)) / math.factorial(2*i+1) for i in range(n+1))
    return result

# Example: Approximate sin(x) for x = π/4 using 5 terms
x_value = math.pi / 4
n_terms = 5
approx = taylor_sin(x_value, n_terms)
exact = math.sin(x_value)

print(f"Approximate sin({x_value}) using {n_terms} terms: {approx}")
print(f"Exact sin({x_value}): {exact}")
print(f"Error: {abs(approx - exact)}")
```

---

## **Example 3: Approximation of \( \ln(1+x) \) at \( x = 0 \)**
The Taylor series for \( \ln(1+x) \) around \( x = 0 \) is:

\[
\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \frac{x^4}{4} + \dots
\]

### **Python Code for \( \ln(1+x) \) Approximation**
```python
def taylor_ln1p(x, n):
    """Approximate ln(1+x) using the Taylor series expansion up to n terms."""
    if abs(x) >= 1:
        raise ValueError("Series does not converge for |x| >= 1")
    
    result = sum(((-1)**(i+1) * x**i) / i for i in range(1, n+1))
    return result

# Example: Approximate ln(1+x) for x = 0.5 using 5 terms
x_value = 0.5
n_terms = 5
approx = taylor_ln1p(x_value, n_terms)
exact = math.log(1 + x_value)

print(f"Approximate ln(1+{x_value}) using {n_terms} terms: {approx}")
print(f"Exact ln(1+{x_value}): {exact}")
print(f"Error: {abs(approx - exact)}")
```

---

### **Key Takeaways**
- The more terms used, the more accurate the approximation.
- The error depends on the remainder term.
- Taylor series is useful in numerical computations where exact functions are costly.
