+ [Multi-body modelling of recumbent cycling: An optimisation of configuration and cadence](#multibody-modelling-of-recumbent-cycling-an-optimisation-of-configuration-and-cadence)
+ [Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces](#adjusting-the-axle-placement-in-wheelchair-users-to-minimize-shoulder-joint-forces)
+ [Spine pressure validation](#spine-pressure-validation)
+ [Validation references](#validation-references)
    - [Webinars](#webinars)
    - [Papers and conference presentations](#papers-and-conference-presentations)


## Multi-body modelling of recumbent cycling: An optimisation of configuration and cadence

**Authors:** Pamela de Jong and Kenneth Meijer University of Eindhoven

This section is based this [Poster](http://wiki.anyscript.org/images/3/33/Dejong.pdf).

This study is part of the work done by ‘Team Icarus’ that is aiming to break the long distance record flying a Human Powered Aircraft. The aim of this study is to find an optimal recumbent position in combination with an optimal cadence using musculo-skeletal modelling and optimization algorithms.

![Recumbet model](https://cloud.githubusercontent.com/assets/22542671/20755334/dce461d4-b70e-11e6-9d39-1e93b9bb7d98.png)

*Recumbent model*

![image](https://cloud.githubusercontent.com/assets/22542671/20755393/1afda80e-b70f-11e6-87bd-6f784d50c258.png)

*Experimental setup*

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

*Predicted activation times (fat) versus experimental activation times (small ) per muscles*

![image](https://cloud.githubusercontent.com/assets/22542671/20755503/840b7074-b70f-11e6-86dd-e155917820e0.png)

*Predicted pedal forces (dashed) versus experimental pedal forces (solid).*

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

*Experimental setup*

![image](https://cloud.githubusercontent.com/assets/22542671/20755623/e71e2fd0-b70f-11e6-9c5a-7c3f608ddad2.png)

*Data acquisition*

![image](https://cloud.githubusercontent.com/assets/22542671/20755634/f3701f82-b70f-11e6-8a3d-45f147e128f3.png)

*1. Anterior deltoid; 2. Biceps; 3. Pectoralis major; 4. Posterior deltoid; 5. Trapezius; 6. Triceps*

**Measured Variables:**

+ Subject anthropometrics
+ 3-D kinematics from Vicon
+ Wheelchair measurements
+ Fx, Fy, and Fz propulsion forces from SMARTWheel’s
+ Surface EMG from 12-muscles

**Model output:**

+ Muscle forces and activations

**Related links**
+ Sarah R. Dubowsky, John Rasmussen Sue Ann Sisto, Noshir A. Langrana (2008): Validation of a musculo-skeletal model of wheelchair propulsion and its application to minimizing shoulder joint forces. Accepted for publication, Journal of Biomechanics.
+ [Webcast presentation: Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces (Sarah Sullivan-Dubrowsky, 8. November, 2007)](http://www.anybodytech.com/fileadmin/webcasts/AnyBodyWebcastWheelchairNov07.pdf)
+ [Webcast recording: Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces (Sarah Sullivan-Dubrowsky, 8. November, 2007)](http://www.anybodytech.com/fileadmin/webcasts/AnyBodyWebcastWheelchairNov07.pdf)

## Spine pressure validation

**Author:** Sylvain Carbes, AnyBody Technology

This example is a comparison between a model and results from the paper:

+ New In Vivo Measurements of Pressures in the Intervertebral Disc in Daily Life. Wilke, Hans-Joachim PhD *; Neef, Peter MD +; Caimi, Marco MD ++; Hoogland, Thomas MD [S]; Claes, Lutz E. PhD Spine. 24(8):755-762, April 15, 1999.

**Method**

+The results in the paper are the disc presures expresed as a percentage of the standing posture pressure.
+The results in AnyBody are the disc compresion forces expresed as a percentage of the standing posture force.
+The percentage from AnyBody and from the paper are compared to each others.
+The load situations from the paper was reproduced in AnyBody

![image](https://cloud.githubusercontent.com/assets/22542671/20755830/a6dcaf2c-b710-11e6-8ded-436eefa19b0d.png)

*Reproduced load situations*

![image](https://cloud.githubusercontent.com/assets/22542671/20755844/b78d1938-b710-11e6-911e-42e8b8169aff.png)

*Spine pressure validation*

**Conclusion**

The comparison gives good results

**Related links**

The model which has been used for producing these results can be found in the AnyBody Managed Model Repository in the folder: Application ->Validation -> SpinePressureValidation

# Validation references

## Webinars

Webinars are available from [here](http://www.anybodytech.com/index.php?id=199).

+ Seated Human Model Validation (Christian Gammelgaard Olesen, 19. May, 2009)
+ TLEM: A new detailed lower extremity model (Dr. Sebastian Dendorfer, 20. August, 2008)
+ Validation of Hip Joint Force Simulation by Gait Analysis (Catherine Manders, 29. January, 2008)
+ Adjusting the Axle Placement in Wheelchair Users to Minimize Shoulder Joint Forces (Sarah Sullivan-Dubrowsky, 8. November, 2007)
+ Development of a musculoskeletal simulator for swimming (Dr. Motomu Nakashima, 3. October, 2007)
+ A detailed rigid-body cervical spine model based on inverse dynamics (Dr. Mark de Zee, 18. September, 2007)
+ Validation of the AnyBody version of the Dutch Shoulder Model by the in-vivo measurement of GH contact forces by Bergmann et al. (Prof. John Rasmussen, 26. April, 2007)
+ Scaling strength in human simulation models (Dr. Kenneth Meijer, 17. January, 2007)
+ A generic detailed rigid-body lumbar spine model (Dr. Mark de Zee, 4. December, 2006)
+ Validation of musculoskeletal models (Dr. Mark de Zee, 4. October, 2006)

## Papers and conference presentations

+ E. Forster, et al. (2004): Agreement of muscular activation and hip contact forces predicted with two different software packages. 11th Workshop on The Finite Element Method in Biomedical Engineering, Biomechanics and Related Fields. University of Ulm, Department of Orthodontics, July 14th -15th, 2004.
+ M. de Zee, et al. (2007).Validation of a musculo-skeletal model of the mandible and its application to mandibular distraction osteogenesis. Journal of Biomechanics, 40, 1192–1201.
+ J. Rasmussen, et al. (2007): Comparison of a musculoskeletal shoulder model with in-vivo joint forces. International Society of Biomechanics 11th congress, Taipei, Taiwan.
+ C. Manders, et al. (2008): Validation Of Musculoskeletal Gait Simulation For Use In Investigation Of Total Hip Replacement. 16th Congress of the European Society of Biomechanics. Journal of Biomechanics, Volume 41, Supplement 1, July 2008, Page S488.
+ S.R. Dubowsky et al. (2008): Validation of a musculo-skeletal model of wheelchair propulsion and its application to minimizing shoulder joint forces. Accepted for publication, Journal of Biomechanics 41, 2981-2988.
+ A. Nolte, et al. (2008). Analysis of the muscle and joint forces in the shoulder joint using the anybody simulation model. 16th Congress of the European Society of Biomechanics. Journal of Biomechanics, Volume 41, Supplement 1, July 2008, Page S492
+ J . Wu, et al. (2008): Analysis of musculoskeletal loading in an index finger during tapping. Journal of Biomechanics , 41, 668 – 676.
+ J. Rasmussen, et al. (2009): Validation of a biomechanical model of the lumbar spine. International Society of Biomechanics 12th congress, Cape Town, RSA.