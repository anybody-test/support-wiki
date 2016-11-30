The AnyBody Modeling System v5.2 (AMS) and the AnyBody Managed Model Repository v1.5 (AMMR) are used in this example:

# What do you need to process your own C3D model?

1. C3D file of the recorded trial: Many of our users have the Plug-In-Gait marker protocol. Different protocols are also applicable. Additional markers can easily be added to the model. Force data from Force Platforms is necessary to ensure a smooth processing of the trials.
2. Anthropometric data of the recorded subject. Weight and total height of the subject are necessary. Individual segment lengths, e.g. thigh, shank, … is helping the process, but not mandatory.

## What is the structure of the GaitLowerExtremity example?

Let’s get started, and open the model with the Main file GaitLowerExtremity.Main.any. The model is divided into two parts, the MotionAndParameterOptimization Study and the InverseDynamics Study. You can switch between both studies by changing 0 (=Off) to 1 (=On). If you want to analyze a new motion capture trial and have a new C3D file, you need to start with the MotionAndParameterOptimization first!
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

```
The following two files are mostly responsible to adapt the model to your new C3D file:
      //This file controls most of the model setup, the following items is defined here:
      //1. C3D file definition
      //2. Optimization settings for which anthropometrics to optimize
      //3. Defition of markers
      //If you want to run the model on your own data this is the place to start, additionally it may 
      //need to make changes in the Mannequin file to position the human model close to the recorded
      //markers.
       #include "ModelSetup.any" 
       #include "TrialSpecificData.any" 
```

Let’s open the ModelSetup.any file and have a look at it:

```
     //This is the input file to the analysis
      AnyInputC3D C3DFileData =   {
        AnyString PrefixStr=""; //if the dataset makes use of a prefix name which goes in front of all
                                  markers and processed datanames write it here
     ...
        AnyString NameOfFile = Main.TrialSpecificData.NameOfFile; //Write the name of the file here
     ...
        MarkerLabels =      {"RFHD","LFHD","RBHD","LBHD","CLAV","C7","T10","STRN","RBAK","RSHO",
                             "LSHO","RUPA","LUPA","RELB","LELB","RFRA","LFRA","RWRA","LWRA",
                             "RWRB","LWRB","RFIN","LFIN","RASI",“LASI","RPSI","LPSI",
                             "RTHI","LTHI","RKNE","LKNE","RTIB","LTIB","RANK","LANK",
                             "RHEE","LHEE","RTOE","LTOE","RMT5","LMT5"};
    ... };
```

I deleted some lines to have only the most important commands here. The AnyInputC3D will import the C3D file into the model. In some rare cases it is possible that the markers have a prefix prior to the name that can be included in here. It will use a C3D file with a name that is specified at the path:

```
    Main.TrialSpecificData.NameOfFile
```

You will be able to change that later in the Trialspecific.any file, so you don’t have to do that here. At MarkerLabels you have to specify the names of your markers. The marker names in this model are the typical Vicon marker names. Typical Simi or Qualisys marker names:

```
       MarkerLabels = {"_31100","_21100","_31001","_21150","_31200","_31150","_21200","_31305",
                       "_21305","_21001","_31320","_31326","_21320","_21326","_31003","_21003",
                       "_31201","_21201","_10404","_31306","_21306","_31002","_21002","_31101",
                       "_21101","_44001","_44002","_44003","_44004","_44005","_44006","_44007","_44008"};
```

```
       MarkerLabels = {"RThighSuperior","RKneeLateral","RKneeMedial","RShankSuperior","RAnkleLateral","RAnkleMedial",
                       "RHeel","RToe","LThighSuperior","LKneeLateral","LKneeMedial","LShankSuperior",
                       "LAnkleLateral","LAnkleMedial","LHeel","LToe","RAsis","LAsis","RPsis","LPsis"};
```

I recommend that you look into the C3D file. There are many freeware tools; I use for example the MLSViewer. There you will find the structure of the C3D file, the names of the markers and more necessary data on the force plates. So please change the names of the list in the file to the names of your markers if they are different. Then let’s look further down:

```
      // FrontFrameOffset is the offset value from the first frame of C3D file
      AnyIntVar FrontFrameOffset = 0;
      // LastFrameOffset is the offset value from the last frame of C3D file
      AnyIntVar LastFrameOffset = 0;
      AnyIntVar FirstFrame = C3DFileData.Header.FirstFrameNo + FrontFrameOffset; 
      AnyIntVar LastFrame = C3DFileData.Header.LastFrameNo - LastFrameOffset;
      AnyIntVar nStep =(LastFrame-FirstFrame+1);
      AnyFloatVar tStart = FirstFrame/C3DFileData.Header.VideoFrameRate;
      AnyFloatVar tEnd = LastFrame/C3DFileData.Header.VideoFrameRate;
```

