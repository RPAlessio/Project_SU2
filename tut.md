---
title: Laminar Incompressible Internal Flow with Convective Heat Transfer
permalink:
written_by: pereirrn 
for_version: 7.3.0
revised_by: 
revision_date: 
revised_version: 7.0.2
solver: INC_NAVIER_STOKES
requires: SU2_CFD
complexity: basic
follows: 
---

## Goals

After concluding this tutorial, the user will have knowledge about laminar, incompressible, internal flow in a pipe with symmetry. The tutorial about [incompressible laminar flow over a flate plate with heat transfer](https://su2code.github.io/tutorials/Inc_Laminar_Flat_Plate/) was considered as the basis for the current tutorial. Therefore, the following features of SU2 are considered in this tutorial:

- Steady, 2D, laminar, incompressible, Navier-Stokes equations 
- Energy Equation
- Flux Difference Splitting (FDS) convective scheme in space (2nd-order, upwind)
- Euler implicit time integration
- Inlet, outlet, symmetry, and no-slip wall with convective heat transfer boundary conditions

The goal os this tutorial is to demonstrate to the user of SU2 how to perform the simulation of a simple, but yet complete, incompressible laminar internal flow with important boundary conditions, such as convective heat flux and symmetry.


## Tutorial

This tutorial will present a step-by-step guide on how to solve laminar internal flows employing [the incompressible solver of SU2](https://su2code.github.io/docs_v7/Solver-Setup/). Please make sure that the SU2 code can be compiled in your machine. For further information on compilation of SU2 and its intallation, please refer to the [Installation](https://su2code.github.io/docs_v7/Installation/) page.

### Background

Laminar incompressible flows bounded by solid walls with heat transfer to the ambient is a common set-up found in engineering applications, such as gas pipelines. Even though the tutorial provides a simplified set-up in comparison with the reality, e.g. incompressible laminar flow with constant Prandtl Number, it brings interesting insights on the influence of convective boundary condition on the temperature field of the flow and the employment of symmetry boundary condition to reduce the model.

### Problem Setup

This tutorial will solve the problem for an incrompressible laminar flow confined by solid walls including heat transfer from the corresponding flow to the environment through convection. The following set-up was employed:

- Density (constant) = 1.2886  kg/m^3
- Inlet Velocity Magnitude = prescribed profiled through external file `inlet_profile.dat` 
- Inlet Flow Direction, unit vector (x,y,z) = (1.0, 0.0, 0.0) 
- Inlet Temperature = 1000 K
- Environment Temperature = 293 K
- Convection Heat Transfer Coefficient: 15 W/m²K
- Outlet Pressure = 0.0 N/m^2
- Viscosity (constant) = 0.00125 kg/(m-s)
- Prandtl Number (constant) = 0.72

### Mesh Description

The computational mesh employed in this tutorial is structured. The discretization was employed through quadrilaterals of equal size throughout the whole domain. In total there are 15000 points in the mesh with 15 points in the y-direction and 10000 points in the x-direction. The mesh comprises 13986 elements in total.
The left edge, where x = 0.0, is considered in the inlet of the domain while the right edge, where x = 5.0, is the corresponding outlet. The upper bound of the domain is the symmetry edge of domain while the lower bound is the wall.

### Configuration File Options

Many keywords of the configuration file are described here. In order to enable an incompressible flow with heat transfer the following options were employed:

```
% -------------------- INCOMPRESSIBLE FLOW DEFINITION ------------------%
%
% Initial density for incompressible flows
% (1.2886 kg/m^3 by default (air), 998.2 Kg/m^3 (water))
INC_DENSITY_MODEL= CONSTANT
INC_DENSITY_INIT= 1.2886

INC_ENERGY_EQUATION = YES
```
The keyword `INC_ENERGY_EQUATION = YES` is necessary in order to enable the heat exchange between the flow and the environment. It is also important to mention that in this tutorial the density is considered constant. This is achieved through the keyword `INC_DENSITY_MODEL = CONSTANT`. 

Since the energy equation is activated, it its necessary to define an initial temperature of flow inside of the domain. Such specificiation is accomplished as follows:

```
% Initial temperature for incompressible flows that include the
% energy equation (288.15 K by default). Value is ignored if
% INC_ENERGY_EQUATION is false.
INC_TEMPERATURE_INIT= 1000.00
```

We also need to define a fluid model in order to define an equation of state for the coupling between the momentum and energy balance equations. For simplicity, the chosen fluid model is `CONSTANT_DENSITY` and the following keyword is added to the `.cfg` file:

```
FLUID_MODEL= CONSTANT_DENSITY
SPECIFIC_HEAT_CP= 1004.703
```

For the fulfillment of the heat transfer requirement, a conductivity model needs to be defined for the fluid as well. In this tutorial, this was done through the following keywords:

```
% Conductivity model (CONSTANT_CONDUCTIVITY, CONSTANT_PRANDTL).
CONDUCTIVITY_MODEL= CONSTANT_PRANDTL
%
% Laminar Prandtl number (0.72 (air), only for CONSTANT_PRANDTL)
PRANDTL_LAM= 0.72
%
% Turbulent Prandtl number (0.9 (air), only for CONSTANT_PRANDTL)
PRANDTL_TURB= 0.90
```

For simplification, the Prandtl number was considered to be constant. Now, the boundary conditions presented. For the inlet of the domain, a velocity inlet was chosen. An external file was used to describe the velocity profile of the flow at the inlet. Along with the velocity profile, the temperature of the flow at the inlet is also defined in this external file with extension `.dat`. In order to define the type of boundary condition for the inlet and to include the information from the external file, the following keywords are employed:

```
INC_INLET_TYPE= VELOCITY_INLET
% Read inlet profile from a file (YES, NO) default: NO
%SPECIFIED_INLET_PROFILE= YES
%
% File specifying inlet profile
%INLET_FILENAME= inlet_step_sym.dat

INLET_MATCHING_TOLERANCE= 0.01
%
% Inlet boundary marker(s) with the following formats (NONE = no marker)
% Incompressible: (inlet marker, temperature, velocity magnitude, flow_direction_x,
%           flow_direction_y, flow_direction_z, ... ) where flow_direction is
%           a unit vector.
MARKER_INLET= ( inlet, 1000.0, 1.0, 1.0, 0.0, 0.0 )
```
For the outlet, a classic "Pressure" boundary condition was applied. This is achieved as follows:

```
INC_OUTLET_TYPE= PRESSURE_OUTLET
MARKER_OUTLET= ( outlet, 0.0 )
```
For the upper bound of the domain, a "Symmetry" boundary conditions was applied:

```
% MARKER_SYM= (marker name)
MARKER_SYM= ( wall )
```

Lastly, the lower bound of the domain is characterized by a non-slip condition along with a convective heat flux boundary condition. This is employed as follows:

```
% MARKER_HEATTRANSFER= (marker name, valeu of convective heat transfer coefficient in [W/m²K], value of environment temperature in [K])
MARKER_HEATTRANSFER = ( sym, 15, 293.0 )
```
With the prescribed boundary conditions, the problem is fully defined.

### Running SU2

To run this tutorial, follow the procedure:

 1. Run the following command from the directory where the `.cfg`  and `.su2` from the tutorial are saved:
 
    ```
    $ SU2_CFD lam_inc_convec.cfg
    ```

 2. SU2 will print residual updates with each iteration of the flow solver, and the simulation will terminate after reaching the specified convergence criteria.
 3. Files with the corresponding results will be saved in the same directory. The files with extension `.vtu` can be opened with the open-source software [Paraview](https://www.paraview.org/).


