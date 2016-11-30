The AnyBody Modeling System v6.0 (AMS) and the AnyBody Managed Model Repository v1.6 (AMMR) are used in this example.

# What do you need to process your own C3D model?

1. You need a C3D file of the recorded trial: In most cases it is easy to switch between trials recorded in Vicon, Qualisys, Simi, ... Many of our users have the Plug-In-Gait marker protocol or something similar. Different protocols are also applicable. Additional markers can easily be added to the model. Force data from Force Platforms is necessary to ensure a smooth processing of the trials. It is also possible to use BVH files from XSens, Animazoo, Kinect, ...

2. Anthropometric data of the recorded subject. Weight and total height of the subject are necessary. Individual segment lengths, e.g. thigh, shank, … is helping the process, but not mandatory.

## What is the structure of the MoCap Model example?

In the folder MoCap Model are three subfolders, one for the Input (eg C3D), one for the Model and one for the Output. There are also 2 main files, one only analyzing the Lower Body, and the other one analyzing the Full Body. For increased computational time it is better to use only the lower body when looking at knee or hip for example...

Let’s get started, and open the model with the Main file MoCapModel_LowerBody.Main.any. On top, you can find some define statements, let's ignore them for now. The model is divided into two parts, the MotionAndParameterOptimization Study and the InverseDynamics Study. You can switch between both studies by changing 0 (=Off) to 1 (=On). If you want to analyze a new motion capture trial and have a new C3D file, you need to start with the MotionAndParameterOptimization first!

```
    //Set this to 1 if you want to run the motion and Parameter Optimization identification
    //**************************************************
    #ifndef MotionAndParameterOptimizationModel
    
    #define MotionAndParameterOptimizationModel 1
    
    #endif
    
    //Set this to 1 if you want to run the inverse dynamic analysis
    #ifndef InverseDynamicModel
    
    #define InverseDynamicModel 0
    
    #endif
    //Usually only have one of the two switches active so set the inactive analysis to 0
    //**************************************************
```

## MotionAndParameterOptimization

In this study, the C3D file will be loaded and used. The Markers of the recording will be used assigned to a body segment. The position of the markers, the joint axis, or joint centers will be determined to find the optimal Motion. The segment lengths will be optimized suing the recorded motion. This study is pure kinematics. The forces are not used, therefore the muscles will not be activated in this procedure. After finding the optimal motion and parameters, the joint angles will be exported into .txt files.

## InverseDynamics

In this study, the motion will come from the previously generated .txt files and the forces from the C3D file will be added. The Body Model will be used with muscles. In this study, muscle activations and forces will be calculated and therefore joint reaction forces will be calculated. The process of using Motion and external forces as input is called “inverse dynamics”.

## How to setup the model?

There are two aspects, in order to setup a new protocol or gait lab, you need to adjust the forceplates in the environment file and the markersetup in the modelsetup. If you have done that and want to simulate different trials, you need to adapt the trialspecific data.

Let's start with the Forceplates

## Let’s open the Environment.any file and have a look at it:

The standard setup is using force plates of the type 4. Please look at C3D.org to read more about force plate types. You can create your own force plates type, but the most common plates are already setup in AnyBody. If you look into your C3D under Forceplate and Type you will see how many and what type you have. If you have a different type than type 4 you have to change a couple things.

There are two different options, you can use the AutoDetection or the basic version. AutoDetection is generally used to detect which foot (left or right) is in contact with the specific force plate automatically. So you should try to use the AutoDetection. Otherwise, if you use the basic version you need to modify this file yourself. Therefore you need to know which segment of the human body is in contact with the specific force plate. So there is no concept of 'automatic detection' when you use this file.

Another important aspect is if the force plates are using a calibration matrix or not. You can see that also in the C3D file under Force_Platform (or similar).

AutoDetection:
+ What type of force plates do you have?
+ How many force plates do you have?
+ Do they have a calibration matrix?
+ What is the vertical direction of your lab coordinate system?

Basic:
+ What type of force plates do you have?
+ How many force plates do you have?
+ Do they have a calibration matrix?
+ What Body segment (eg left foot) is touching what plate?

  Here are some examples how that might look like:

AutoDetection, type 2, 2 plates, no calibration matrix, vertical direction Z