In the C3D file you can see if markers are dropping out, when they go to 0 suddenly. You can also limit the time to analyze the trial by identifying a start and end frame, otherwise the full C3D will be used and processed. In the following sequence, you can see what values are optimized in the parameters optimization study. The segment lengths are optimized when you see an “On” after the segment name. Sometimes, it helps if you exclude certain parameters here. If the Model is setup to optimize too many parameters (and markers / see below), there is a possibility that the model will not find a solution for the optimization process and it will fail. For instance, if you don’t use any markers on the trunk segment, then ‘TrunkHeightOnOff’ value should be set as ‘Off’.

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

In the final part of the ModelSetup.any file all the markers are created using class templates. If the LegTD model is used, the CreateMarkerTD class is used. The upper body markers will be generated using the CreateMarker class. Both classes are included in the upper part of the main file(‘CreateMarkerClass.any’ and ‘CreateMarkerClassTD.any’). Usually you don’t need to modify these files. You can see in the code below that the Marker with a given name is here assigned to a body segment, e.g. RTHI is assigned to the right thigh segment. The RTHI marker is optimized in X, Y and Z direction (Opt_=”On”). The target marker is placed on the right thigh with an offset of sRelOpt = {x,y,z}. It is most likely that you place the markers in your Gait Lab different into the segments, therefore you might have to adjust the marker's sRelOpt value. This sRelOpt value is in meter and represents the offset of the marker on the segment. The default used values are very close to most other marker protocols, so there is usually no need to change a lot. There is also a weight function in the code that weighs the optimization of this marker. You may be able to increase these weight values for a certain marker if that marker does not have serious soft tissue artifact(STA). Think before you make changes to the existing model if your changes make really sense! You cannot optimize too many marker directions and segment lengths, otherwise the model will not find a solution and will fail in the study. You can add easily new markers by adding a new marker to this section.

```
      CreateMarkerTD RTHI ( 
      MarkerName=PrefixDef(RTHI),
      MarkerPlacement =Right.Leg.Seg.Thigh, 
      OptX="On",OptY="On",OptZ="On",
      WeightX=1.0,WeightY=1.0,WeightZ=1.0,
      Model1=MotionAndParameterOptimizationModel, Model2= InverseDynamicModel
      )= {
        sRelOpt = {-0.03102465, -0.2669603, 0.1139464};
      };
 
      CreateMarkerTD RKNE (
      MarkerName=PrefixDef(RKNE),
      MarkerPlacement= Right.Leg.Seg.Thigh,
      OptX="Off",OptY="Off",OptZ="On",
      WeightX=1.0,WeightY=1.0,WeightZ=1.0,
      Model1=MotionAndParameterOptimizationModel, Model2= InverseDynamicModel 
      ) = {
        sRelOpt= {0.031, -0.3992649, 0.05};
      };
```

Now close the ModelSetup.any file and save the changes when being asked.

## Let’s open the TrialSpecific.any file and have a look at it:

In this file you have to change the name of your C3D file:

```
 AnyString NameOfFile ="GaitNormal0001-processed"; //Write the name of the file here
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

## Let’s open the EnvironmentAutoDetection.any file and have a look at it:

The standard setup is using force plates of the type 4. Please look at C3D.org to read more about force plate types. You can create your own force plates type, but the most common plates are already setup in AnyBody. If you look into your C3D under Forceplate and Type you will see how many and what type you have. If you have a different type than type 4 you have to change a couple things. First you need to include the class templates in the main file. By default you have:

```
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType4AutoDetection.any"
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType4.any"
```

You can have several class templates included, so just add the type 2 and 3 force plates class templates with following code:

```
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType2AutoDetection.any"
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType2.any"
    
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType3AutoDetection.any"
    #include "../../../Body/AAUHuman/ToolBox/Mocap/ForcePlateType3.any"
```

As you can see there are two different options, you can use the AutoDetection or the basic version. AutoDetection is generally used to detect which foot (left or right) is in contact with the specific force plate automatically. So you should try to use the AutoDetection. Otherwise, if you use the basic version you need to modify this file yourself. Therefore you need to know which segment of the human body is in contact with the specific force plate. So there is no concept of 'automatic detection' when you use this file. One of advantage of this ForcePlateTypeX.any file is that you can use this file when any kind of human segment is in contact with a force plate such as pelvis segment.

Another important aspect is if the force plates are using a calibration matrix or not. You can see that also in the C3D file under Force_Platform (or similar). According to the following points you have to change the EnvironmentAutoDetection.any or Environment.any file:

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

# Possible Errors

## Missing markers

Following error may occur during loading:

```
    Constructing model tree...
    ERROR(SCR.PRS9) : C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/ModelSetup.any  :
        Defined At  : C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/ModelSetup.any  :
                      'RTHI'  :  Unresolved object
    Model loading skipped
```

This error message means that you don’t have this particular marker in your C3D file. It might be a typo after typing in a new marker, or you really don’t have that marker and you need to outcomment it in the ModelSetup.any file.

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
    ERROR(SCR.PRS9) :  C:/U..s/a..Y/D..p/P..a/FDA/F..2/A..k/A..n/F..l/G..y/M..l/EnvironmentAutoDetectionType2.any  :
                      'Cal'  :  Unresolved object
    Model loading skipped
```

