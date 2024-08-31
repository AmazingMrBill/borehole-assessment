# Borehole Assessment Project

## Overview

This project focuses on analyzing and optimizing borehole operations using various simulation and data analysis techniques. It includes subsurface modeling, geological mapping, and scenario testing to help in decision-making and planning for borehole management.

## Features

1. **Subsurface Modeling**: Simulates and visualizes the geological formations below the Earth's surface using synthetic data.
2. **Geological Mapping**: Creates detailed maps showing different geological formations based on borehole data.
3. **Scenario Testing**: Simulates different conditions (e.g., rainfall and usage rates) to evaluate their impact on borehole yield.

## Installation

To set up the environment for this project, follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/borehole-assessment.git
   cd borehole-assessment
   ```

2. **Create and Activate a Virtual Environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```

3. **Install Required Packages**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Install SimulAI**:
   SimulAI might not be available via `pip`. Ensure it is installed from the correct source or repository as per the documentation:
   ```bash
   pip install git+https://github.com/IBM/simulai.git
   ```

## Usage

### Subsurface Modeling

This feature simulates geological formations and visualizes them. Run the script `subsurface_modeling.py` to generate and plot subsurface data.

```bash
python subsurface_modeling.py
```

### Geological Mapping

Creates a geological map based on synthetic data. Run the script `geological_mapping.py` to generate and plot the map.

```bash
python geological_mapping.py
```

### Scenario Testing

Simulates different conditions to analyze their impact on borehole yield. Run the script `scenario_testing.py` to generate and plot yield scenarios.

```bash
python scenario_testing.py
```

## Code Examples

### 1. Subsurface Modeling

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.interpolate import griddata

# Generate synthetic subsurface data
np.random.seed(42)
n_points = 100
x = np.random.uniform(0, 10, n_points)
y = np.random.uniform(0, 10, n_points)
z = np.sin(x) * np.cos(y) + np.random.normal(scale=0.1, size=n_points)

# Create a grid for interpolation
grid_x, grid_y = np.mgrid[0:10:100j, 0:10:100j]
grid_z = griddata((x, y), z, (grid_x, grid_y), method='cubic')

# Plot the subsurface model
plt.figure(figsize=(10, 6))
plt.contourf(grid_x, grid_y, grid_z, levels=14, cmap='viridis')
plt.scatter(x, y, c=z, edgecolor='k', marker='o')
plt.title('Subsurface Modeling')
plt.colorbar(label='Subsurface Property')
plt.xlabel('X coordinate')
plt.ylabel('Y coordinate')
plt.show()
```

### 2. Geological Mapping

```python
import geopandas as gpd
from shapely.geometry import Point

# Generate synthetic geological data
n_points = 50
np.random.seed(42)
data = {
    'Longitude': np.random.uniform(-180, 180, n_points),
    'Latitude': np.random.uniform(-90, 90, n_points),
    'Formation': np.random.choice(['Sandstone', 'Limestone', 'Clay'], n_points)
}

# Create a GeoDataFrame
gdf = gpd.GeoDataFrame(data, 
                       geometry=gpd.points_from_xy(data['Longitude'], data['Latitude']),
                       crs="EPSG:4326")

# Plot the geological map
fig, ax = plt.subplots(figsize=(12, 8))
gdf.plot(column='Formation', ax=ax, legend=True, cmap='Set2', markersize=50)
plt.title('Geological Mapping')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```

### 3. Scenario Testing

```python
import numpy as np
import matplotlib.pyplot as plt

# Define a function to simulate borehole yield based on different conditions
def simulate_yield(rainfall, usage_rate):
    base_yield = 1000  # Base yield in cubic meters
    yield_reduction = 0.1 * usage_rate  # Yield reduction due to usage
    yield_increase = 0.05 * rainfall  # Yield increase due to rainfall
    return base_yield - yield_reduction + yield_increase

# Generate synthetic data
rainfall = np.linspace(0, 500, 100)  # Rainfall in mm
usage_rate = np.linspace(0, 200, 100)  # Usage rate in cubic meters per day

# Simulate yields
yields = np.array([simulate_yield(r, u) for r in rainfall for u in usage_rate])
rainfall_mesh, usage_rate_mesh = np.meshgrid(rainfall, usage_rate)
yields_mesh = yields.reshape(len(usage_rate), len(rainfall))

# Plot the scenario testing results
plt.figure(figsize=(12, 8))
plt.contourf(rainfall_mesh, usage_rate_mesh, yields_mesh, levels=20, cmap='coolwarm')
plt.colorbar(label='Borehole Yield (cubic meters)')
plt.title('Scenario Testing: Borehole Yield')
plt.xlabel('Rainfall (mm)')
plt.ylabel('Usage Rate (cubic meters/day)')
plt.show()
```

## Contributing

Contributions are welcome! Please submit issues and pull requests via the GitHub repository. Follow the standard contribution guidelines for code, documentation, and testing.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
