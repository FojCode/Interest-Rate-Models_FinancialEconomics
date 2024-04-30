# Spot rate of a zero coupon bond 
def calculate_spot_rate(bond_price, time_to_maturity):
    """
    Calculate the spot rate of a zero-coupon bond.

    Parameters:
    bond_price (float): The current price of the bond.
    time_to_maturity (float): The time to maturity in years.

    Returns:
    float: The spot rate as a decimal.
    """
    
    face_value = 100
    spot_rate = (face_value / bond_price) ** (1 / time_to_maturity) - 1
    return spot_rate # The formula for the spot rate is: (Face Value / Bond Price)^(1/time_to_maturity) - 1

# Example:
bond_price = 95  # Example bond price in Rand
time_to_maturity = 2  # Time to maturity in years
spot_rate = calculate_spot_rate(bond_price, time_to_maturity)
print(f"The spot rate is: {spot_rate:.2%}")

# instantaneous forward rate of a zero coupon bond 

import math

def calculate_instantaneous_forward_rate(bond_price, time_to_maturity):
    """
    Calculate the instantaneous forward rate of a zero-coupon bond.

    Parameters:
    bond_price (float): The current price of the bond.
    time_to_maturity (float): The time to maturity in years.

    Returns:
    float: The instantaneous forward rate.
    """
    
    if bond_price <= 0:
        raise ValueError("Bond price must be greater than 0") # Ensure bond_price is not zero to avoid division by zero error
    instantaneous_forward_rate = -math.log(bond_price) / time_to_maturity # The formula for the instantaneous forward rate is: -ln(Bond Price) / time_to_maturity
    
    return instantaneous_forward_rate

bond_price = 95  # Example bond price in R
time_to_maturity = 2  # Time to maturity in years
forward_rate = calculate_instantaneous_forward_rate(bond_price, time_to_maturity)
print(f"The instantaneous forward rate is: {forward_rate:.2%}")

# Vasicek model - instantaneous forward rate 

import math

def vasicek_instantaneous_forward_rate(r0, k, theta, sigma, t):
    """
    Calculate the instantaneous forward rate using the Vasicek model.

    Parameters:
    r0 (float): The current short-term interest rate.
    k (float): The speed of reversion to the mean.
    theta (float): The long-term mean rate.
    sigma (float): The volatility of the interest rate.
    t (float): The time to maturity in years.

    Returns:
    float: The instantaneous forward rate.
    """
    b = (1 - math.exp(-k * t)) / k # The Vasicek model formula for the instantaneous forward rate
    forward_rate = r0 * math.exp(-k * t) + theta * (1 - math.exp(-k * t)) - (sigma**2 / (2 * k**2)) * (1 - math.exp(-2 * k * t))
    
    return forward_rate

# Example:
r0 = 0.10  # Current short-term interest rate
k = 0.5    # Speed of reversion
theta = 0.08  # Long-term mean rate
sigma = 0.04  # Volatility
t = 1  # Time to maturity in years

forward_rate = vasicek_instantaneous_forward_rate(r0, k, theta, sigma, t)
print(f"The instantaneous forward rate is: {forward_rate:.2%}")

# Vasicek model - price of a zero-coupon bond
import math

def vasicek_bond_price(r, k, theta, sigma, t):
    """
    Calculate the price of a zero-coupon bond using the Vasicek model.

    Parameters:
    r (float): The current short-term interest rate.
    k (float): The speed of reversion to the mean.
    theta (float): The long-term mean rate.
    sigma (float): The volatility of the interest rate.
    t (float): The time to maturity in years.

    Returns:
    float: The price of the bond.
    """
    
    b_t = (1 - math.exp(-k * t)) / k # Calculate b(t)

    a_t = ((theta - (sigma**2) / (2 * k**2)) * (b_t - t)) - ((sigma**2) / (4 * k)) * b_t**2 # Calculate a(t)
    
    bond_price = math.exp(-(a_t + b_t * r)) # Calculate the bond price
    
    return bond_price

# Example:
r = 0.10  # Current short-term interest rate
k = 0.5    # Speed of reversion
theta = 0.08  # Long-term mean rate
sigma = 0.05  # Volatility
t = 1  # Time to maturity in years

price = vasicek_bond_price(r, k, theta, sigma, t)
print(f"The price of the zero-coupon bond is: ${price:.2f}")
