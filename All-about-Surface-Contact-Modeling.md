# Suface Contact Modeling

## What is a good value for the Pressure Module in the definition of ForceSurfaceContact between a PE insert and a Cobalt-Chrome Implant?

The PressureModule in the contact model is a value in N/m^3The contact model is implemented very close to the model mentioned in the paper of Bei and Fregly: Multibody dynamic simulation of knee contact mechanics. Here, a pressure is calculated using material properties as a function of a maximal penetration h. You can use the same theory for the calculation of the Pressure Module. As far as I remember the Abaqus contact model, this is closely related to the pressure overclosure. We often use values in the range around 1e11, reducing this if the solver fails to solve and checking the penetration. There is also a discussion on the forum about this topic. In particular for the knee, we 4e9 has been used.

## What is a reasonable Value for the InverseDynamics.ForceDepKin.MaxNewtonStep und ForceTol? With the default values the model is very slow and also I get Warning messages!

The InverseDynamics.ForceDepKin.MaxNewtonStep should be related to the model. The convergence of the Newton algorithm depends a lot on the initial guess and the smoothness of the problem. For the initial guess, it is important that the initial guess of the model is chosen carefully and the changes from one time step to the next are not too big since the result of a step is used for the initial guess for the next time step. So we are quite often using the default value. Similar for the ForceTol: it depends on the model. The ForceTol defines the maximal residual force in the degree FDK degree of freedom. So if the difference in the force in the range of the force tolerance results in a very small change in the kinematics, this value might be ok. For contact simulations, we are often quite happy using a tolerance of around 1N or even larger (10N).

## Does it matter which is the master and which the slave surface? And when should I use a Single Sided calculation?

If you don’t use single sided contact, it should not make a difference which surface is master and which is slave. If the single sided calculation is switched on, only the surface of the slave is used to calculate the distance of the vertices of the master surface. So if one surface is smoother (in terms of surface normals) this should be chosen as slave surface.