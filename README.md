
# -*- coding: utf-8 -*-
"""
Created on Thu Feb 02 16:27:00 2024

@author: Saadc
"""

import math
import sys

def calculate_corrosion_loss(exposure_time, chloride, SO2, TOW, temperature, humidity, material_type):
    if material_type == 'carbon_steel':
        coefficients = [13.4, 0.98, 3800, 0.46, 25, 0.62, 50, 0.34, 0.016, 20]
    elif material_type == 'zinc':
        coefficients = [0.16, 0.36, 3800, 0.24, 25, 0.82, 50, 0.44, 0.05, 20]
    else:
        print("Invalid material type. Exiting.")
        sys.exit()
    # Check conditions for humidity and temperature
    if humidity > 80 and temperature > 0:
        # Extract coefficients for the selected material type
        A, B, C, D, E, F, G, H, J, T0 = coefficients

        # Calculate constants
        temp_factor = math.exp(J * (temperature + T0))
        time_factor = A * (exposure_time ** B)
        tow_factor = (TOW / C) ** D
        so2_factor = ((1 + SO2) / E) ** F
        cl_factor = ((1 + chloride) / G) ** H

        # Calculate corrosion loss (K)
        corrosion_loss = time_factor * tow_factor * so2_factor * cl_factor * temp_factor

        return corrosion_loss
    else:
        return 0  # Corrosion loss is 0 if conditions are not met
    


def select_option():
    menu_options = ['1. Fragility Code for poles', '2. Corrosion', '3. Help (Assistance on how the code functions)', '4. Exit']
    print("Select an option:")
    for option in menu_options:
        print(option)
    choice = input("Enter your choice: ")
    if choice == '1':
        # Add your code for power poles here
        print(" Fragility Code for poles selected ")
    elif choice == '2':
        # Call the corrosion function
        corrosion1()
        sys.exit()
    elif choice == '3':
        print()
        print("If you select option 1, you'll have access to a range of fragility curves for different types of power poles, allowing you to choose the most suitable curve for your study case.You can also choose by spesification if the pole name is unkown, inserting all the pole spesifications the pole name will appear and the code will automatically continue to powerpoles .Upon selection, you'll receive the corresponding probability of failure (Y) at a specified  hazard intensity (x). The unit for hazards are: for wind speed it is m/s usually the range is from (20 m/s to 80 m/s), for earthquake it is Spectral acceleration and the unit is gravity (g) with a range usually from 0 to 3), for the tsunami hazard it is inundation depth and the unit is in meters m with a range of (0 to 14) ")
        print()
        print("If you select option 2, you will be able to calculate corrosion for two materials based on different parameters. You can change the parameters , but the coefficient are fixed for each material.")
        print()
        print("If you select option 3, you'll receive assistance on how the code functions.")
        print()
        print("If you choose 4 you will exit from the code.")
        print()
        sys.exit()
    elif choice == '4':
        print("Exiting the code.")
        sys.exit()
    else:
        print("Invalid choice. Please select 1, 2, 3, or 4.")
        select_option()
        

def corrosion1():
    # User input for material type
    material_type = input("Enter the material type (carbon_steel or zinc): ")

    # Input parameter values (you can prompt the user for these values) 
    exposure_time = 8 # years
    chloride = 2500  # mg·m⁻²·day⁻¹
    SO2 = 150  # μg/m³
    TOW = 3000  # h/year
    temperature = 4  # °C
    humidity = 85  # %

    # Calculate corrosion loss
    corrosion_loss = calculate_corrosion_loss(exposure_time, chloride, SO2, TOW, temperature, humidity, material_type)

    # Print result
    if corrosion_loss > 0:
        print(f"Corrosion Loss (K) for {material_type}: {corrosion_loss} μm")
        print(f"Corrosion Loss (K) for {material_type}: {corrosion_loss / 1000} mm")
    else:
        print("Conditions not met. Corrosion loss is 0.")


# Call select_option function
select_option()





import numpy as np
from builtins import input

# Define the logistic function
def logistic_function(x, a, k, x0):
    return a / (1 + np.exp(-k * (x - x0)))