```
    ForcePlateType2AutoDetection Plate1 (
    PlateName = Plate1,
    Folder =Main.ModelSetup.C3DFileData,
    Limb1=  .BodyModelRef.Right.Leg.Seg.Foot,
    Limb2=  .BodyModelRef.Left.Leg.Seg.Foot,
    No=0,
    VerticalDirection ="Z",
    HeightTolerance=0.07,
    VelThreshold=2.2,
    Fx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fx1,
    Fy=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fy1,
    Fz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fz1,
    Mx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mx1,
    My=Main.ModelSetup.C3DFileData.Analog.DataFiltered.My1,
    Mz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mz1,
    FootPresent=HumanModelPresent)
    ={
    //  Cal=Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.CAL_MATRIX.Data[0];
    //  Cal = .Ident;
    };

    ForcePlateType2AutoDetection Plate2 (
    PlateName = Plate2,
    Folder =Main.ModelSetup.C3DFileData,
    Limb1=  .BodyModelRef.Right.Leg.Seg.Foot,
    Limb2=  .BodyModelRef.Left.Leg.Seg.Foot,
    No=1,
    VerticalDirection ="Z",
    HeightTolerance=0.07,
    VelThreshold=2.2,
    Fx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fx2,
    Fy=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fy2,
    Fz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fz2,
    Mx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mx2,
    My=Main.ModelSetup.C3DFileData.Analog.DataFiltered.My2,
    Mz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mz2,
    FootPresent=HumanModelPresent)
    ={
    //  Cal=Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.CAL_MATRIX.Data[0];
    //  Cal = .Ident;
    };

```
  
Basic, type 3, 4 plates, no calibration matrix, sequence: touching plate A with right leg, then B with left foot, then C with right and D with left foot again:

```
    ForcePlateType3 PlateB (
    PlateName = PlateB,
    Folder =Main.ModelSetup.C3DFileData,
    Limb = .HumanModelRef.Right.Leg.Seg.Foot,
    No=0,
    Fx_12=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFx1_43_2,
    Fx_34=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFx3_43_4,
    Fy_14=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFy1_43_4,
    Fy_23=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFy2_43_3,
    Fz_1=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFz1,
    Fz_2=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFz2,
    Fz_3=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFz3,
    Fz_4=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPBFz4)
    ={};
    
    ForcePlateType3 PlateA (
    PlateName = PlateA,
    Folder =Main.ModelSetup.C3DFileData,
    Limb = .HumanModelRef.Left.Leg.Seg.Foot,
    No=1,
    Fx_12=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAFx1_43_2,
    Fx_34=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAFx3_43_4,
    Fy_14=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAY1_43_4,
    Fy_23=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAY2_43_3,
    Fz_1=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAZ1,
    Fz_2=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAZ2,
    Fz_3=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAZ3,
    Fz_4=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPAZ4)
    ={};
    
    ForcePlateType3 PlateC (
    PlateName = PlateC,
    Folder =Main.ModelSetup.C3DFileData,
    Limb = .HumanModelRef.Left.Leg.Seg.Foot,
    No=2,
    Fx_12=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCFx1_43_2,
    Fx_34=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCFx3_43_4,
    Fy_14=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCY1_43_4,
    Fy_23=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCY2_43_3,
    Fz_1=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCZ1,
    Fz_2=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCZ2,
    Fz_3=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCZ3,
    Fz_4=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPCZ4)
    ={};
    
    ForcePlateType3 PlateD (
    PlateName = PlateD,
    Folder =Main.ModelSetup.C3DFileData,
    Limb = .HumanModelRef.Right.Leg.Seg.Foot,
    No=3,
    Fx_12=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDFx1_43_2,
    Fx_34=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDFx3_43_4,
    Fy_14=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDY1_43_4,
    Fy_23=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDY2_43_3,
    Fz_1=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDZ1,
    Fz_2=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDZ2,
    Fz_3=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDZ3,
    Fz_4=Main.ModelSetup.C3DFileData.Analog.DataFiltered.FPDZ4)
    ={};
```

  Please check also the name convention of your forces in the C3D file, this can be for example Fx1, Fy1, ... or only Fx, Fy, ... or F1x, F1y, .... or even sometimes Fx_1, Fy_2, ... !

## Let’s open the ModelSetup.any file and in there the marker.any file to have a look at it:

