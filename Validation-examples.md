## Multi-body modelling of recumbent cycling: An optimisation of configuration and cadence

**Authors:** Pamela de Jong and Kenneth Meijer University of Eindhoven

This section is based this [Poster](http://wiki.anyscript.org/images/3/33/Dejong.pdf).

This study is part of the work done by ‘Team Icarus’ that is aiming to break the long distance record flying a Human Powered Aircraft. The aim of this study is to find an optimal recumbent position in combination with an optimal cadence using musculo-skeletal modelling and optimization algorithms.

![Recumbet model](https://cloud.githubusercontent.com/assets/22542671/20755334/dce461d4-b70e-11e6-9d39-1e93b9bb7d98.png)

Recumbent model

![image](https://cloud.githubusercontent.com/assets/22542671/20755393/1afda80e-b70f-11e6-87bd-6f784d50c258.png)

Experimental setup

**Input to the model:**
+ Kinematics
+ Crank torque (not the pedal forces!!), based on the measured tangential pedal forces
**Model output:**
+ Tangential pedal forces
+ Radial pedal forces
+ Muscle forces and activations
**Measured Variables:**
+ Tangential pedal forces
+ Radial pedal forces
+ Surface EMG measurements

![image](https://cloud.githubusercontent.com/assets/22542671/20755484/77452470-b70f-11e6-875b-2128a63470ad.png)

Predicted activation times (fat) versus experimental activation times (small ) per muscles

![image](https://cloud.githubusercontent.com/assets/22542671/20755503/840b7074-b70f-11e6-86dd-e155917820e0.png)

Predicted pedal forces (dashed) versus experimental pedal forces (solid).

**Conclusions:**
+ The recumbent model predicts muscle activation times and pedal forces well.
+ Energy model is applicable to all AnyBody models and led to plausible efficiency values.

## Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces

**Authors:** Sarah Sullivan-Dubrowsky, Rutgers, State Univ. of New York

This example is based this webcast : [Webcast Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces (Sarah Sullivan-Dubrowsky, 8. November, 2007)](http://www.anybodytech.com/redir.html?forward=webcast#%0AAdjusting%20the%20Axle%20Placement%20in%20Wheelchair%20Users%20to%20Minimize%20Shoulder%20Joint%20Forces%0A%20)

**Background**
This is a demonstration of the construction and validation of a model of an individual with paraplegia propelling a wheelchair. This anthropometrically accurate model is driven kinematically with motion capture data and kinetically with recorded x-, y-, and z- forces from force-sensing push rims. Two validation techniques are used to build confidence in the model: 1.) experimental subject EMG activity is compared to corresponding muscle activity as calculated by AnyBody, and 2.) the torque (as opposed to the individual force-components) as calculated from the force-sensing push rims drives the model and the resulting force components are compared to the original push rim force outputs. With confidence in the model, we look at utilizing the AnyBody modeling system for quantitatively comparing the axle placement in wheelchair set-up and its effect on shoulder joint forces.

+ Muscles in the model are represented as multiple fibers
+ Compare each computational fiber’s activity with experimental activity
+ Highlighted (purple) fibers correspond to participant EMG

![image](https://cloud.githubusercontent.com/assets/22542671/20755616/dc311de4-b70f-11e6-95d7-e507197b62a0.png)

Experimental setup

![image](https://cloud.githubusercontent.com/assets/22542671/20755623/e71e2fd0-b70f-11e6-9c5a-7c3f608ddad2.png)

Data acquisition

![image](https://cloud.githubusercontent.com/assets/22542671/20755634/f3701f82-b70f-11e6-8a3d-45f147e128f3.png)

1. Anterior deltoid; 2. Biceps; 3. Pectoralis major; 4. Posterior deltoid; 5. Trapezius; 6. Triceps

