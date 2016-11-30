## Why are models from AMMR 1.5 or before not loading anymore in AMS 6.0?

In AMS 6.0 (or later) the evaluation of kinematic measures has been changed to make the computations more efficient. Therefore, one feature of kinematic measures has been changed: the member OutDim describing the number of components in a kinematic measure which was an optional member of each kinematic measure has become a mandatory member in AMS 6.0. Since it was optional, the OutDim had not been set in all places in the older repositories.

## How can models based on AMMR 1.5 or earlier be fixed for AMS 6.0?

As an example we have a look at the StandingModel example from the AMMR 1.5.1.

When we load the main file StandingModel.Main.any in an AMS 6.0, we can see that we get a list of warnings of the type

```
 WARNING(SYS6) :   G:/AMMR/AM..1/Body/A..n/Trunk/JointsLumbar.any(65)  :   Measure.OutDim  :  Changed usage :  initialization of OutDim was previously (v.5.x) not needed in all cases, but its value is now required.
 Currently, it has an implicit value of '1' if not set, but it is strongly recommended to specify it anyway. Obligatory initialization requirement will come in future versions.
 WARNING(SYS6) :   G:/AMMR/AMMR1.5.1/Body/AAUHuman/Trunk/Disc.any(25)  :   LinCombRight.OutDim  :  Changed usage :  initialization of OutDim was previously (v.5.x) not needed in all cases, but its value is now required.
 Currently, it has an implicit value of '1' if not set, but it is strongly recommended to specify it anyway. Obligatory initialization requirement will come in future versions.
```

and finally an error

```
 Configuring model...
 ERROR(OBJ1) :   G:/AMMR/AM..1/Body/A..n/Trunk/JointsLumbar.any(81)  :   SpineRhythmDrv.Reaction.Type  :  Number of elements in type specification must match number of elements in force vector (num. elem = 1)
 Model loading skipped
```

To fix this model, we will try to fix the error messages and deal with the remaining warnings later. The error message tells us that we have a mismatch between the output dimension of the measure and the force vector using this measure. Simply speaking it tells us that at this point an OutDim specification is missing. When we click on the link of the file location in the error message we will jump to the place where the OutDim definition is missing:

```
 //Spine rhythm leaves three rotational dof between pelvis and thorax free 
 AnyKinEqSimpleDriver SpineRhythmDrv = {
   
   AnyKinMeasureLinComb Measure = {
     
     AnyJoint &u1 = ..SacrumPelvisJnt;
     AnyJoint &u2 = ..L5SacrumJnt;
     AnyJoint &u3 = ..L4L5Jnt;
     AnyJoint &u4 = ..L3L4Jnt;
     AnyJoint &u5 = ..L2L3Jnt;
     AnyJoint &u6 = ..L1L2Jnt;
     AnyJoint &u7 = ..T12L1Jnt;
     
     #include "SRMatrixes.any"            
     
   }; // Measure
   
   DriverPos = {0,0,0, 0,0,0, 0,0,0, 0,0,0, 0,0,0, 0,0,0};
   DriverVel = {0,0,0, 0,0,0, 0,0,0, 0,0,0, 0,0,0, 0,0,0};
   Reaction.Type = {Off,Off,Off, Off,Off,Off, Off,Off,Off, Off,Off,Off, Off,Off,Off, Off,Off,Off};
   
   
 }; // SpineRhythmDrv
```

To fix this error, we add the correct OutDim into the kinematic measure, which is in this case an AnyKinMeasureLinCom. In this case, the OutDim is 18 which we can figure e.g. by counting the entries in the DriverPos vector in this case.

```
 AnyKinEqSimpleDriver SpineRhythmDrv = {
   
   AnyKinMeasureLinComb Measure = {
     
     AnyJoint &u1 = ..SacrumPelvisJnt;
     AnyJoint &u2 = ..L5SacrumJnt;
     AnyJoint &u3 = ..L4L5Jnt;
     AnyJoint &u4 = ..L3L4Jnt;
     AnyJoint &u5 = ..L2L3Jnt;
     AnyJoint &u6 = ..L1L2Jnt;
     AnyJoint &u7 = ..T12L1Jnt;
     
     #include "SRMatrixes.any"            
     OutDim = 18;
   }; // Measure
```