The ModelSetup file contains the marker topology of the data set. It links free floating markers with the markers on the human. Usually, there is no need to make major changes here. If you want to run your own C3D files with different marker names, you might have to change marker names and marker positions. This has to be done in the "Markers.any" file. Further instructions can be found in the file.

// How it works: // // Marker on the Left Posterior Superior Iliac // CreateMarkerDriver LPSI ( // A marker on the skeleton is created! // MarkerName= LPSI, // Change the name here to the name of your Marker Protocol!: // MarkerPlacement=Trunk.SegmentsLumbar.PelvisSeg, // Marker is assigned to Pelvis Segment // OptX="Off", OptY="Off", OptZ="On", // Is the position of the Marker good, or does it need to be optimised? // // Please be aware that having to many Markers optimised can lead to model failure // WeightX=1.0,WeightY=1.0,WeightZ=1.0, // If you are convinced of the position of this Marker, you an increase the importance of it (weight) // Model1=MotionAndParameterOptimizationModel, // Model2= InverseDynamicModel, // sRelOptScalingOnOff="On" // Determine whether sRelOpt value will be scaled by the geometrical scaling funciton or not // ) = { // sRelOpt ={-0.06727544, -0.03, -0.045}; // Position of the Marker on the skeleton, relativ to the segment. Please adjust! // sRelOptDelta = {0, 0, 0} // This value is optional. This value will be optimized and you can set the initial guess if you want. // };

// Hip/Pelvis

// Marker on the Left Posterior Superior Iliac CreateMarkerDriver LPSI ( MarkerName= LPSI, MarkerPlacement=Trunk.SegmentsLumbar.PelvisSeg, OptX="Off", OptY="Off", OptZ="On", WeightX=10.0,WeightY=10.0,WeightZ=10.0, Model1=MotionAndParameterOptimizationModel, Model2= InverseDynamicModel, sRelOptScalingOnOff="On" ) = {

```
 sRelOpt ={-0.06727544, -0.03, -0.045};
};
```

In the final part of the ModelSetup.any file all the markers are created using class templates. You can see in the code above that the Marker with a given name is here assigned to a body segment, e.g. RTHI is assigned to the right thigh segment. The RTHI marker is optimized in X, Y and Z direction (Opt_=”On”). The target marker is placed on the right thigh with an offset of sRelOpt = {x,y,z}. It is most likely that you place the markers in your Gait Lab different into the segments, therefore you might have to adjust the marker's sRelOpt value. This sRelOpt value is in meter and represents the offset of the marker on the segment. The default used values are very close to most other marker protocols, so there is usually no need to change a lot. There is also a weight function in the code that weighs the optimization of this marker. You may be able to increase these weight values for a certain marker if that marker does not have serious soft tissue artifact(STA). Think before you make changes to the existing model if your changes make really sense! You cannot optimize too many marker directions and segment lengths, otherwise the model will not find a solution and will fail in the study. You can add easily new markers by adding a new marker to this section.

Now close the files and save the changes when being asked.

## Let’s open the TrialSpecific.any file and have a look at it:

In this file you have to change the name of your C3D file:
```
 AnyString NameOfFile ="GaitNormal0001"; //Write the name of the file here
```
Please adjust the gravity. In some Gait Labs, Y is normal to the ground, however, sometimes it is the Z-direction:

```
 AnyVector Gravity = {0, -9.81, 0}; // Y direction is normal to the ground
```

As mentioned above, it will make the optimization easier if you have anthropometric data from your subject. You need to enter here height and weight of the subject. These two values will also be used for strength scaling of the muscles in the model. If you have individual segment lengths, please include them as well. This will help the initial guess.

```
     //These antrhopometric data will be used as initial guess for the optimization
     // alogorithm the optimization algoritm will allow changes to the 
     //segment lengths
     AnyFolder Anthropometrics={
       AnyVar BodyMass=62;  //the mass is automatically distributed to the segments
       AnyVar BodyHeight=1.73;  //height
   
      AnyVar ThighLength= 0.4234534;  //rigth and left side is mirrored 
      AnyVar ShankLength= 0.4120814;
      AnyVar FootLength=0.21;
      AnyVar PelvisWidth=0.16; //distance between hip joints
   
      AnyVar HeadHeight = 0.14;//height in neutral position from  C1HatNode to top of head
      AnyVar TrunkHeight = 0.620233;//height in neautral position from  C1HatNode to L5SacrumJnt
      AnyVar UpperArmLength = 0.340079;
      AnyVar LowerArmLength =0.2690167;
      AnyVar HandLength = 0.182;
      AnyVar HandBreadth = 0.085;
    };
```

