1. Open a new Main file using a ”Standing Man” template
2. To make it run quicker turn of the arms for now. include:

```
 #define BM_ARM_LEFT OFF
 #define BM_ARM_RIGHT OFF
```

right after

```
   #path BM_MANNEQUIN_FILE "Model\Mannequin.any"
```

Now let’s make it do a knee bend

3. Add drivers to make the Kneebend open the #include "Model\JointsAndDrivers.any" file by double clicking on it. add following lines for easier reference:
```
 AnyFolder &LegR = ..HumanModel.BodyModel.Right.Leg;
 AnyFolder &LegL = ..HumanModel.BodyModel.Left.Leg;
```

Let's fix the right foot to the ground, and add following code into the Drivers folder:

```
 // Place the right toe and heel on the ground
 AnyKinEqSimpleDriver RToeGroundConstraint ={
   AnyKinLinear ToePos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ball = Main.Model.HumanModel.BodyModel.Right.Leg.Seg.Foot.ToeJoint;
   };
  MeasureOrganizer = {1};  // Only the y coordinate
   DriverPos = {0.0};
   DriverVel = {0};
   Reaction.Type = {Off};   // Provide ground reaction forces
 };
  AnyKinEqSimpleDriver RHeelGroundConstraint ={
   AnyKinLinear HeelPos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ball = Main.Model.HumanModel.BodyModel.Right.Leg.Seg.Foot.HeelNode;
   };
   MeasureOrganizer = {1};  // Only the y coordinate
   DriverPos = {0.0};
   DriverVel = {0};
   Reaction.Type = {Off};   // Provide ground reaction forces
 };
   // Position the Ankles right above the z axis
 AnyKinEqSimpleDriver RAnkleX = {
   AnyKinLinear AnklePos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ankle = ..LegR.Seg.Foot.SubTalarJoint;
   };
   MeasureOrganizer = {0};  // Only the x coordinate
   DriverPos = {0.0};
   DriverVel = {0.0};
   Reaction.Type = {Off};
 };
```

And the same for the left foot:

```
 // Place the left toe and heel on the ground
   AnyKinEqSimpleDriver LToeGroundConstraint ={
    AnyKinLinear ToePos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ball = Main.Model.HumanModel.BodyModel.Left.Leg.Seg.Foot.ToeJoint;
    };
   MeasureOrganizer = {1};  // Only the y coordinate
   DriverPos = {0.0};
   DriverVel = {0};
   Reaction.Type = {Off};  // Provide ground reaction
 };
 AnyKinEqSimpleDriver LHeelGroundConstraint ={
   AnyKinLinear HeelPos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ball = Main.Model.HumanModel.BodyModel.Left.Leg.Seg.Foot.HeelNode;
   };
   MeasureOrganizer = {1};  // Only the y coordinate
   DriverPos = {0.0};
 DriverVel = {0};
   Reaction.Type = {Off};  // Provide ground reaction
 };  
 // Position the Ankles right above the z axis
 AnyKinEqSimpleDriver LAnkleX ={
   AnyKinLinear AnklePos = {
     AnyFixedRefFrame &Ground = Main.Model.Environment.GlobalRef;
     AnyRefNode &Ankle = ..LegL.Seg.Foot.SubTalarJoint;
   };
   MeasureOrganizer = {0};  // Only the x coordinate
   DriverPos = {0.0};
   DriverVel = {0.0};
   Reaction.Type = {Off};
 };
```

4.	To make it more realistic, change the model so that AnyBody predicts ground reaction forces

Add this code called conditional contact to the same Joints and Drivers file:

```
 ConditionalContactFootPlaneClass RightFootSupport (
 BaseObject = Main.Model.Environment.GlobalRef,
 Foot = .LegR.Seg.Foot,
 DisplayTriggerVolume = 0,
 DisplayTargetNode =1
 ) = {
   UserDefinedLimitLow = -0.05;
   UserDefinedLimitHigh = 0.05;
   UserDefinedRadiusLimit = 0.4;
   Strength = 2000;
   StaticFrictionCoefficient = 0.8;
   NormalDirection = Y;
   FrictionDirection1 = X;
   FrictionDirection2 = Z;
 };
```

```
   ConditionalContactFootPlaneClass LeftFootSupport (
   BaseObject = Main.Model.Environment.GlobalRef,
   Foot = .LegL.Seg.Foot,
   DisplayTriggerVolume = 1,
   DisplayTargetNode =1
   ) = {
     UserDefinedLimitLow = -0.05;
     UserDefinedLimitHigh = 0.05;
     UserDefinedRadiusLimit = 0.4;
     Strength = 2000;
     StaticFrictionCoefficient = 0.8;
     NormalDirection = Y;
     FrictionDirection1 = X;
     FrictionDirection2 = Z;
   };
```
 
To use this conditional contact, you need to include the classes at the beginning of the Main file:

```
    #include "../libdef.any"
    #include "<ANYBODY_PATH_TOOLBOX>\FrictionContactMuscles\ConditionalContactClass.any"
    #include "<ANYBODY_PATH_TOOLBOX>\FrictionContactMuscles\ConditionalContactFootPlaneClass.any"
    Main = {...
```

5. Now change the velocities for each joint to make the body model do a knee bend (similar to tutorial 1) Therefore you need to open and edit

```
   #path BM_MANNEQUIN_FILE "Model\Mannequin.any"
```