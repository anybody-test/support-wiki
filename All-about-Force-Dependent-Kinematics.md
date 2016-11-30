# Force Dependent Kinematics

## If the model fails with a Newton relaxation too small. (final force error = XXX) please consider these points

+ This error means that the solver failed to make the force residual lower than the ForceTol defined in the AnyBodyStudy.
+ When you run the FDK it will start by inserting the driver positions for the DOF you have made force dependent. The first thing which happens is that the kinematics is solved. Then it will then start to alter the FDK DOF to get rid of the residuals, if that fails you will get a force error like the one above.
+ If you only have forces ie. linear translations to be force dependent then the number of the force error is in Newtons if you have it mixed with rotational dof the number can also be in Nm.
+ If you increase the ForceTol it simply means that there will be a higher residual forces in the constraints you are trying to make zero or reduce to be below the limit you give in the ForceTol.
+ The remaining residual in the FDK dof can be seen in the driver function of the FDK DOF. Keep in mind that this error can be seen as an artificial reaction in the model which has no cost for the human model, so it is recommended to have it low.
+ There is a practical limit to how low the ForceTol can be, essentially the stiffer structures appearing around the FDK DOF the higher this tolerance needs to be because the kinematic tolerance cannot be infinitely small. The ForceTol cannot be smaller than the force difference arising from moving the FDK DOF with the magnitude of the kinematic tolerance.
+ If the model fails at the first time step consider to alter the initial position of the DOF you have as FDK, so that the initial position is closer to where you expect the solution to end. Example: you have a revolute knee joint which should be FDK on one or several of the linear DOF. In the revolute joint you cannot alter the driver positions these will be zero always. So instead replace the joint with an AnyKinLinear and AnyKinRotational driven by an AnyKinEqSimpleDriver, in this way you can alter the DriverPos value to non-zero values that can be closer to the solution. If the model contains surface surface contact it can be a good idea to ensure a small amount of initial contact.

## MaxNewtonStep

This setting decided how far the FDK solver may go in one step. If the model has linear DOF a values of 1 means it can go 1 meter, this can create problems for the surface contact which may fall into a wrong solution for such big steps. So try to limit this value to something you think the surface contact can handle without that it penetrates too deep, so for example 0.001, if the value becomes to small it may slow down the analysis, so this is a tradeoff between speed and stability.

## Choosing the FDK DOF

It can have a major impact on your model, which DOF you choose to be controlled by FDK. Basically if you want to drive for example five DOF in a knee, it is a good idea to try to choose these measures in a way that they will change as little as possible when the model is running. This will make it easier for the solver to find the solution. In the same line of thought it can be important to add a gearing on the linear or rotational FDK dof, which can make them more "equal". This can be done by feeding the measure (either the linear or rotational) through a AnyKinMeasureLinComb with a constant in and then apply the FDK on this measure instead of the original measure.

Note that when the solver is running it may change the FDK DOF as much as the MaxNewtonStep allows, and the MaxNewtonStep do not make a difference if the measure is linear or rotational, this is why i can be important to make a gearing so that influence of a change in a linear DOF is comparable with a similar change of a rotational DOF, doing this can significantly improve the convergence of the model.

Please note that the gearing effectively alters the ForceTolerance

## If the model fails with a kinematic error

+ Ensure that the model handles reasonable driver values of your FDK DOF. When solving for the solution the solver will alter these driver values seeking to remove the reactions, so it will try out many combinations of driver values. This means that it is important that the kinematic model is robust to such alterations.

## AnyForceSurfaceContact

If the surface has thin parts is a good idea to remove the backside of the surface so that it becomes open. This ensures that the forces will continue to grow as the surfaces are compressed into each other. If the surface is thin it will at a certain point be "pushed" out on the backside, using open surfaces prevent this and ensures that the FDK solver can find the correct solution.

If you are using AnyForceSurfaceContact contact make sure that the contact force works as intended. This can be done simply by creating a standalone model consisting of

+ A fixed reference frame with one of the surfaces attached
+ A segment with the second surface defined
+ A prismatic joint between the segment and fixed reference frame
+ A driver enforcing the prismatic joint to make the two surfaces start by having no contact continuing until there is a deep penetration.
+ Then in the AnyForceSurfaceContact read out the result force and ensure that the force is not declining too rapidly after having reached peak force. This is important because the FDK solver may try out penetrations which are deep and it may cause the solution to get stuck on the “wrong” side of the surface. A simple solution can be to add extra surfaces below the real surface and create contact between these. They will act as a safety net, but should never be in contact when the solution has been found.

## Introduce FDK stepwise

+ When introducing FDK DOF to the model do this stepwise, one DOF at the time, and for each step ensure it runs as expected and that the solution is stable before introducing additional DOF with FDK

## More info

+ Read more about the FDK solver [here](http://www.anybodytech.com/fileadmin/AnyBody/Docs/Tutorials/chapX_ForceDependentKinematics/lesson3.html).