# comparison
import math

def compare_root_finding_methods(f, df, a, b, tol=1e-6, max_iter=100):
    """
    Compare the number of steps taken by bisection and Newton-Raphson methods to converge.
    
    Parameters:
    f (function): The function whose root we want to find
    df (function): The derivative of f (for Newton-Raphson)
    a (float): Left endpoint of initial interval (for bisection)
    b (float): Right endpoint of initial interval (for bisection)
    tol (float): Tolerance for convergence
    max_iter (int): Maximum number of iterations allowed
    
    Returns:
    dict: A dictionary containing the number of steps for each method and whether they converged
    """
    
    # Validate initial interval for bisection
    if f(a) * f(b) >= 0:
        raise ValueError("Bisection method requires f(a) and f(b) to have opposite signs")
    
    # Initial guess for Newton-Raphson (midpoint of interval)
    x0 = (a + b) / 2
    
    # Bisection method
    bisection_steps = 0
    a_bisect, b_bisect = a, b
    bisection_converged = False
    
    for bisection_steps in range(1, max_iter + 1):
        c = (a_bisect + b_bisect) / 2
        if abs(f(c)) < tol:
            bisection_converged = True
            break
        
        if f(a_bisect) * f(c) < 0:
            b_bisect = c
        else:
            a_bisect = c
    
    # Newton-Raphson method
    newton_steps = 0
    x = x0
    newton_converged = False
    
    for newton_steps in range(1, max_iter + 1):
        fx = f(x)
        if abs(fx) < tol:
            newton_converged = True
            break
        
        dfx = df(x)
        if dfx == 0:
            break  # derivative is zero, can't continue
        
        x = x - fx / dfx
    
    return {
        'bisection_steps': bisection_steps if bisection_converged else None,
        'newton_steps': newton_steps if newton_converged else None,
        'bisection_converged': bisection_converged,
        'newton_converged': newton_converged
    }


# Example usage:
if __name__ == "__main__":
    # Define a function and its derivative
    def f(x):
        return x**3 - 2*x - 5
    
    def df(x):
        return 3*x**2 - 2
    
    # Compare methods
    result = compare_root_finding_methods(f, df, 1, 3)
    print(f"Bisection steps: {result['bisection_steps']}")
    print(f"Newton-Raphson steps: {result['newton_steps']}")
    print(f"Bisection converged: {result['bisection_converged']}")
    print(f"Newton-Raphson converged: {result['newton_converged']}")