In the following sequence, you can see what values are optimized in the parameters optimization study. The segment lengths are optimized when you see an “On” after the segment name. Sometimes, it helps if you exclude certain parameters here. If the Model is setup to optimize too many parameters (and markers / see below), there is a possibility that the model will not find a solution for the optimization process and it will fail. For instance, if you don’t use any markers on the trunk segment, then ‘TrunkHeightOnOff’ value should be set as ‘Off’.

```
      OptimizeAnthropometricsOnOff OptimizeOnOff (
      PelvisWidthOnOff ="On", 
      ThighLengthOnOff="On", 
      ShankLengthOnOff="On", 
      FootLengthOnOff="On", 
      HeadHeightOnOff="Off", 
      TrunkHeightOnOff="On", 
      UpperArmLengthOnOff="Off",
      LowerArmLengthOnOff="Off",
      Model1=MotionAndParameterOptimizationModel, Model2= InverseDynamicModel
      ) ={};
```

The model needs an initial guess of the starting position. You can change starting positions (angles) in the different joints, but you can also change the initial position of the Pelvis wrt global, so you might need to change PelvisRotZ & X from 0 to 90 degrees:

```
 AnyFolder InitialPositionOfBody ={
   
   AnyVar PelvisRotZ = 0;
   AnyVar PelvisRotY = 0;
   AnyVar PelvisRotX = 0;
   
   AnyFolder Right={
     AnyVar HipFlexion = 20.31705;
     AnyVar HipAbduction = 5.707386;
     AnyVar HipExternalRotation = -3.079478;
     AnyVar KneeFlexion = 6.493074;
     AnyVar AnklePlantarFlexion = 41.576023-30;
     AnyVar SubTalarEversion = 0.229261;
   };
   AnyFolder Left ={
     AnyVar HipFlexion = -20.1819;
     AnyVar HipAbduction = -3.050171;
     AnyVar HipExternalRotation = 0*-16.907892;
     AnyVar KneeFlexion = 7.764009;
     AnyVar AnklePlantarFlexion = 48.790382-30;
     AnyVar SubTalarEversion =0.20658;
   };
 };
```

Please close and save the file and let’s go back to the main file. The third file that needs to be adjusted is the environment file:

There are several extra drivers defined for the case that not sufficient kinematic information has been recorded. For example if there are no markers on the upper body, you can switch on trunk drivers to drive the upper body with sepcial drivers and not with motion capture.

# Possible Errors

## Missing markers

Following error may occur during loading:

```
    Constructing model tree...
    ERROR(SCR.PRS9) : C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/Marker.any  :
        Defined At  : C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/Marker.any  :
                      'RTHI'  :  Unresolved object
    Model loading skipped
```

This error message means that you don’t have this particular marker in your C3D file. It might be a typo after typing in a new marker, or you really don’t have that marker and you need to out-comment it in the Marker.any file.

```
    //  CreateMarkerTD RTHI ( 
    //  MarkerName=PrefixDef(RTHI),
    //  MarkerPlacement =Right.Leg.Seg.Thigh, 
    //  OptX="On",OptY="On",OptZ="On",
    //  WeightX=1.0,WeightY=1.0,WeightZ=1.0,
    //  
    //  Model1=MotionAndParameterOptimizationModel, Model2= InverseDynamicModel
    //  )= {
    //    sRelOpt = {-0.03102465, -0.2669603, 0.1139464};
    //  };
```

Please be aware that removing markers means that you should add new markers or you might get a model that has not enough constraints to run.

## Missing calibration matrix definition

Following error may occur during loading:

```
    Constructing model tree...
    ERROR(SCR.PRS9) :  C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/ForcePlate.any  :
                      'Cal'  :  Unresolved object
    Model loading skipped
```

This error message means that you don’t have a calibration matrix in your C3D file and you need to outcomment it in your environment file:

```
    ={
    //  Cal=Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.CAL_MATRIX.Data[0];
    };
```

## Model is kinematically unconstrained

Following warning may occur during loading and then following error message during running the model:

```
    Model Warning: Study 'Main.Study' contains too few kinematic constraints to be kinematically determinate.
```

