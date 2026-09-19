# Solar-System-Simulation
Simulation of our Solar System made in python to understand how planets orbit the Sun

Orbitalis 3D: N-Body Gravitational Solar System Simulation
High-precision 3D N-body Solar System simulator built in Python using GPU-accelerated Taichi. Features exact Keplerian orbital setups, Jacobi barycentric correction, and interactive real-time visualization.

Orbitalis 3D simulates the motion of all eight planets and the Sun by solving Newton's law of universal gravitation using a Velocity-Verlet integrator. Instead of simple circular orbits, it incorporates exact JPL J2000 planetary parameters, including semi-major axes, eccentricities, and longitudes of perihelion. By calculating gravitational interactions across all bodies (N-body) and adjusting initial state vectors to local interior barycentres (Jacobi coordinates), the simulation achieves an accurate orbital period agreement with NASA JPL observations to within ~0.1%. Users can interactively explore the Solar System in 3D while monitoring live telemetry and energy conservation error.

Physics & Accuracy Explanation
1. N-Body Gravitational Physics
The core engine computes the full N-body interactions (every planet pulls on every other planet and the Sun) using Newton's Law of Universal Gravitation. The acceleration of each body is calculated by summing the gravitational pull from all other bodies based on their mass and distance. This accounts for real planetary perturbations (such as Jupiter altering the orbits of inner planets) rather than treating planets as isolated bodies on perfect tracks.

2. Jacobi Coordinates & Reflex Motion Correction
In simple simulations, planets are often placed around a stationary Sun. However, the combined mass of the planets (especially Jupiter) causes the Sun to wobble around the Solar System's center of mass (barycentre) at speeds up to ~12 m/s.

To prevent this wobble from introducing artificial errors into outer planet orbits, Orbitalis 3D uses Jacobi coordinates:

Each planet's initial orbit is initialized relative to the barycentre of all bodies interior to it (the mass of the Sun plus the combined mass of all planets closer to the Sun).

The total system momentum is set to zero at the exact start time.

This Jacobi coordinate setup improves outer planet orbital period accuracy (e.g., Neptune) from a ~0.8% error down to ~0.1% compared to NASA JPL telemetry.

3. Numerical Integration & Energy Conservation
Velocity-Verlet Integrator: The simulation uses a second-order symplectic Velocity-Verlet algorithm. Symplectic integrators preserve the phase-space volume, maintaining long-term stability and bounded energy drift without artificial decay.

Double Precision (f64): All Taichi physical kernels execute in 64-bit floating-point precision to minimize numerical rounding errors over thousands of orbits.

Validation & Energy Error: The simulation dynamically tracks total orbital energy (kinetic energy plus potential energy) in real time to monitor numerical stability, plotting the relative energy error |(E - E0)/E0| upon exit.
