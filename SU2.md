## **Report 1 - SU2**
### Simulation Software Engineering
### Renan Pereira Alessio

### What is SU2?

SU2 is an open-source code which aims to provide numerical solutions to partial differential equations. Furthermore, it can calculate constrained optimization of partial differential equations. At the beginning, SU2 mostly focused on fluid dynamics problems, such as incompressible and incompressible flows, external and internal flows and aerodynamic designs. Nowadays, this open-source software performs multiphysical simulations, including fluid-structure interaction and chemically-reacting flows. Other solvers for applications such as structure mechanics considering elasticity and electrodynamics are also available in the software. From a simulation point-of-view, SU2 acts as a solver for the different applications being considered, performing all the calculations concerning the problem at hand. Therefore, the pre- and post-processings of the application are performed in different softwares.


### Main Features

#### General Overview

As previously mentioned, SU2 provides different solvers depending on the application at hand. Each solver is developed by employing the programming language C++ and is designated to a specific category of problem. In general lines, this solver must be supported with pre-processing data, such as discretization feature and physical aspects of the problem. In order to create the discretization data, one may use the open-source software Gmsh. If one does so, the information regarding the discretized domain must be saved in a file with .su2 extension. The final pre-processing step is to generate the configuration file. Such file will contain all the information concerning the physics of the problem, numerical features of the chosen analysis and the desired output information. As an example, one can determine the flow aspects, such as turbulence and compressibility, its boundary conditions, the spatial and time discretization methods and convergence parameters. This configuration file must have a .cfg extension in order to be read by the SU2 solver and must include the file with the discretization information through a command line. Once the pre-processing stage is done, one can run the desired simulation. Once the simulation is done, SU2 will output the files mentioned in the configuration file. The file containing the results will be the one with extension .vtu. This file can be read in the open-source software called ParaView.


#### Running the Simulation

Once the pre-processing files are created, one may run the simulation. In order to do so, both files created must placed in the same directory. Afterwards, one needs to access such directory through the terminal, indicate the path to the desired SU2 solver and indicate the configuration file of the simulation. Generally, the command line in the terminal has the following set-up:

user:~/Desktop/SU2Project$ /home/user/Desktop/SU2-v7.2.1-linux64/bin/SU2_CFD application.cfg 


Once the simulation is done, the output files will be written in the same directory containing the pre-processing files.

#### How to Obtain the Software

The person who desires to use SU2 can obtain its files either in its official website (https://su2code.github.io/download.html) or in its GitHub repository su2code/SU2 under releases.

#### Miscellaneous and Contributions

SU2 provides documentation on installation of the software and theory about the main governing equations alongside with a users guide. Anyone can contribute to the documentation of SU2 by submitting a pull request to the develop branch of the repository. Furthermore, a variety of tutorials enclosing many study cases and a V&V (verification & validation) section for SU2 solvers are available in the homepage of the software. Likewise, one may contribute to the tutorials and V&V sections by making a pull request to the develop branch in the repository. More information on how to contribute to the software is available at https://su2code.github.io/vandv/home/.


### Tutorial


