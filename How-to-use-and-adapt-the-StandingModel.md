+ [Lets open the StandingModel.Main.Any file:](#lets-open-the-standingmodelmainany-file)
    - [libdef](#libdef)
    - [RunApplication](#runapplication)
    - [HumanModel](#humanmodel)
    - [Scaling](#scaling)
    - [Lets have a look at the environment.any file:](#lets-have-a-look-at-the-environmentany-file)
    - [Lets have a look at the mannequin.any file:](#lets-have-a-look-at-the-mannequinany-file)
    - [Lets have a look at the study:](#lets-have-a-look-at-the-study)
This document will explain the Standing Model. In particular, the structure, the different sequences and some key commands will be shown and discussed. Many explanations are compatible with other models. The AnyBody Modeling System v5.2 (AMS) and the AnyBody Managed Model Repository v1.5 (AMMR) are used in this example.


# Let’s open the StandingModel.Main.Any file:

In the folder AMMR-v1.5\Application\Examples\StandingModel you see many different AnyScript files. The main file that is necessary to start and run a model is always called xxx.Main.any. In the case of the StandingModel it is called StandingModel.Main.Any.

## libdef

After opening the file, you will immediately see the include function of the libdef.any file. This command can be found in all models and it provides the path link to the Body folder. In the AMMR all applications are separated from the basic body. The libdef will link the applications to the Body folder.

## RunApplication

Next, you will find an OperationSequence. IN the StandingModel you have a calibration sequence and then the inverse dynamics (where the muscle forces will be calculated) included.

```
 AnyOperationSequence RunApplication = {
   /**This operation calibrates the muscles in the model if these are of the type AnyMuscleModel3E.
   This will just be an empty operation if the model is using a muscle type that does not require calibration.*/
   AnyOperation &CalibrationAnal = Main.HumanModel.Calibration.CalibrationSequence;  
   ///This operation is the inverse dynamic analysis
   AnyOperation &InvAnal=Main.Study.InverseDynamics;
 };
```

## HumanModel

In the next part of the model, the human model will be constructed. First the BodyPartsSetup is opened. In this file you can choose what body parts you want to use. If you switch a “0” into a “1” this segment will be used in the model. Of course, you cannot use two right legs, and define muscles twice in a segment. General differences, there are some combinations not allowed, but you will get a warning when loading the model. Some comments: -	RightLegTD is the newer version compared to the RightLeg. -	3E muscles are Hill Type muscles compared to simple muscles (in many applications it is ok to use simple muscles)

## Scaling

By default a standard scaling is used in the StandingModel:

```
   // ScalingStandard means that all the body parts will have the standard size 
   // at which they were originally developed, i.e. with anthropometric
   // data from the anatomical literature. This roughly corresponds to a 50th
   // percentile European male.    
   #include "<ANYBODY_PATH_BODY>\Scaling\ScalingStandard.any"
   
   // ScalingLengthMassFat will scale each segment of the body to anthropometric data
   // specified in the selected AnyFamily include file, attempting to take the
   // fat percentage into account in the assessment of the muscle strength.
   // #include "<ANYBODY_PATH_BODY>\Scaling\ScalingLengthMassFat.any" 
   // Scaling = {
   //   #include "<ANYBODY_PATH_BODY>\Scaling\AnyFamily\AnyMan.any"
   // };
```

By out-commenting the first include function and commenting the next two includes you will use the LengthMassFat scaling:

```
   // ScalingStandard means that all the body parts will have the standard size 
   // at which they were originally developed, i.e. with anthropometric
   // data from the anatomical literature. This roughly corresponds to a 50th
   // percentile European male.    
   // #include "<ANYBODY_PATH_BODY>\Scaling\ScalingStandard.any"
   
   // ScalingLengthMassFat will scale each segment of the body to anthropometric data
   // specified in the selected AnyFamily include file, attempting to take the
   // fat percentage into account in the assessment of the muscle strength.
    #include "<ANYBODY_PATH_BODY>\Scaling\ScalingLengthMassFat.any" 
    Scaling = {
      #include "<ANYBODY_PATH_BODY>\Scaling\AnyFamily\AnyMan.any"
    };
```
For more details on scaling please have a look at the tutorials and the short abstract here in Appendix A. If you proceed further in the model, you will find two files included: 

+ Environment.any 
+ Mannequin.any

## Let’s have a look at the environment.any file:

In the StandingModel the environment file is almost empty as the human body model is only standing upright by default without anything attached to the body model. If you look into another model, let’s say the StandingLift model, you will see many additional objects in the environment file. In this StandingLift model the Body model is carrying a box consisting of several parts. The environment file is the place where you can add any kind and number of objects to the model. However, just adding an object here will not be enough. You cannot have objects floating in space in AnyBody. You need to attach it to something. So you can either fix it to global or to body segments using drivers.

## Let’s have a look at the mannequin.any file:

In many models the mannequin file is only used to give the body model an initial guess on the start position. The final motion will be applied in the drivers. In the StandingModel the mannequin values are used for both, for the initial position and for driving the body model. The values are transferred to individual driver files and are direct input for driving it. If you open the file and scroll down a bit you will find following folders:

```
   AnyFolder Mannequin = {
 
   AnyFolder Posture = {
```

In the posture folder you can edit the start angles of the body joints. For example flexion and pronation for the elbow joint:

```
     AnyVar ElbowFlexion = 0.01; 
     AnyVar ElbowPronation = -20.0;
```

or the Flexion, Abduction and Rotation for the hip joint and flexion for the knee joint:

```
     //Leg
     AnyVar HipFlexion = 0.0; 
     AnyVar HipAbduction = 5.0; 
     AnyVar HipExternalRotation = 0.0;
     
     AnyVar KneeFlexion = 0.0; 
```

The body model is by default setup to have a symmetric posture, so values for the right joints are transferred to the left joints as well:

```
     AnyVar HipFlexion =.Right.HipFlexion;  
     AnyVar HipAbduction =.Right.HipAbduction;
     AnyVar HipExternalRotation = .Right.HipExternalRotation;
     AnyVar KneeFlexion = .Right.KneeFlexion;   
```

You can easily change that by adding separate numbers in

```
     AnyVar HipFlexion = 10;//.Right.HipFlexion;  
```

Once you have the starting poture well defined, you need to define the motion by typing in joint velocities into the next folder:

```
    AnyFolder PostureVel={  
```

By default all starting values are 0, but they can be changed. Please be aware of step number and simulation time of the study (see next chapter about that):

```
     //Leg
     AnyVar HipFlexion = 0.0; 
     AnyVar HipAbduction = 0.0; 
     AnyVar HipExternalRotation = 0.0;
```

## Let’s have a look at the study:

The study section of the StandingModel is rather short. tEnd describes the time the model has to perform the defined motion. nStep shows the number of steps to do the motion.

```
 AnyBodyStudy Study = {
   AnyFolder &Model = .Model;
   
   tEnd = 1.0;
   Gravity = {0.0, -9.81, 0.0};
   nStep = 5;
   
 }; // End of study
```