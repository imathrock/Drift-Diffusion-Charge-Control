# Drift Diffusion Charge Control Solver

This is a documentation of the simulation of a Green GaN LED made at IIT-Bombay 

This would also like an open source version of 1D-DDCC 
This software will be using drift diffusion modelling to simulate Currents and electron hole recombinations in certain defined structures.

The following is an Rough outline of how this program would work and will be refined later. 

## Full Process of a Drift-Diffusion Charge Control Solver

1. **Define the Physical and Geometrical Parameters**

   - **Device Geometry**: Define the geometry of the semiconductor device, including dimensions, boundaries, and material regions.
   - **Material Properties**: Specify the properties of each material region, such as permittivity ($\epsilon$), electron affinity, bandgap ($E_g$), effective masses, mobilities ($\mu_n$ and $\mu_p$), and recombination parameters.
   - **Doping Profile**: Define the doping concentrations ($N_D$ for donors and $N_A$ for acceptors) throughout the device.

2. **Set Up Initial Conditions**

   - **Initial Carrier Densities**: Assume initial electron ($n$) and hole ($p$) densities, often based on thermal equilibrium conditions:
     
     $$
     n_0 = n_i e^{\frac{E_F - E_i}{kT}}, \quad p_0 = n_i e^{\frac{E_i - E_F}{kT}}
     $$
     
     where $n_i$ is the intrinsic carrier concentration, $E_F$ is the Fermi level, $E_i$ is the intrinsic energy level, $k$ is Boltzmann’s constant, and $T$ is the temperature.

   - **Initial Potential**: Set an initial guess for the electrostatic potential $V(x)$, typically using a zero potential or a built-in potential calculated from the doping profile.

3. **Solve Poisson’s Equation for Electrostatic Potential**

   - **Poisson’s Equation**:

     $$
     \nabla^2 V = -\frac{q}{\epsilon} (p - n + N_D^+ - N_A^-)
     $$

     Here, $q$ is the elementary charge, $N_D^+$ is the ionized donor concentration, and $N_A^-$ is the ionized acceptor concentration.

   - Use numerical methods like finite differences, finite elements, or finite volumes to discretize and solve Poisson’s equation for the potential $V(x)$.

4. **Compute Electric Field**

   - Calculate the electric field $E(x)$ from the gradient of the potential:

     $$
     E = -\nabla V
     $$

5. **Solve Continuity Equations for Carrier Densities**

   - **Electron Continuity Equation**:

     $$
     \frac{\partial n}{\partial t} = \nabla \cdot J_n + G - R
     $$

   - **Hole Continuity Equation**:

     $$
     \frac{\partial p}{\partial t} = \nabla \cdot J_p + G - R
     $$

   - **Current Densities**:

     $$
     J_n = q \mu_n n E + q D_n \nabla n
     $$

     $$
     J_p = q \mu_p p E - q D_p \nabla p
     $$

   - **Discretization**: Use numerical methods to discretize and solve the continuity equations, typically employing methods like the Newton-Raphson method for nonlinear systems.

6. **Update Carrier Densities and Recombination Rates**

   - **Recombination Models**: Use appropriate recombination models (e.g., Shockley-Read-Hall (SRH), Auger, or radiative recombination) to update the recombination rate $R$.

   - **Generation Rates**: If external excitation (like light or voltage) is present, calculate the generation rate $G$.

7. **Iterative Solution for Self-Consistency**

   - **Iterate Potential and Densities**: Repeat steps 3 to 6 iteratively to achieve convergence. This means updating the electrostatic potential, electric field, carrier densities, and current densities until changes between iterations fall below a specified threshold.

   - **Convergence Criteria**: Convergence is usually defined by the change in potential and carrier densities being less than a predefined tolerance level (e.g., $10^{-6}$).

8. **Post-Processing and Analysis**

   - **Extract Device Characteristics**: Once converged, extract relevant device characteristics like current-voltage (I-V) curves, carrier concentration profiles, electric field distributions, etc.

   - **Analyze Performance**: Evaluate the performance parameters of the device (e.g., threshold voltage, carrier mobility, saturation current).