This fixed the first error.

When we try to load the model again, we can see that we get another error message point to the same problem, just in another location. In this case, the link in the error message points to an AnyKinEQSimpleDriver:

```
 AnyKinMeasureLinComb LinComb = {
   AnyKinMeasure &u1 = .HumerusScapulaRot;
   AnyKinMeasure &u2 = .HumerusDeltoidMuscleConnectorRot;
   Coef={
     {0.3,0,0,-1,0,0},
     {0,0.05,0,0,-1,0}, //this one controls the rotaion around the long axis og humerus
     {0,0,0.3,0,0,-1}
   };    
   
 }; // Measure
 
 //This is not an engine no muscles are attached to the control segment
 //and the deltoidmuscleconnector segment is only driven kinematically 
 //all reactions are swithed off
 AnyKinEqSimpleDriver LinCombDrv={
   AnyKinMeasureLinComb &ref=.LinComb ;
   DriverPos={0,0,0};
   DriverVel={0,0,0};
   Reaction.Type={Off,Off,Off};  
 };
```

Here, we have to do a similar thing. The kinematic measure in this driver is now a reference to a kinematic measure. We follow the reference to the driver (which in this case is defined just above the AnyKinEqSimpleDriver) and define the correct OutDim similar to the fix we made before. In this case we look at the DriverPos vector in the driver and set the OutDim to 3.

```
 AnyKinMeasureLinComb LinComb = {
   AnyKinMeasure &u1 = .HumerusScapulaRot;
   AnyKinMeasure &u2 = .HumerusDeltoidMuscleConnectorRot;
   Coef={
     {0.3,0,0,-1,0,0},
     {0,0.05,0,0,-1,0}, //this one controls the rotaion around the long axis og humerus
     {0,0,0.3,0,0,-1}
   };    
   OutDim = 3;
 }; // Measure
 
 //This is not an engine no muscles are attached to the control segment
 //and the deltoidmuscleconnector segment is only driven kinematically 
 //all reactions are swithed off
 AnyKinEqSimpleDriver LinCombDrv={
   AnyKinMeasureLinComb &ref=.LinComb ;
   DriverPos={0,0,0};
   DriverVel={0,0,0};
   Reaction.Type={Off,Off,Off};  
 };
```

Pressing the load button again, we see that we still get a list of warnings but no error messages and the model was loaded successfully.

## How can I get rid of warnings about missing initialization of OutDim?

To get rid of the remaining warnings about a missing initialization of OutDim, we follow a similar precedure as for the error messages: First we click the link to the file location in the warning message to jump to the AnyScript code which producing the warning. If we take the first error in the list, we jump to a code snippet that looks like this:

```
 AnyKinMeasureLinComb LinCombRight = {
   AnyKinMeasure &u2 = ..LinRight;
   AnyKinMeasure &u1 = .ControlMeasureRightY;
   Coef={
     {..Ratio,-1}
   };      
 };
```

We now have to add an OutDim to this kinematic measure. To figure out the right dimension, we right-click the name of the measure LinCombRight in select Locate In Model Tree in the pop-up menu. 

![image](https://cloud.githubusercontent.com/assets/22542671/20758548/75df30d4-b71a-11e6-8c05-fb5716dc2f6f.png)

*In the model tree, we unfold the measure and select the the Pos member to see that the output of this measure has dimension 1. Thus, we add the correct OutDim to the AnyScript definition of the measure*

```
AnyKinMeasureLinComb LinCombRight = {
   AnyKinMeasure &u2 = ..LinRight;
   AnyKinMeasure &u1 = .ControlMeasureRightY;
   Coef={
     {..Ratio,-1}
   };   
   OutDim = 1;
 };
```

A click on the load button shows us that we eliminated one of the warnings. In a similar way, we can now fix all the other warnings.
