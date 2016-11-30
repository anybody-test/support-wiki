## Where do I change the cost function for the muscles?

This is done in the AnyBodyStudy object of the model, it has several options available. 
When looking at a loaded model in the ModelTree these settings are placed in the  
InverseDynamics.Criterion folder inside the study. 

To change the cost function: find the AnyBodyStudy object and write for example "InverseDynamics.Criterion = MR_MinMaxStrict"

Please refer to the section in reference manual on AnyBodyStudy, for the details on the different settings.

Please also see the tutorial "Inverse Dynamics of Muscle Systems" 

The picture below provides an overview of the different settings. 

The criterion is to minimize "Primary Term" + "Auxiliary Terms". By changing the terms "Criterion.AuxLinearTerm.Weight" and "Criterion.AuxQuadraticTerm.Weight" the influence of the auxiliary terms can be altered.

If looking at for example MR_MinMaxStrict it can be seen that this criterion do not have any auxiliary terms available, whereas the MR_MinMaxAux has this.

![recruitmentoverview](https://cloud.githubusercontent.com/assets/22542671/20746344/ad7e4316-b6e5-11e6-8b41-501a669b557c.png)

## Does the spine rhythm impose any stiffness on the model?
The spine rhythm matrix does not represent the stiffness of the spine. Actually the spine model has no passive stiffness implemented at the moment. The SRmatrices.any file which defines the spine rhythm is for the kinematics only. The coefficients in the matrix specify the rotation of each lumbar vertebra to a certain fraction of the rotation of T12/Pelvis. The T12/Pelvis rotation is the input of the lumbar driver. The SRMatrixes define the shape of the spine between T12 and the pelvis. 

## Why is it ok to have a reaction force between the pelvis and environment if you have measured ground reaction forces?
If you have a full body model you will also need a reaction to the environment to take up residual forces, in principle this residual should be zero but this will never the cased due to differences in kinematics, anthropometry, and skin artefacts, measuring noise also plays a role. If looking at a model with only the legs and the pelvis the issues are the same but in this case the reaction force will carry both the residual and the missing forces between the pelvis and the upper body.

In a MoCap driven model, the residual force can be found in the ModelEnvironmentConnection:

Main.Studies.InverseDynamicStudy.Output.ModelEnvironmentConnection.JointsAndDrivers.JntDriverTrunk.Reaction.Fout

## How should the force output from the joint be interpreted?
All joints are equipped with a folder "Constraints.Reaction.RefFrameOutput"  this folder contains the force and moment in the reference frames used by the joint.

These forces and moments are given in the global coordinate system, and can be transferred into local by multiplying by the rotation matrix of the reference frame in question. So for example:
```
 AnyRevoluteJoint Jnt ={
    AnyFixedRefFrame &ref = .global;
    AnyRefNode &ref2 = .Mass.node1; 
    AnyVec3 M_local = Constraints.Reaction.RefFrameOutput.M[0] * ref.Axes ;
 };
```
Here M_local would be the reaction force in the first frames coordinate system

If you are unsure what direction the X-Axis represents, it is advisable to draw a RefFrame in the Joint Node. The first node mentioned in the joint is used to define the reference frame for the output of forces and moments. Example for the Knee:

```
  Main.Studies.HumanModel.BodyModel.Right.Leg.Jnt.Knee.ThighNode = {
  
  AnyDrawRefFrame KneeJointRefFrame ={
  
  RGB = {0.91796875, 0.76953125, 0.06640625};
  
  ScaleXYZ = 0.2 * {1, 1, 1};
  
  };};
```

## I have a different force plate setup than used in the applications of the repository what should I do?
Basically you need to model this in the “environment.any” file, but first you need to look at the definition of your forceplate at www.c3d.org , in the manual you will find a forceplate section, there are several types defined. You will need to find yours and review its properties.

One very important thing to remember is that the sequence of the feet on the plates must fit with the experiment so if right foot is on the first plate, this should be reflected in the
AnyReacForce between the right foot and the first plate.

Applying the forces you need to double check that the reference frame you are using for applying the force has a orientation according to the lab setup, this is of course essential!

## How is it possible that the model will calculate automatically the reaction forces with the environment?
The system computes reaction forces automatically. If your model is supported in a statically determinate fashion, then there is only one unique set of reaction forces that balances the system correctly, and AnyBody will compute if for you.
What happens then if the system is not statically determinate? Just to give you an idea of such a situation, consider a body standing on hands and knees on the floor. It needs only three support points to be stable, but it has four points. AnyBody handles this case through its muscle recruitment. There are infinitely many combinations of the four support forces that balance the model, and the human body distributes the force between the two hands and two knees by contracting and relaxing muscles until a reasonable equilibrium is attained. You can think of a situation where the majority of the support force was carried by one of the hands. The body is likely to feel that this hand is overloaded and seek to redistribute the force to the two legs and the other hand until a more even load distribution was found. This is exactly what the AnyBody Modeling System does. It looks at all the muscle loads in the system and finds the distribution of muscle forces that optimizes the load distribution.
In some cases it can improve the accuracy of the analysis tremendously if you have measured reaction forces available and impose them on the model rather than let the system compute them. This is particularly the case for gait and other very dynamic problems where a large part of the reaction force comes from balancing of inertia in the system. If you are modeling a measured gait cycle, then you have essentially measured positions, and they must be differentiated twice to get the accelerations the system needs to resolve the inertia forces.

## Can elastic elements (ligaments, cartilage..) be modelled?
You can actually specify ligaments and other (passive element) forces that depend on position and velocity (in general springs and dampers with almost any explicit expression you can come up with). In particular, we have a ligament object class which is specially 
designed for ligament, but in general it is nothing else than an applied force.
The downside is that the ligament forces cannot be a function of the muscle forces in this way, since they merely enters the inverse dynamics solution as applied forces.
We hope to resolve this problem in a future version. One could currently model ligaments as simple muscles. This has, as far a I know, been done in some studies in the literature.


## How do you specify the center of pressure during the gait?
The ground force is measured in the gait experiment. It is measured by a force plate and the measurement is converted into a 3D force vector and the associated point of application (center of pressure). Pressure data measure by any other measurement system can similarly be converted into point loads. A point load is adequate in the model, where the foot is a single rigid segment. The position of the center of pressure is defined in the data set. We simply create a force plate (as a segment) in the model and add the measured forces to this segment. The position of the force plate is controlled by the measured data. The contact between the foot and the force plate is made using a reaction force that transfers all forces applied to the force plate to the foot.

## Does the AnyBody Modeling System compute joint moments?
In principle it does, since it completely resolves all mechanical aspects of the problem. But you are probably thinking whether the system initially computes joint moments and then proceeds to distribute them on the muscles spanning each joint. This is not the way the AnyBody Modeling System does it. Contrary to popular belief, it is really not a viable solution. The explanation is that because we have many bi-articular muscles, there is not unique solution to the joint moment distribution problem. Depending on the actions of the bi-articular muscles, net moment may be transferred from one joint to the other.
Instead, the AnyBody Modeling System sets up a redundant system of equilibrium for the entire system including the muscles. This redundant system is subsequently solved uniquely via the system's advanced muscle recruitment algorithms.
But the fact of the matter is that for joints with bi-articular muscles. the concept of joint moments really does not exist.

## I have a body model that is supported by the floor/a chair/a bed/a bicycle/... Do I have to specify the reaction forces or can the model compute them automatically?
The system computes reaction forces automatically. If your model is supported in a statically determinate fashion, then there is only one unique set of reaction forces that balances the system correctly, and AnyBody will compute if for you.
What happens then if the system is not statically determinate? Just to give you an idea of such a situation, consider a body standing on hands and knees on the floor. It needs only three support points to be stable, but it has four points. AnyBody handles this case through its muscle recruitment. There are infinitely many combinations of the four support forces that balance the model, and the human body distributes the force between the two hands and two knees by contracting and relaxing muscles until a reasonable equilibrium is attained. You can think of a situation where the majority of the support force was carried by one of the hands. The body is likely to feel that this hand is overloaded and seek to redistribute the force to the two legs and the other hand until a more even load distribution was found. This is exactly what the AnyBody Modeling System does. It looks at all the muscle loads in the system and finds the distribution of muscle forces that optimizes the load distribution.
In some cases it can improve the accuracy of the analysis tremendously if you have measured reaction forces available and impose them on the model rather than let the system compute them. This is particularly the case for gait and other very dynamic problems where a large part of the reaction force comes from balancing of inertia in the system. If you are modeling a measured gait cycle, then you have essentially measured positions, and they must be differentiated twice to get the accelerations the system needs to resolve the inertia forces upon which the ground forces will eventually depend. Every time you differentiate measured data you increase the inevitable measurement inaccuracy by an order of magnitude, so ground reaction forces are not strictly necessary, but they help a lot for dynamical problems.

## Can i constrain the reaction forces to have a certain direction?
It is actually possible to restrict the directions of the reaction  forces to be within a certain direction, if they are created with the AnyGeneral muscles, it can not be done directly in the definitions of the constraints.

This exactly what is done in the GH joint in the shoulder!. The GH joint is a spherical joint, which reactions have been switched off.  Switching  off the reaction is possible in all joints, by changing the following property. '''Constraints.­Reaction.­Type''', by default all its components will be '''on'''  but you can remove reactions individually from the joint. 
For a revolute joint settings like '''Constraints.­Reaction.­Type={off,­off,off,on,­on};''' will mean that all three forces reactions are switched off whereas the moment reactions is still '''On'''. In the GH joint we wanted to constrain the direction of the reaction force to fall within the glenoid cavity of scapula. To ensure this, eight pushing muscles now acts from the gh joint rotation center onto the edge of the
glenoid cavity, to try to picture this setup it resembles the frame of a tipi tent!.
  
Since each of the muscles can only push there is no way that the resultant force can fall outside the cavity of the glenoid cavity. 
 
The setup of the gh reactions can be found in the file body/aauhuman/­arm/ghreaction.­any, this is a good example on how to implement this.

## How can i constraint the magnitude of a reaction force?
This is basically not possible, but by redefining the reaction using an AnyGeneral muscle you can do something which may resemble it.
If the reaction is replaced by a muscle which is very strong the activity of this muscle will not affect the activation of the regular muscles in the human model. If the strength of the muscle is gradually lowered the recruitment of the regular muscles in the body will gradually be affected. Generally speaking if the strength of the reaction is low enough the model will feel it like it is standing on a needle and it will try to recruit all muscles in a way to avoid loading this reaction, if this is possible.
So by adjusting the magnitude of the strength of this muscle different results can be obtained, but it is not possible to have a certain upper bound on the magnitude.





## Adding stiffness in joint
This small example showing how to add "rotational spring" in a joint using an interpolation function and an AnyForce object. 

[StiffnessInJoint.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/StiffnessInJoint.any)


## Understand the reactions in the Glenohumeral Joint== 
The glenohumeral joint is a bit tricky because of the special reaction applied in the gh joint.
This reaction ensures that the reaction force always falls within the glenoid cavity.

To get an overview you first of all have a look at the Jnt.any file in the Arm folder of the AAUHuman folder in the repository. Here you will find that the reaction in the spherical gh joint has been switched off and instead these reactions are replaced by pushing muscles, defined in the file ghreaction.any.

The gh reaction is created by eight pushing muscles which all point into the GH rotation center and originate from the edge of the glenoid. This setup ensures that the gh reaction force will always fall within the glenoid fossa. The FTotal property is a summation of these forces.

## Measure the reaction force in a joint in a certain coordinate system
When looking at a certain joint in the model, how is it possible to read out the force of this joint measured in a user-defined coordinate system?

The force in a joint is by default measured in the coordinate system of the first mentioned reference frame in the joint definition. 

But how can you read out the force in a different coordinate system? It can be changed in two ways: by modifying the joint itself so that the first-mentioned node is the one of interest or by doing some calculations on the reaction forces. 

Usually it is not recommended to change the joint itself because it may have many implications on the model; it is safer to rework the output. 

The first thing to do is to create a AnyForceMomentMeasure which is an object which can be used for measuring forces and moments which are applied to a certain reference frame. The output from this object is a force and moment measured in the global reference frame. 

These following lines in an example of it use, there is a reference to the forces we want to measures in this case the reactions from a joint, and there is a reference to the node we would like to measure this force in. 
```
  AnyForceMomentMeasure JointReactionMeasure =  {
      AnyForceBase &ref1=.jnt.Constraints.Reaction; //this is a reference        to the constraints of the joint
      AnyRefNode &ref2=.Arm.node1;
    };
```
Output is as mentioned above a force and moment vector measured in globalref, so if we would like to have the vector displayed in another coordinate system we have to multiply by this coordinate systems rotation matrix. An example of this is shown here:
 ```
    Vec = .JointReactionMeasure.F*.Arm.node1.Axes;
 ```
In the sample file named AnyForceMomentMeasure.any a small example can be found that illustrates this.

File: AnyForceMomentMeasure.any

## Measure the moment created by the muscles around a joint degree of freedom in a certain coordinate system
This small example show how to use the AnyForceMomentMeasure2 to measure the moment created by the muscles around a joint degree of freedom and responsible for the motion of this joint.
[[Muscle Moment Measure example.any]](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/MuscleMomentMeasureExample.any)
