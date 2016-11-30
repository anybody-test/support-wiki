- [Is it possible to introduce artificial joints, i.e. total knee replacements (TKR), or the introduction of an orthosis for the foot? Could a TKR be incorporated into the model to predict joint contact forces and ligament load sharing?](#is-it-possible-to-introduce-artificial-joints--ie-total-knee-replacements--tkr---or-the-introduction-of-an-orthosis-for-the-foot--could-a-tkr-be-incorporated-into-the-model-to-predict-joint-contact-forces-and-ligament-load-sharing-)
- [How can the AnyBody Modeling System handle closed chains in the model?](#how-can-the-anybody-modeling-system-handle-closed-chains-in-the-model-)
- [Does the system have a "welded" joint that creates a rigid connection between two segments?](#does-the-system-have-a--welded--joint-that-creates-a-rigid-connection-between-two-segments-)
- [What is the role of the initial positions of the segments?](#what-is-the-role-of-the-initial-positions-of-the-segments-)
- [Why is the Pos property of a driver zero once the kinematics has been solved?](#why-is-the-pos-property-of-a-driver-zero-once-the-kinematics-has-been-solved-)
- [How to understand the AnyKinRotational Cardan angles (Type=RotAxesAngles)](#how-to-understand-the-anykinrotational-cardan-angles--type-rotaxesangles-)
- [How to drive an AnyKinRotational Cardan angles (Type=RotAxesAngles)](#how-to-drive-an-anykinrotational-cardan-angles--type-rotaxesangles-)
- [How to understand the AnyKinRotational Cartesian rotation vector (Type=RotVector)](#how-to-understand-the-anykinrotational-cartesian-rotation-vector--type-rotvector-)
- [Define an interpolation driver==](#define-an-interpolation-driver--)
- [Create a linear combination measure](#create-a-linear-combination-measure)
- [Create point cloud joints](#create-point-cloud-joints)
- [Understand the available settings for the rotational measure types](#understand-the-available-settings-for-the-rotational-measure-types)
- [Different ways to add a force](#different-ways-to-add-a-force)
- [Understand the available settings of the AnyKinLinear](#understand-the-available-settings-of-the-anykinlinear)
- [Create a joint between an ellipsoid and a point](#create-a-joint-between-an-ellipsoid-and-a-point)
- [Create a model of just a single arm](#create-a-model-of-just-a-single-arm)
- [Kinematically connect a human model to the environment](#kinematically-connect-a-human-model-to-the-environment)
- [Drive successfully a model with the manual marker method](#drive-successfully-a-model-with-the-manual-marker-method)
- [Define a constraint point on line or point on plane](#define-a-constraint-point-on-line-or-point-on-plane)
- [Use a simple text file to drive a model](#use-a-simple-text-file-to-drive-a-model)
- [How can I use the weight function ConstructWeightFunUsingResidualOnOff in a model when markers drop out?](#how-can-i-use-the-weight-function-constructweightfunusingresidualonoff-in-a-model-when-markers-drop-out-)
- [Is it possible with Anybody, to calculate the probable trajectory of the marker during this period, and to use it (in other words, to fill in the gaps)?](#is-it-possible-with-anybody--to-calculate-the-probable-trajectory-of-the-marker-during-this-period--and-to-use-it--in-other-words--to-fill-in-the-gaps--)

- [How can I measure pelvis position and rotation wrt to the femur?](#how-can-i-measure-pelvis-position-and-rotation-wrt-to-the-femur-)
- [Can I use AnyBody to make models which are moved "just" by forces?](#can-i-use-anybody-to-make-models-which-are-moved--just--by-forces-)
- [How can I solve the kinematics error if there are some soft-constraints in your model(using MaxIteration)](#how-can-i-solve-the-kinematics-error-if-there-are-some-soft-constraints-in-your-model-using-maxiteration-)
- [Steps to use BVH files to drive a model](#steps-to-use-bvh-files-to-drive-a-model)
- [My markers are placed ProcessedData Folder when loading my C3D file](#my-markers-are-placed-processeddata-folder-when-loading-my-c3d-file)


### Is it possible to introduce artificial joints, i.e. total knee replacements (TKR), or the introduction of an orthosis for the foot? Could a TKR be incorporated into the model to predict joint contact forces and ligament load sharing?
Yes. A very important point of our technology is that it is a MODELING system. This means that the users define/construct the models, and as such, the user also has complete access to make any modification of the existing repository models.
In fact, we are hoping that our users will continually add new models and modifications to the repository.

### How can the AnyBody Modeling System handle closed chains in the model?
Bicycling is a typical closed-chain example. The legs and crank together form a closed mechanical chain. 
The AnyBody Modeling System handles closed chains in the mechanism, but it is true that many other systems for mechanism analysis do not.
There are really two distinct problems connected with closed chains. The first problem is in the kinematics where closed chains complicate the solution algorithm because make it impossible to resolve the positions by stepping from one joint to the next until the free ends of the mechanism are reached. The AnyBody Modeling System employs a very general method for position analysis and does not rely on assumptions of open chains.
The second problem is in the computation of muscle forces. The classical way of doing inverse dynamics is to compute joint moments and subsequently distribute them onto the individual muscles. When there are closed chains present, the problem of resolving joint moments may not have a unique solution.
However, the AnyBody Modeling System does not go via joint moments. Instead it sets up a redundant system of equilibrium for the entire system including the muscles. This redundant system is subsequently solved uniquely via the system's advanced muscle recruitment algorithms.

### Does the system have a "welded" joint that creates a rigid connection between two segments?
Yes, the AnyStdJoint locks all mutual degrees of freedom between the segments it connects.

### What is the role of the initial positions of the segments?
The initial positions, only serves as the initial guess on the segments positions. This means that if the model assembles correctly one can be satisfied with the initial positions. They will have no influence on any analysis results. If the model is a closed chain mechanism then there might be several kinematic solutions that will fulfill the kinematic constraints on the model. In such a case the initial position can be used to ensure that the model chooses the correct solution, simply by providing initial positions which are close to the correct one.

### Why is the Pos property of a driver zero once the kinematics has been solved? 
The position of the driver should be zero when the kinematics has been solved. This is because the position displays the error on the driver. If you are looking for the position of the measure being driven, please look at the position of this measure.

### How to understand the AnyKinRotational Cardan angles (Type=RotAxesAngles) 
This small example has been made to illustrate visually the function of the AnyKinRotational running with the RotAxesAngles type. 
The RotAxesAngles is measuring the cardan angles by three angles of planar rotation about the reference system’s axes. One should be aware that the sequence
of these rotations is important, and that any rotation subsequent to the first is a rotation about a local axis arising from the previous rotation(s).

Try to run the model and notice the way the coordinate systems are rotated.

[AnyKinRotationalRotAxesAngles.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyKinRotationalRotAxesAngles.any)


### How to drive an AnyKinRotational Cardan angles (Type=RotAxesAngles) 
This small example provides an example of how to drive the rotations in a small model using RotAxesAngles measures. The example is made to display that swapping the sequence of the DOF in the driver do not change the kinematic outcome of the model. 

[[Media:DrivingAnyKinRotationalRotAxesAngles.any]]

### How to understand the AnyKinRotational Cartesian rotation vector (Type=RotVector) 
This small example has been made to illustrate visually the function of the AnyKinRotational running with the RotVector type.  The Cartesian rotation vector measures a general
3-D orientation as a single rotation about a given axis. The rotation vector is a geometric
vector that is parallel to this axis of rotation, and its length is equal to the angle of rotation. 

Try to run the model and notice the way the coordinate systems are rotated.

[[Media:AnyKinRotationalRotVector.any]]

### Define an interpolation driver

This is a small example made to illustrate the differences between the interpolation functions types. The model will create an interpolated line which is controlled by a number of control points, p1-p10.

![image](https://cloud.githubusercontent.com/assets/1038978/18738566/399fe838-809d-11e6-9b5a-f2b044f95bd5.png)
 

Try to shift between the different types and you will see the differences it makes on the path of the line. You can choose between the types:
* Piecewise linear
* Bezier
* bSpline

The types can be changed in the "Driver" object 

[[Media:InterpolationDrivers.any]]


### Create a linear combination measure
This small example illustrates how to create a linear combination measure; this is useful for creating linear combinations of measures. The example has two segments sliding on prismatic joints. One of the segments is driven using an interpolation driver, the second segment is driven to follow the motion of the first segment to some fraction, in this case half the motion. 

![linearcombination](https://cloud.githubusercontent.com/assets/1038978/18739385/82ead268-80a3-11e6-9f30-e9283353a247.gif)

[[Media:LinearCombination.avi]]

### Create point cloud joints
The AnyKinMeasureNormComb object integrates several measures into one single measure; in this example one for each grey sphere representing its distance to the red surface. 
Driving this measure to zero will enforce “contact” between one of the spheres and the surface at any time during the analysis and “contact” may shift between the spheres. An AnyKinMeasureNorm- Comb object is a norm combination measure which creates a norm-like quantity of the inputs. The measure introduces a weight factor for each input and an offset value. Using the right coefficients it will try to enforce one of the spheres to be in “contact” with the plane of surface at any time.
 
Please note that this is not a traditional surface to surface joint, but in some cases, it can be used instead. When using this measure, the parameters of the measure should be carefully adjusted. the Contact can be more or less rigid and exact depending on the parameters of the AnyKinMeasureNormComb.

![anykinmeasurecombexample](https://cloud.githubusercontent.com/assets/1038978/18739339/2c03fd1c-80a3-11e6-965e-1c70896af104.gif)


[[Media:AnyKinMeasureCombExample.any]]

### Understand the available settings for the rotational measure types
Please feel free to add

### Different ways to add a force 
Please feel free to add


### Understand the available settings of the AnyKinLinear

The AnyKinLinear measure is one of the most used objects when creating an anyScript model, so it is very important to be familiar with its various settings. The measure is used for measuring the position vector between two reference frames. The linear measure has a reference setting which is used to determine which reference system it will use for the measurements. 

The reference “Ref” has the following possible settings:
* 	Ref=-1 (default) [[Media:AnyKinLinearDefault.avi]]
* 	Ref=0 This means that the first mentioned reference frame will be used as the basis [[Media:AnyKinLinearRef0.avi]]
Notice that the trajectory line (blue) is aligned with the blue x axis
* 	Ref=1 This means that the second mentioned reference frame will be used as the basis [[Media:AnyKinLinearRef1.avi]]

Notice that the trajectory line (blue) is aligned with the green x-axis

The sample file is a small model made to illustrate the function of the Ref setting in the AnyKinLinear measure. The model has one segment, a rotational measure with a driver and a linear measure with a driver. Please find the linear measure "lin" and try out the difference of having the three different Ref settings.
The yellow coordinate system is the global reference system, the blue is a rotated node located at the origin of the global reference system. The green coordinate system is a moving coordinate system. When shifting between the different settings, please notice how this changes the motion of the green coordinate system.

For more details on how to use the AnyKinLinear please also see the tutorial http://www.anybodytech.com/677.0.html


[[Media:AnyKinLinear.any]]


### Create a joint between an ellipsoid and a point
It is possible to create joints between ellipsoids and plane surfaces or points using the facilities of the AnyScript language. There are basically two ways to accomplish this.

The first method relies on the fact that the distance from the first focal point of the ellipsoid to any point on the surface and back to the second focal point is constant and equal to 2*a, where a is the length of the first principal axis of the ellipsoid. This can be described by means of AnyKinPLine measures, but it has the drawback that only ellipsoids with b = c, where b and c are the second and third principal axis lengths can be modeled.
 
The picture shows a sketch of the joint.

[[Media:EllipsoidJointDemonstration.any]]
[[Media:EllipsoidJoint.any]]

The second method is more general and implements the equation of an ellipsoid directly. It uses a linear measure between the center of the ellipsoid and the point. The three components of this measure are inserted into a Norm Combination Measure, i.e. a measure that computes the powersum of its components. If we use the power of 2 and use the ellipsoid axis lengths as weight factors on the three components, then we can exactly express the ellipsoid equation:

$$x^2/a + y^2/b + z^2/c = 1.$$

Here's an example model.
[[Media:Ellipsoid.any]]

### Create a model of just a single arm

It is possible to include just the arm model, but it is not recommended. The explanation is that the muscles in the arm in such complicated 3D model are attaching to so many places in the thorax model that the two model parts are too hard to split apart compared to the possible gain. 


### Kinematically connect a human model to the environment
When trying to analyze complex models like human-product integrated models usually the human model will be partly driven by the product that it is connected to. 

When connecting the body to external devices, extra constrains will be added through the connections, it is up to the user to define this, basically you need to add to remove as many dof as you are adding, but unfortunately simple dof counting will not be enough. You have to make sure that each dof is not underdetermined or over-determined.  It does not work to have too many constraints on the hand and none on the foot for example, this is not a task the system will do automatically. 

The best way to do this is to make a simple stick figure sketch and then try to imagine which external connections will drive the individual human joints. Of course all this require a preliminary knowledge of the kinematic model of the human joints. In the mannequin.any files you can get a quick overview of the individual dof in the entire human model.

But basically, if you remove a driver for the elbow flexion you need to replace this by some other driver that can driver this dof.

When connecting the environment and the human model it is a good idea to use this strategy.

* Create a model only containing the environment
* Add drivers for the joints 
* Ensure that it runs kinematically
* Introduce the human model
* Adjust initial positions to match environment 
* Add drivers for the model using the drivers from for example the FreePostureModel
* Ensure the model runs kinematically
* Gradually connect the human model and the environment, one DOF at the time, when adding a constraint between human and environment, remove a driver for a human joint which is controlling the "same" DOF.
* For each change ensure that the model runs kinematically

### Drive successfully a model with the manual marker method
AnyBody models can be driven by markers with two different methods. An automated method called GaitApplication2 is available but for models of the lower extremities only. Models containing the trunk or the upper extremities or with some other particularity have to be driven by the manual setup.
In such model there are two sets of marker, one set is the body markers (belonging to the segments) and the other set is the markers recorded by the motion capture system. The aim is to hook up the two sets in a certain way so that the body follows the recorded motion but at the same time over constraining the model must be avoided. This is done by hooking up the markers in some wisely selected directions only.

The very first step is to verify that the body marker configuration corresponds to the recorded marker configuration. Then the model has to be scaled according to the anthropometrical data available for the trial.

The second step is to set the initials conditions correctly. To do so the joint drivers from the FreePosture model should be used. With the Mannequin file (controlling the drivers) the position the body should be set in the same initial position as the recorded markers. At the same time adjust the body markers placement so that the two sets of markers match pretty well when running SetInitialConditions.

The third step is to hook up the two sets of markers. As said previously the markers will be driven only in some selected direction. It is recommended to make some quick sketches in order to select the appropriate directions to constrain. The rule is that every DOF of the model must be driven by one constraint only (the total number of constraints must be equal to the total number of DOF of the model). An example of such sketch is available here [[Media:LegSketch.pdf]]. When this selection is done you should free one DOF at a time and replace the joint driver by the corresponding marker hooking driver, beginning preferably by the proximal segments like pelvis or thorax. For each new DOF freed make sure that the system can solve the kinematic constraints by running SetInitialConditions. If this is not the case then some adjustments are needed in either the initial position or body marker placement or even in the driver (direction of the constraint etc).
Once all the markers are hooked up, and the SetInitialConditions run successfully the model is ready to perform the kinematic analysis.
For an example of such a model please see the GaitVaughan or wheelChairRancho model.


### Define a constraint point on line or point on plane
Here are two examples of how to define such constraints, with the line and plane defined by AnyRefNodes moving during the simulation.
[[Media:Constraint_point_on_line.any]] [[Media:Constraint_point_on_plane.any]]

### Use a simple text file to drive a model
This is a small example that uses a simple text file to create the motion in the model.
[[Media:AnyFunInterPol.zip]]


### How can I use the weight function ConstructWeightFunUsingResidualOnOff in a model when markers drop out? 
When you insert the AnyInputC3D template from the Class Tree you will have to change the following weight function to “ on” as described in the tutorial:

ConstructWeightFunUsingResidualOnOff = On;

Then include the following command in the CreateMarkerClass.any, or change the existing weight function

AnyKinDriverMarker Driver = {

// WeightFun={&.WeightFun};
WeightFun={&Main.ModelSetup.C3DFileData.Points.Mar kers.MarkerName.Weight};

There is a CreateMarkerClass.any and a CreateMarkerClassTD.any depending on what model you use, you have to edit each (or both).


###  Is it possible with Anybody, to calculate the probable trajectory of the marker during this period, and to use it (in other words, to fill in the gaps)? 
AnyBody cannot manipulate those trajectory lines. Vicon might actually be able to do that in the C3D file.


###  How can I measure pelvis position and rotation wrt to the femur? 
This can be done in the following ways 

Method1 :

```c++
AnyKinLinear FemurToHipLin ={
  Ref=0; //measure in first listed ref frame 
  AnyRefNode  &femur=….Thigh.node;
  AnyRefNode  &pelvis=….Pelvis.node;
};


AnyKinRotational FemurToHipRot ={
  AnyRefNode  &femur=….Thigh.node;
  AnyRefNode  &pelvis=….Pelvis.node;
  Type=RotAxesAngles;
};
```

Method2 :


AnyKinLinear same as before:


Rotational measure reads out in Euler parameters is a special form of quaternions, please see AnyKinRotational in the manual, it will give four parameters as output.


AnyKinRotational FemurToHipRot ={

AnyRefNode  &femur=….Thigh.node;

AnyRefNode  &pelvis=….Pelvis.node; 

Type=EulerParam;

};

Method3:


The small equation will provide the rotation matrix between femur and  pelvis.


AnyMatrix FemurPelvisRot = .Femur.Axes’*.pelvis.Axes


### Can I use AnyBody to make models which are moved "just" by forces? 
Yes and no,

By default the AMS runs inverse dynamics, so motion is input, and applying a force will not change this motion.  

With the release of AMS 5.0 a new solver type was introduced which enables the solver to find the quasi-static equilibrium for selected DOF in the model, we call this force dependent kinematics (FDK). 

Basically, it finds the equilibrium for the degrees of freedom that you have specified as 'ForceDep'. It is a static equilibrium in the sense that the solver assumes accelerations and velocities of these degrees of freedom to be zero. 

The small sample model display here illustrates the principle
[[Media:FDKDisplayModel.any]]

For more details on the FDK capabilities and limitations, please see the section on FDK in the tutorials and manuals.

### How can I solve the kinematics error if there are some soft-constraints in your model(using MaxIteration)
In your model, there may be some soft-constraints such as GaitFullBody or GaitLowerExtremity models.

For instance, in your own GaitFullBody example with your own C3D files, you may have difficulties to pass the 'MotionAndParameterOptimizationModel' process.

In the 'AnyBodyStudy' class, there are some options that you can touch about the kinematics.
InitialConditions.KinematicTol
InitialConditions.MaxIteration
Kinematics.KinematicTol
Kinematics.MaxIteration

We would not recommend you to reduce 'KinematicTol' values because it will reduce the accuracy of your analysis models.

Instead, if you increase 'MaxIteration' value more than its default value, the kinematic solver of AnyBody will do more trials to find the kinematic solution.


###  Steps to use BVH files to drive a model

1. Start with the FreePosture model as the base model.

2. Load the bvh model into your model.

3. Adjust the start and end time of your study to use the time values from the the bvh file.

4. Set the construct model in the bvh object to be "On" this will create a stick figure of your model.

5. Set the TranslationScale and RotationScale to the correct values depending on the bvh file you have.

6. Make sure it solved the kinematics before moving on.

7. Now the task is to hook up the human model onto the BVH stick figure, it is a good idea to do this is small steps always having a model which can solve kinematically.

8. Start with the pelvis, remove the current driver for the pelvis and replace it by a driver which connects the pelvis to the pelvis equivalent segment in the BVH stick figure.

9. Then move on to the thorax remove the PelvisThroax rotation driver it provides three rotations that you then need to replace by something else. One option could be to create a rotational driver between the thorax and the equivalent segment in the BVH stick figure then drive these rotations.

10. Now you should have a model where the trunks move with the stick figure and the arms and legs are just kept in a fixed pos.

11. The next steps is to hook up the arms and legs in a similar fashion (One way of doing this would be to create a six dof driver between the hand of the human model and the hand of the stick figure, and then add one additional driver, again you need to remove the same amount of drivers as you add).


###  My markers are placed ProcessedData Folder when loading my C3D file 
Sometimes the markers end up in the processed data folder when they have been read into AMS using the AnyInputC3D, this is because the markers has been tagged as processdata when the C3D file was generated. The easiest way to make it possible to use the markers for drivers is  to move them into the marker folder. This can easily be fixed by setting the MarkerUseAllPointsOnOf to "On" the default is "Off", this will read also the processed data as markers. If you do not want all the data to figure as markers you can in addition create a list of the markers which should be used and in this way exclude unwanted data from the markers. The list can be set with the  MakeNameUniqueStr item. One last option is to change the property MarkerUseCamMaskOnOff which has this function "Switch for using the Camera Mask for deciding which point data as 3D marker data. Non-zero camera mask is interpreted as 3D marker data"