This error message means that you don’t have a calibration matrix in your C3D file and you need to outcomment it in your environment AutoDetection.any file:

```
    ={
    //  Cal=Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.CAL_MATRIX.Data[0];
    };
```

##Model is kinematically unconstrained

Following warning may occur during loading and then following error message during running the model:

```  
  Model Warning: Study 'Main.Study' contains too few kinematic constraints to be kinematically determinate.
```

```
    WARNING(OBJ.MCH.KIN1) : C:/U..s/a..Y/D..p/R..5/A..c/A..n/E..s/S..l/StandingModel.Main.any  :
                    Study : Model is kinematically indeterminate :
                    Position analysis failed :  440 independent constraints and 441 unknowns
                    - attemps to continue (attempt no. 5)
    ERROR(OBJ.MCH.KIN1) :   C:/U..s/a..Y/D..p/R..5/A..c/A..n/E..s/S..l/StandingModel.Main.any  :
    Study  :  Model is kinematically indeterminate :  Position analysis failed
```

This means you don’t have enough drivers for your model. It might be that you don’t have markers on you upper body. In that case you need to introduce drivers that fix your upper body model separately. There are more explanations on the Wiki or the Forum at www.anyscript.org.

## Kinematic analysis fails

Even when there is the right number of markers and drivers in the model it can happen that the kinematic analysis fails during the optimization process. If it fails at step 0 then it is usually due a bad guess on the initial position, that means the position of the body at load time is too far from the markers. This is solved by changing the initial position in TrialSpecificData.any.

The kinematic can also fail later during the motion. That can be due to a marker dropping, and thus distorting the motion of a limb. Solutions include lowering the constant weight of the marker, using a custom weight function that will be zero when the marker drops or simply deleting the marker (if there are enough remaining to drive the model). The failing can also come from a bad initial guess of the segment length. For example a too short limb may be stretched by the marker’s motion and may lead to a kinematic error in the joints. To cure this you can make the corresponding segments a bit longer in the TrialSpecificData.any file.

## What is the correct force plate type from the C3D files?

When you load the GaitFullBody model in AMS, you can check the following values:

```
   Main.ModelSetup.C3DFileData.Groups.FORCE_PLATFORM.TYPE.Data
```

When we see this value in the GaitFullBody model, the values are as follows:

```
   Data = {4, 4, 4}
```

So it means that in the C3D file there are three force plates of which type is 4.

If your force plate type is 2 or 3, then you have to use another class template instead of ForcePlateType4AutoDetection.any and ForcePlateType4.any files.

## What's the difference between the ForcePlateType4AutoDetection.any and ForcePlateType4.any files?

The ForcePlateType4AutoDetection.any file is generally used to detect which foot(left or right) is in contact with the specific force plate.

So most of the gait cases, because heel strike and toe off occur between a foot and a force plate, the contact condition is not constant.

For this case, the ForcePlateType4AutoDetection.any is useful.
And you should use this ForcePlateType4AutoDetection.any file only for left & right legs unless you can modify this file by yourself.

The usage of ForcePlateType4.any file is for when you know which segment of human is in contact with the specific force plate continually.

So there is no concept of 'detection' when you use this file.

One of advantage of this ForcePlateType4.any file is that you can use this file when pelvis segment is in contact with a force plate.

## How to run MotionAndParameterOptimizationModel process using GaitLowerExtremity model if there are some missing markers on the upper body?

In this case, you should add more drivers in the "ExtraDrivers.any" file.

If you didn't use markers on the head, please add the following constraints:

```
AnyKinEqSimpleDriver NeckDrv = {
  AnyKinMeasureOrg &ref0 = ...BodyModel.Interface.Trunk.NeckJoint; 
  DriverPos= pi/180*{.JntPos.NeckExtension};
  DriverVel= pi/180*{.JntVel.NeckExtension};
  Reaction.Type={Off};
};
```

If you didn't use markers on the trunk, please add the following constraints:

```
AnyKinEqSimpleDriver PelvisThoraxDrv = {
    AnyKinMeasure& ref0 = ...BodyModel.Interface.Trunk.PelvisThoraxExtension;
    AnyKinMeasure& ref1 = ...BodyModel.Interface.Trunk.PelvisThoraxLateralBending;
    AnyKinMeasure& ref2 = ...BodyModel.Interface.Trunk.PelvisThoraxRotation;
    DriverPos = pi/180*
    {
       .JntPos.PelvisThoraxExtension,
       .JntPos.PelvisThoraxLateralBending,
       .JntPos.PelvisThoraxRotation
    };
    DriverVel =  pi/180*
    {
       .JntVel.PelvisThoraxExtension,
       .JntVel.PelvisThoraxLateralBending,
       .JntVel.PelvisThoraxRotation
    };
    Reaction.Type = {Off, Off, Off};
};
```

But of course you should know that because you are missing some kinematic information of upper body segments,
the analysis result would not be better than when you have full kinematic information of the whole body.

## How to change my model when the lab coordinate system is different from that of the default GaitFullBody model?

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