# List of parameter sets
parameter_sets = [
    
    
    {'a': 0.986, 'k': 0.506, 'x0': 30.883, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 0, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.983, 'k': 0.527, 'x0': 29.525, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 5, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.995, 'k': 0.562, 'x0': 26.255, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 10, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.997, 'k': 0.707, 'x0': 22.047, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 15, 'wind_degree': 0, "Condition": "Failure"},
    
    {'a': 0.983, 'k': 0.586, 'x0': 27.195, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 0, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.993, 'k': 0.597, 'x0': 26.079, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 5, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.989, 'k': 0.678, 'x0': 23.146, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 10, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.993, 'k': 0.911, 'x0': 17.717, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 15, 'wind_degree': 0, "Condition": "Failure"},
    
    {'a': 0.997, 'k': 0.509, 'x0': 29.904, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 0, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.995, 'k': 0.605, 'x0': 25.235, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 5, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.995, 'k': 0.684, 'x0': 21.954, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 10, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 1.0,   'k': 0.764, 'x0': 19.922, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 15, 'wind_degree': 90, "Condition": "Failure"},
    
    {'a': 0.992, 'k': 0.556, 'x0': 27.443, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 0, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.995, 'k': 0.703, 'x0': 22.192, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 5, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.999, 'k': 0.773, 'x0': 20.109, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 10, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.993, 'k': 0.903, 'x0': 17.208, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Wind', 'corrosion': True, 'ice_thickness_mm': 15, 'wind_degree': 90, "Condition": "Failure"},
    
    {'a': 0.987, 'k': 0.362, 'x0': 41.071, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 0, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.995, 'k': 0.411, 'x0': 36.035, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 5, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.991, 'k': 0.471, 'x0': 31.369, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 10, 'wind_degree': 0, "Condition": "Failure"},
    {'a': 0.988, 'k': 0.422, 'x0': 28.294, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 15, 'wind_degree': 0, "Condition": "Failure"},
    
    {'a': 0.991, 'k': 0.249, 'x0': 62.417, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 0, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.992, 'k': 0.279, 'x0': 54.967, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 5, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.991, 'k': 0.328, 'x0': 48.175, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 10, 'wind_degree': 90, "Condition": "Failure"},
    {'a': 0.998, 'k': 0.359, 'x0': 43.811, 'Voltage_class': 'High Voltage', 'Pole_type': 'Deadend', 'Hazard_type': 'Wind', 'corrosion': False, 'ice_thickness_mm': 15, 'wind_degree': 90, "Condition": "Failure"},
    
    {'a': 0.951, 'k': 7.569, 'x0':0.6007, 'Voltage_class': 'High Voltage', 'Pole_type': 'Danube', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0, 'wind_degree': 90, "Condition": "Failure"},

    {'a': 0.972, 'k': 4.042, 'x0': 0.327, 'Voltage_class': 'High Voltage', 'Pole_type': 'Concrete_high', 'Hazard_type': 'Tsunami',  'ice_thickness_mm': 0, "Condition": "Damage"}, 
    {'a': 0.774, 'k': 0.749, 'x0': 2.073, 'Voltage_class': 'High Voltage', 'Pole_type': 'Concrete_high', 'Hazard_type': 'Tsunami',  'ice_thickness_mm': 0,  "Condition": "Failure"},
    
    
    
    
    {'a': 0.946, 'k': 0.988, 'x0': 2.285, 'Voltage_class': 'High Voltage', 'Pole_type': 'Steel', 'Hazard_type': 'Tsunami',  'ice_thickness_mm': 0,  "Condition": "Failure"},
    {'a': 0.966, 'k': 2.295, 'x0':0.955 , 'Voltage_class': 'High Voltage', 'Pole_type': 'Steel', 'Hazard_type': 'Tsunami',  'ice_thickness_mm': 0,  "Condition": "Damage"},
    {'a': 1.00 , 'k': 0.220, 'x0': 58.676, 'Voltage_class': 'High Voltage', 'Pole_type': 'Steel', 'Hazard_type': 'Wind',  'ice_thickness_mm': 0,  "Condition": "Failure"},
    
    {'a': 0.993, 'k': 0.254, 'x0': 58.248, 'Voltage_class': 'High Voltage', 'Pole_type': 'Wood', 'Hazard_type': 'Wind',  'ice_thickness_mm': 0,  "Condition": "Failure"},
    
    {'a': 0.633, 'k': 3.691, 'x0': 1.393, 'Voltage_class': 'High Voltage', 'Pole_type': 'Tubular_steel', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0,  "Condition": "Failure"},
    
    {'a': 0.947, 'k': 8.625, 'x0': 0.5666, 'Voltage_class': 'Low Voltage', 'Pole_type': 'Concrete_low', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0, "Height": 9 , "Condition": "Failure"},
    {'a': 0.948, 'k': 8.075, 'x0': 0.446, 'Voltage_class': 'Low Voltage', 'Pole_type': 'Concrete_low', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0, "Height": 9 , "Condition": "Damage"},
    {'a': 0.960, 'k': 10.927, 'x0': 0.462, 'Voltage_class': 'Low Voltage', 'Pole_type': 'Concrete_low', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0, "Height": 12 ,  "Condition": "Failure"},
    {'a': 0.940, 'k': 10.471, 'x0': 0.333, 'Voltage_class': 'Low Voltage', 'Pole_type': 'Concrete_low', 'Hazard_type': 'Earthquake',  'ice_thickness_mm': 0, "Height": 12 , "Condition": "Damage"},

    

]



def display_menu(options, prompt):
    print(f"{prompt}:")
    for i, option in enumerate(options, start=1):
        print(f"{i}. {option}")
    choice = int(input("Enter the corresponding number or type: "))
    return choice

        

def choose_pole_type():
    pole_type_options = ['Power Poles', 'Telecommunication Pole']
    pole_type_choice = display_menu(pole_type_options, "Choose a pole type:")
    
    if pole_type_choice == 1:
        return "Power Poles"
    elif pole_type_choice == 2:
        print("You selected Telecommunication Pole.")
        hazard_options = ['Wind']  # Add more hazard options if needed
        hazard_choice = display_menu(hazard_options, "Choose a hazard:")
        
        if hazard_choice == 1:  # If wind is chosen
            x_values = []  # List to store X values
            
            # Parameters for the logistic function
            a = 0.989
            k = 0.282
            x0 = 55.396
            
            while True:
                x_input = input("Enter the value of X (type 'exit' to stop): ")
                
                if x_input.lower() == 'exit':
                    break
                
                try:
                    x_value = float(x_input)
                    x_values.append(x_value)
                except ValueError:
                    print("Invalid input. Please enter a valid number or 'exit' to stop.")
            
            print("Parameters:")
            print(f"a: {a}, x0: {x0}, k: {k}")
            
            # Calculate and print the logistic function result for each X value
            for x in x_values:
                logistic_result = calculate_logistic_function(a, x, x0, k)
                print(f"X value: {x}, Logistic Function Result: {logistic_result}")
            
            raise SystemExit  # Exit the code
        elif hazard_choice == 2:
            print("You selected 'Other Hazard'. Implement logic accordingly.")
            # Implement logic for other hazards if needed
            return None
        else:
            print("Invalid hazard choice.")
            return None
    else:
        print("Invalid choice.")
        return None

def calculate_logistic_function(a, x, x0, k):
    return a / (1 + math.exp(-k * (x - x0)))







def choose_by_specifications():
    specifications = {
        "DANUBE": {"voltage": "high", "min_mass": 10, "max_mass": 17, "min_height": 30, "max_height": 50, "material": ["steel S355J2", "steel S355J2"], "shape": "L-shape"},
        "DEADEND": {"voltage": "high", "min_mass": 10, "max_mass": 56, "min_height": 30, "max_height": 50, "material": ["steel S355J2", "steel S460M"], "shape": "L-shape"},
        "WOOD_POLE": {"voltage": "high", "min_mass": 1.4, "max_mass": 1.5, "min_height": 10, "max_height": 19, "material": ["wood", "redwood"], "shape": "cylindrical"},
        "STEEL": {"voltage": "high", "min_mass": 10, "max_mass": 50, "min_height": 30, "max_height": 50, "material": ["steel", "carbon steel"], "shape": "L-shape"},
        "CONCRETE_HIGH": {"voltage": "high", "min_mass": 1.5, "max_mass": 2.75, "min_height": 15, "max_height": 19, "material": ["reinforced concrete", "plain concrete"], "shape": "tapered"},
        "TUBULAR_STEEL": {"voltage": "high", "min_mass": 1, "max_mass": 8.0, "min_height": 15, "max_height": 19, "material": ["steel Q345B", "steel Q235B"], "shape": "tubular"},
        "CONCRETE_LOW": {"voltage": "low", "min_mass": 0.8, "max_mass": 1, "min_height": 9, "max_height": 15, "material": ["reinforced concrete", "plain concrete"], "shape": "H-type"},
        "TELECOMMUNICATION": {"voltage": "null", "min_mass": 5, "max_mass": 17, "min_height": 40, "max_height": 51, "material": ["steel S235 " , "steel S235"], "shape": "L-shape"}
    }
    
    matching_poles = []  # Initialize the list to collect matching poles
    
    # Average values for DANUBE and DEADEND
    danube_avg_mass = 13.5  # tons
    danube_avg_height = 40  # meters
    deadend_avg_mass = 33  # tons
    deadend_avg_height = 40  # meters

    # Get user input for specifications
    voltage = input("Enter the voltage class of the pole (if known): ").lower()
    min_mass = float(input("Enter the mass in tons of the pole (if known): "))
    max_mass = float(input("Enter the height in meters of the pole (if known): "))
    material = input("Enter the material used for the pole (if known): ")
    shape = input("Enter the shape of material pole (if known): ")

    # Iterate through specifications
    lowest_diff = float('inf')  # Initialize lowest difference
    chosen_pole = None  # Initialize chosen pole
    for pole_type, specs in specifications.items():
        if (
            specs['voltage'] == voltage and
            specs['min_mass'] <= min_mass <= specs['max_mass'] and
            specs['min_height'] <= max_mass <= specs['max_height'] and
            material in specs['material'] and
            specs['shape'] == shape
        ):
            # Calculate percentage difference for DANUBE and DEADEND
            if pole_type in ["DANUBE", "DEADEND"]:
                avg_mass = danube_avg_mass if pole_type == "DANUBE" else deadend_avg_mass
                avg_height = danube_avg_height if pole_type == "DANUBE" else deadend_avg_height
                mass_diff = abs((min_mass - avg_mass) / avg_mass * 100)
                height_diff = abs((max_mass - avg_height) / avg_height * 100)
                print(f"{pole_type}: Mass Difference: {mass_diff:.2f}%, Height Difference: {height_diff:.2f}%")
                
                # Check if current pole has lower difference
                if mass_diff + height_diff < lowest_diff:
                    lowest_diff = mass_diff + height_diff
                    chosen_pole = pole_type
            else:
                # Collect matching poles
                matching_poles.append(pole_type)

    print("Matching poles:", matching_poles)
    if chosen_pole:
        print(f"Chosen pole with lowest difference: {chosen_pole}")
    else:
        print("No other suitable pole found.")
    sys.exit()



menu_options = ['Poles', 'Choose by Specifications']
menu_choice = display_menu(menu_options, "Choose a database:")

if menu_choice == 1:
    selected_pole_type = choose_pole_type()
    if selected_pole_type:
        print("You chose:", selected_pole_type)
elif menu_choice == 2:
    choose_by_specifications()
else:
    print("Invalid choice.")





# Ask user for input
Voltage_class_menu_options = ['High Voltage', 'Low Voltage']
Voltage_class_choice = display_menu(Voltage_class_menu_options, "Choose Voltage Class")
Voltage_class = Voltage_class_menu_options[Voltage_class_choice - 1]

# Filter Pole_type_menu_options based on the selected Voltage_class
if Voltage_class == 'High Voltage':
    Pole_type_menu_options = ['Danube', 'Deadend', 'Wood', 'Tubular_steel' , "Steel" , "Concrete_high"]
else:  # For Low Voltage
    Pole_type_menu_options = ['Concrete_low']

Pole_type_choice = display_menu(Pole_type_menu_options, "Choose Pole Type")
Pole_type = Pole_type_menu_options[Pole_type_choice - 1]




# Adjust Hazard_type_menu_options based on Pole_type
if Pole_type in ['Danube']:
    Hazard_type_menu_options = ['Wind', 'Earthquake']
elif Pole_type == 'Concrete_high':
    Hazard_type_menu_options = ['Tsunami']
elif Pole_type in  ['Concrete_low','Tubular_steel'] :
    Hazard_type_menu_options = ['Earthquake']
elif Pole_type in ['Wood', "Deadend"]:
    Hazard_type_menu_options = ['Wind']
elif Pole_type == "Steel":
    Hazard_type_menu_options = ['Wind',"Tsunami"]

else:
    Hazard_type_menu_options = []

Hazard_type_choice = display_menu(Hazard_type_menu_options, "Choose Hazard Type")
Hazard_type = Hazard_type_menu_options[Hazard_type_choice - 1] if Hazard_type_menu_options else None

if Hazard_type == 'Wind' and (Pole_type in [ 'Danube', 'Deadend']):
    # Allow choosing wind direction for 'Steel', 'Wood', 'Deadend' poles when the Hazard_type is 'Wind'
    wind_direction_menu_options = ['0', '90']
    wind_direction_choice = display_menu(wind_direction_menu_options, "Choose Wind Direction")
    wind_degree = int(wind_direction_menu_options[wind_direction_choice - 1])
else:
    wind_degree = None

# Display corrosion menu
def handle_danube_parameters():
    corrosion_menu_options = ['True', 'False']
    corrosion_choice = display_menu(corrosion_menu_options, "Choose Corrosion")
    corrosion = corrosion_menu_options[corrosion_choice - 1] == 'True'

    ice_thickness_mm = int(input("Enter ice_thickness_mm: "))
    return corrosion, ice_thickness_mm

def handle_deadend_parameters():
    ice_thickness_mm = int(input("Enter ice_thickness_mm: "))
    return False, ice_thickness_mm  # Set corrosion to False for Deadend poles

# Main code
if Hazard_type == "Wind" and Pole_type in ["Danube", "Deadend"]:
    if Pole_type == "Danube":
        corrosion, ice_thickness_mm = handle_danube_parameters()
    elif Pole_type == "Deadend":
        corrosion, ice_thickness_mm = handle_deadend_parameters()
else:
    corrosion = False  # Provide a default value or prompt the user for corrosion if needed
    ice_thickness_mm = None




if Pole_type == 'Concrete_low':
    height_menu_options = [9, 12]
    height_choice = display_menu(height_menu_options, "Choose Height")
    height = height_menu_options[height_choice - 1]
else:
    height = None



# Initialize values to be used later
a, k, x0 = None, None, None


# Add 'Condition' key to dictionaries if not present
for parameter_set in parameter_sets:
    parameter_set.setdefault('Condition', None)

# Determine tower condition options based on pole type
if Pole_type in ["Wood", "Deadend", "Danube", "Tubular_steel"]:
    Condition_menu_options = ["Failure"]
elif Pole_type == "Steel" and Hazard_type == "Wind":
    Condition_menu_options = ["Failure"]
elif Pole_type == "Steel" and Hazard_type == "Tsunami":
    Condition_menu_options = ["Failure", "Damage"]
    
else:
    Condition_menu_options = ['Failure', 'Damage']

# Ask user for tower condition input
Condition_choice = display_menu(Condition_menu_options, "Choose Tower Condition")
Condition = Condition_menu_options[Condition_choice - 1]



# Iterate through parameter sets
for parameter_set in parameter_sets:
    # Check if the parameters match, including the voltage class
    if (
        Pole_type in ["Danube", "Deadend"] and
        Hazard_type in ["Wind", "Earthquake"] and
        parameter_set['Voltage_class'] == Voltage_class and
        parameter_set['Pole_type'] == Pole_type and
        parameter_set['Hazard_type'] == Hazard_type and
        parameter_set['Condition'] == Condition and
        parameter_set.get('corrosion', False) == corrosion and
        ((Hazard_type == 'Wind' and parameter_set.get('ice_thickness_mm', None) == ice_thickness_mm) or Hazard_type == 'Earthquake') and
        (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
    ):
        # Retrieve the values
        a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

        # Print the values
        print(f"a: {a}, k: {k}, x0: {x0}")


for parameter_set in parameter_sets:
        # Check if the parameters match, including the voltage class
        if (
            Pole_type in ["Steel", "Wood"] and
            Hazard_type == "Wind" and
            parameter_set['Voltage_class'] == Voltage_class and
            parameter_set['Pole_type'] == Pole_type and
            parameter_set['Hazard_type'] == Hazard_type and
            parameter_set.get('corrosion', False) == corrosion and
            parameter_set['Condition'] == Condition and
            (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
        ):
            # Retrieve the values
            a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

            # Print the values
            print(f"a: {a}, k: {k}, x0: {x0}")

for parameter_set in parameter_sets:
        # Check if the parameters match, including the voltage class
        if (
            Pole_type == "Steel" and
            Hazard_type == "Tsunami" and
            parameter_set['Voltage_class'] == Voltage_class and
            parameter_set['Pole_type'] == Pole_type and
            parameter_set['Hazard_type'] == Hazard_type and
            parameter_set.get('corrosion', False) == corrosion and
            parameter_set['Condition'] == Condition and
            (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
        ):
            # Retrieve the values
            a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

            # Print the values
            print(f"a: {a}, k: {k}, x0: {x0}")
            
for parameter_set in parameter_sets:
    # Check if the parameters match, including the voltage class
    if (
        Pole_type ==  "Concrete_low" and
        Hazard_type == "Earthquake" and
        parameter_set['Voltage_class'] == Voltage_class and
        parameter_set['Pole_type'] == Pole_type and
        parameter_set['Hazard_type'] == Hazard_type and
        parameter_set.get('corrosion', False) == corrosion and
        parameter_set['Height'] == height and
        parameter_set['Condition'] == Condition and
        (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
    ):
        # Retrieve the values
        a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

        # Print the values
        print(f"a: {a}, k: {k}, x0: {x0}")
        
           
for parameter_set in parameter_sets:
    # Check if the parameters match, including the voltage class
    if (
        Pole_type ==  "Tubular_steel" and
        Hazard_type == "Earthquake" and
        parameter_set['Voltage_class'] == Voltage_class and
        parameter_set['Pole_type'] == Pole_type and
        parameter_set['Hazard_type'] == Hazard_type and
        parameter_set.get('corrosion', False) == corrosion and
        parameter_set['Condition'] == Condition and
        (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
    ):
        # Retrieve the values
        a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

        # Print the values
        print(f"a: {a}, k: {k}, x0: {x0}")


           
for parameter_set in parameter_sets:
    # Check if the parameters match, including the voltage class
    if (
        Pole_type ==  "Concrete_high" and
        Hazard_type == "Tsunami" and
        parameter_set['Voltage_class'] == Voltage_class and
        parameter_set['Pole_type'] == Pole_type and
        parameter_set['Hazard_type'] == Hazard_type and
        parameter_set.get('corrosion', False) == corrosion and
        parameter_set['Condition'] == Condition and
        (wind_degree is None or parameter_set.get('wind_degree') == wind_degree)
    ):
        # Retrieve the values
        a, k, x0 = parameter_set['a'], parameter_set['k'], parameter_set['x0']

        # Print the values
        print(f"a: {a}, k: {k}, x0: {x0}")


# Check if a valid parameter set was found
if all((a, k, x0)):
    while True:
        # Ask user for input
        x_input = input("Enter x value for logistic function (type 'exit' to stop): ")

        # Check if the user wants to exit
        if x_input.lower() == 'exit':
            print("Exiting the program...")
            break

        try:
            # Convert input to float
            x = float(x_input)

            # Call the logistic function
            result = logistic_function(x, a, k, x0)

            # Print the result
            print("Probability of Failure:", result)
        except ValueError:
            print("Invalid input. Please enter a valid number or 'exit' to stop.")
else:
    print("No matching parameter set found.")

    
    
    
    
    
    
    
