# High-Value Target (HVT) Missile Defense Simulation

## Overview

The HVT Missile Defense Simulation is a high-fidelity MATLAB project that models the interception of ballistic targets using multiple interceptor missiles with advanced guidance algorithms. It provides a robust framework for evaluating and comparing different missile guidance strategies against high-value ballistic targets capable of performing evasive maneuvers.

## Key Features

- **Realistic Physics**: Accurate aerodynamics, atmospheric effects, and G-force limited maneuvers (30G maximum)
- **Advanced Guidance Methods**: Implements and compares legacy and phase-based guidance approaches
- **Multi-Phase Guidance**: Transitions between pre-calculated, midcourse, and terminal guidance phases
- **Batch Simulation**: Statistical analysis with parallel processing support for large-scale simulations
- **Performance Metrics**: Comprehensive analysis of interception success rate and acceleration effort
- **Visualization Tools**: 3D engagement animation and performance analytics

## Quick Start

### Main Menu Interface

The easiest way to access all simulation functionality:

```matlab
MainMenu
```

### Single Simulations

Run a default simulation:
```matlab
main_script
```

Run with custom parameters:
```matlab
run_custom_sim
```

### Batch Simulations

Run statistical comparison between guidance methods:
```matlab
% Regular batch simulation with detailed output
BatchSimulation

% Silent batch simulation (minimal console output)
SilentRunBatchComparison
```

## Technical Specifications

- **Coordinate System**: North-East-Down (NED)
- **Units**: Imperial (feet, pounds)
- **Simulation Time Step**: 0.01 seconds
- **Maximum Simulation Time**: 100 seconds
- **Target Model**: Based on 9K720 Iskander with 30G maximum acceleration
- **Angle of Attack Range**: -5° to +10° (constrained for realistic maneuvers)
- **Guidance Ranges**:
  - Long-range (>10km): Pre-calculated or real-time PIP guidance
  - Mid-range (5-10km): Active PIP calculation
  - Short-range (<5km): Terminal proportional navigation

## Guidance Methods

### Legacy Method
- Uses continuous real-time PIP calculation until terminal guidance range (5km)
- No pre-simulation phase
- All interceptors launched simultaneously
- Terminal proportional navigation guidance under 5km

### New Method (Phase-Based)
- Pre-simulation with three trajectory cases: positive, zero, and negative maneuvers
- Long-range guidance (>10km): Uses pre-calculated PIPs from pre-simulation
- Mid-range guidance (5-10km): Active PIP calculation
- Short-range guidance (<5km): Terminal proportional navigation
- Assigns different interceptors to different predicted trajectories
- **Optimal Interceptor Selection**: When target begins maneuvering, observes its trajectory for 10 seconds, then determines which interceptor has the closest trajectory match to the actual maneuver and deactivates others to optimize acceleration effort

## Project Structure

```
hvt-missile-sim/
│
├── MainMenu.m               # Central interactive menu interface
├── main_script.m            # Default simulation entry point
├── run_custom_sim.m         # Custom parameter simulation
├── RunSimulation.m          # Core unified simulation engine
├── BatchSimulation.m        # Batch simulation controller
├── SilentRunBatchComparison.m # Silent batch simulation with minimal output
│
├── config/                  # Parameter management
│   └── SimulationConfig.m   # Centralized configuration
│
├── dynamics/                # Physics models
│   ├── EnhancedTargetDynamics.m       # Target physics with maneuvers
│   ├── MissileStateDerivative.m       # Interceptor state equations
│   ├── TargetStateDerivative.m        # Target state equations with realistic lift model
│   └── ...
│
├── guidance/                # Guidance algorithms
│   ├── BasicPIP.m           # Predicted Intercept Point calculation
│   ├── PIPGuidance.m        # Midcourse guidance
│   ├── TerminalPNGuidance.m # Terminal proportional navigation
│   ├── GuidanceSelector.m   # Method selection logic
│   └── ...
│
├── utils/                   # Utility functions
│   ├── DefineConstants.m    # Physical constants definition
│   ├── InitializeTargets.m  # Target initialization
│   ├── InitializeInterceptors.m  # Interceptor initialization
│   ├── TargetParameterRandomizer.m # Randomization for batch simulations
│   └── ...
│
├── analysis/                # Analysis tools
│   ├── CalculateAccelerationEffort.m  # Performance metrics
│   └── ...
│
├── visualization/           # Visualization components
│   └── AnimateEngagement.m  # 3D visualization of engagement
│
├── batch/                   # Batch simulation tools
│   ├── RunBatchSimulation.m  # Core batch function
│   └── ...
│
├── guidance-comparison/     # Guidance method comparison framework
│   ├── LegacyGuidance.m     # Legacy guidance implementation
│   ├── NewMethodGuidance.m  # Phase-based guidance implementation
│   └── ...
│
└── results/                 # Output directory for simulation results
```

## Simulation Workflow

1. **Initialization**:
   - Load parameters from configuration
   - Initialize target and interceptor states
   - Set up coordinate systems and physical constants

2. **Pre-Simulation** (New Method Only):
   - Generate three potential target trajectories with different maneuver cases
   - Calculate Predicted Intercept Points (PIPs) for each scenario
   - Assign interceptors to different predicted trajectories

