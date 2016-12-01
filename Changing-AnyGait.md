#Currently AnyGait is suitable for 3 different Marker Setups

## AnyGaitCommonSetup.any

In the folder AnyGaitSourceCode you will find a file "AnyGaitCommonSetup.any". In this file you can switch between the 3 Setups:

```
 #define ANYGAIT_MARKERPROTOCOL "QUAL"
 // #define ANYGAIT_MARKERPROTOCOL "VIC"
 // #define ANYGAIT_MARKERPROTOCOL "SIMI"
```

This will lead to change the marker names. Following markers are required:

In the hip posterior and anterior markers are needed for left and right. On the left and right leg: a proximal femur marker, a lateral knee marker, a medial knee marker (only for static), a tibial marker, a lateral ankle marker, a medial ankle marker (only for static), a heel and a toe marker.

![image](https://cloud.githubusercontent.com/assets/22542671/20789424/a61696da-b7b4-11e6-9971-3cd289fa4bb6.png)

*With courtesy of the University Medical Center Regensburg*

## Marker.any

In the folders AnyGaitSourceCode\Model\OptLegModel\KinOptModel you can find the marker.any file with the current marker names. If you have other names, you need to change them.

"VIC" Static trial:

```
 RTHI,  RKNE, RFEO, RTIB,  RANK, RTIO, RHEE, RTOE,
 LTHI,  LKNE, LFEO, LTIB,  LANK, LTIO, LHEE,  LTOE,
 RASI,  LASI, RPSI, LPSI,
```

"VIC" dynamic trial:

```
 RTHI,  RKNE, RTHL, RTIB, RANK, RMMA,  RHEE, RTOE,
 LTHI,  LKNE, LTHL, LTIB, LANK, LMMA,  LHEE, LTOE, 
 RASI,  LASI, RPSI, LPSI,
```

"SIMI"

```
 _31002, _31100, _31101, _31150, _31200, _31201, _31305, _31306,
 _21002, _21100, _21101, _21150, _21200, _21201, _21305, _21306,
 _31001, _21001, _31003, _21003
```

"QUAL"

```
 RThighSuperior, RKneeLateral, RKneeMedial, RShankSuperior, RAnkleLateral, RAnkleMedial, RHeel, RToe
 LThighSuperior, LKneeLateral, LKneeMedial, LShankSuperior, LAnkleLateral, LAnkleMedial, LHeel, LToe  
 RAsis, LAsis, RPsis, LPsis
```

# How to change Force Plate Type

## InverseDynamics.any

Let’s open the InverseDynamics.any file and have a look at it:

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

## Environment.any
The force plates are used in the Environment.any file. Here are some examples how that might look like:
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