```
    WARNING(OBJ.MCH.KIN1) : C:/U..s/a..Y/D..p/R..5/A..c/A..n/E..s/S..l/MoCapModel.Main.any  :
                    Study : Model is kinematically indeterminate :
                    Position analysis failed :  440 independent constraints and 441 unknowns
                    - attemps to continue (attempt no. 5)
    ERROR(OBJ.MCH.KIN1) :   C:/U..s/a..Y/D..p/R..5/A..c/A..n/E..s/S..l/MoCapModel.Main.any  :
    Study  :  Model is kinematically indeterminate :  Position analysis failed
```

This means you don’t have enough drivers for your model. It might be that you don’t have markers on you upper body. In that case you need to introduce drivers that fix your upper body model separately. There are more explanations on the Wiki or the Forum at www.anyscript.org.

## Kinematic analysis fails

Even when there is the right number of markers and drivers in the model it can happen that the kinematic analysis fails during the optimization process. If it fails at step 0 then it is usually due a bad guess on the initial position, that means the position of the body at load time is too far from the markers. This is solved by changing the initial position in TrialSpecificData.any.

The kinematic can also fail later during the motion. That can be due to a marker dropping, and thus distorting the motion of a limb. Solutions include lowering the constant weight of the marker, using a custom weight function that will be zero when the marker drops or simply deleting the marker (if there are enough remaining to drive the model). The failing can also come from a bad initial guess of the segment length. For example a too short limb may be stretched by the marker’s motion and may lead to a kinematic error in the joints. To cure this you can make the corresponding segments a bit longer in the TrialSpecificData.any file.

## What is the correct force plate type from the C3D files?

When you load the MoCap model in AMS, you can check the following values:

```
   Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.TYPE.Data
```

When we see this value in the MoCap model, the values are as follows:

```
   Data = {4, 4, 4}
```

So it means that in the C3D file there are three force plates of which type is 4.

## What's the difference between the ForcePlateType4AutoDetection and ForcePlateType4 files?

The ForcePlateType4AutoDetection file is generally used to detect which foot(left or right) is in contact with the specific force plate.

So most of the gait cases, because heel strike and toe off occur between a foot and a force plate, the contact condition is not constant.

For this case, the ForcePlateType4AutoDetection is useful.

And you should use this ForcePlateType4AutoDetection file only for left & right legs unless you can modify this file by yourself.

The usage of ForcePlateType4 file is for when you know which segment of human is in contact with the specific force plate continually.

So there is no concept of 'detection' when you use this file.

One of advantage of this ForcePlateType4 file is that you can use this file when pelvis segment is in contact with a force plate.

## How to run MotionAndParameterOptimizationModel process using MoCap model if there are some missing markers on the upper body?

There are some extra drivers pre-defined in the "ExtraDrivers.any" file. They can be switched on in the trialspecific file.

But of course you should know that because you are missing some kinematic information of upper body segments, the analysis result would not be better than when you have full kinematic information of the whole body.

## How to change my model when the lab coordinate system is different from that of the default MoCap model?

In general, VICON system uses 'Z' axis as its vertical direction of the lab coordinate system.

Let's assume that you are using the VICON motion capture system and the AMTI force platforms(usually type2).

In this case, you have to change several things in the model.

1) First, you have to change the gravity direction of the 'AnyBodyStudy InverseDynamicStudy' class object.

```
   Gravity = {0, 0, -9.81};
```

2) Next, you have to modify 'EnvironmentAutoDetection.any' file as follows:

```
   ForcePlateType2AutoDetection Plate1 (
   PlateName = Plate1,
   Folder =Main.ModelSetup.C3DFileData,
   Limb1=  .BodyModelRef.Right.Leg.Seg.Foot,
   Limb2=  .BodyModelRef.Left.Leg.Seg.Foot,
   No=0,
   VerticalDirection ="Z",
   HeightTolerance=0.07,
   VelThreshold=2.2,
   Fx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fx1,
   Fy=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fy1,
   Fz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Fz1,
   Mx=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mx1,
   My=Main.ModelSetup.C3DFileData.Analog.DataFiltered.My1,
   Mz=Main.ModelSetup.C3DFileData.Analog.DataFiltered.Mz1,
   FootPresent=HumanModelPresent)
   ={ 
   };
```