3. **Main Simulation**:
   - Step through time using 0.01s intervals (RK4 integration)
   - Update target states with realistic physics
   - Apply appropriate guidance law based on range to target
   - Calculate acceleration commands and update interceptor states
   - Check for interceptions and end conditions

4. **Analysis & Visualization**:
   - Generate 3D animation of the engagement
   - Plot acceleration profiles
   - Calculate performance metrics (success rate, acceleration effort)
   - Compare guidance methods in batch mode

## Physics Models

### Target Dynamics

- **Enhanced Aerodynamics**: Realistic lift and drag based on altitude and velocity
- **Atmospheric Model**: Exponential density variation with altitude
- **G-Force Limiting**: Maximum 30G acceleration limit for target maneuvers
- **Realistic Lift Model**: Lift vector perpendicular to velocity in the maneuver plane
- **Maneuver Control**: Configurable Angle of Attack (-5° to +10°) and maneuver altitude

### Interceptor Dynamics

- **Maximum Acceleration**: 50G limit for guidance commands
- **Guidance Phases**: Different control laws based on engagement phase
- **Launch Parameters**: Configurable launch position, timing, and salvo intervals
- **State Integration**: 4th-order Runge-Kutta for high accuracy

## Batch Simulation Framework

The batch simulation system provides statistical analysis by running multiple scenarios with randomized parameters:

- **Parallel Processing**: Automatic detection and utilization of multi-core systems
- **Memory Optimization**: Efficient data structures for large batch sizes
- **Parameter Randomization**: Generates diverse target scenarios for robust testing
- **Performance Metrics**: Tracks success rates and acceleration effort for each method
- **CSV Output**: Creates timestamped CSV files with detailed results
- **Visualization**: Generates comparison charts between guidance methods

## Performance Features

- **Parallel Computing**: Automatic detection and utilization of the Parallel Computing Toolbox
- **Vectorized Operations**: Optimized calculations for faster execution
- **Buffered I/O**: Efficient file operations for batch processing
- **Memory Management**: Pre-allocated data structures to minimize overhead
- **Silent Mode**: Option to suppress verbose output for batch simulations

## Recent Improvements

### April 2025 Updates

- **Advanced Optimal Interceptor Selection**: Enhanced algorithm to observe targets for 10 seconds before selecting optimal interceptor based on trajectory matching
- **Trajectory Comparison Logic**: Improved comparison between actual and predicted trajectories for more accurate interceptor selection
- **Acceleration Effort Optimization**: Only count effort until interceptor deactivation, improving efficiency metrics
- **Silent Mode Support**: Added proper support for silent mode in SimulationConfig and batch scripts
- **Output Optimization**: Streamlined batch simulation output to show only important results
- **Parallel Processing Enhancement**: Improved parallel processing detection and reliability
- **SimulationConfig Enhancements**: Added support for silent, skipAnimation, and skipAccelerationPlot parameters
- **README Updates**: Comprehensive documentation of all project features and structure

### Previous Improvements

- **Realistic Lift Model**: Implemented physically accurate lift model perpendicular to velocity
- **Target G-Force Limits**: Added 30G acceleration limits for realistic target maneuvers
- **AoA Range Constraints**: Modified the default AoA range to -5° to +10° for realistic behaviors
- **Legacy Guidance Fix**: Ensured legacy guidance method uses real-time PIP calculation
- **Simultaneous Launch**: Modified to launch interceptors simultaneously instead of with a salvo delay

## Usage Examples

### Custom Single Simulation

```matlab
% Set custom parameters
params = struct();
params.launchDelay = 0.0;                  % Launch delay in seconds
params.preSim_maneuverAltitude = 60.0;     % Maneuver altitude in kft
params.preSim_maneuverAoA = [10, 0, -5];   % AoA values in degrees

% Run with custom parameters
[outSim, targets, interceptors] = RunSimulation(params);
```

### Silent Batch Simulation

```matlab
% For batch simulations with minimal console output
SilentRunBatchComparison
```

## Troubleshooting

### Common Issues

- **Parallel Processing**: If you encounter issues with parallel processing, the simulation will automatically fall back to serial mode.
- **Visualization**: If animation windows don't appear, try running `close all` to clear any hidden figures.
- **File Paths**: Ensure all paths are properly set by running `addpath(genpath('.'))` from the main directory.

## Future Development Roadmap

1. **Advanced Guidance Methods**
   - Support for additional guidance approaches
   - Dynamic guidance parameter adaptation
   - Enhanced optimal interceptor selection with real-time reassignment capability
   - Machine learning-based trajectory prediction for better maneuver matching

2. **Enhanced Statistical Analysis**
   - Confidence intervals for success rates
   - Sensitivity analysis with parameter sweeps
   - Automated outlier detection

3. **Sensor and Communication Modeling**
   - Realistic sensor noise and errors
   - Communication delays and bandwidth limitations

4. **Performance Optimizations**
   - Further memory usage improvements
   - GPU acceleration for batch simulations

## Contact Information

For questions or contributions, please contact the project maintainers.
