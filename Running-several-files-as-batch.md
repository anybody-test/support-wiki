If you have several models that you want to run at once, you can use the AnyBody console application to do so. Here is a little example as introduction how to setup batch files for beginners:

Lets say you have a mocap model with a Kinematic sequence (MotionAndParameterOPtimization) and an Inverse sequence. Those should be defined in the main model as:

```
 #ifndef KinematicsModel
         #define KinematicsModel 0
         #endif
         #ifndef InverseDynamicModel
         #define InverseDynamicModel 1
         #endif
```

Let's assume you have also setup several trials

```
#ifndef TrialNumber 
         #define TrialNumber 1
         #endif
```

that will include different mocap files:

```
 #if TrialNumber == 1
     AnyString TrialName       = "Run 1";
   #endif
   #if TrialNumber == 2
     AnyString TrialName       = "Run 2";
   #endif
   ...
```

1. you need to make a macro file "RunAll.anymcr" next to your Main file that includes following code:

```
// Trial 1
        // Kinematic Study
        load "MyModel.main.any" -def KinematicsModel="1" -def InverseDynamicModel="0" -def TrialNumber ="1"
        operation Main.KinematicAnalysisSequence 
        run
        // Inverse Dynamics Study
        load "MyModel.main.any" -def KinematicsModel="0" -def InverseDynamicModel="1" -def TrialNumber ="1"
        operation Main.InverseDynamicAnalysisSequence 
        run
        
        //Trial 2
        // Kinematic Study
        load "MyModel.main.any" -def KinematicsModel="1" -def InverseDynamicModel="0" -def TrialNumber ="2"
        operation Main.KinematicAnalysisSequence 
        run
        // Inverse Dynamics Study
        load "MyModel.main.any" -def KinematicsModel="0" -def InverseDynamicModel="1" -def TrialNumber ="2"
        operation Main.InverseDynamicAnalysisSequence 
        run
        ...
```

2. Now you need to run this macro. Therefore, you need to open a MS DOS command line (click on the windows start button and type cmd to get the MS DOS command (cmd.exe)) and change the directory to your AnyBody installation:

```
cd C:\Program Files (x86)\AnyBody Technology\AnyBody.6.0
```

type AnyBodyCon.exe than /m to specify that it is a macro, then the location of the macro file. This will look something like that:

```
AnyBodyCon.exe /m C:\Users\abc\Desktop\AMMR\Application\Model1\RunAll.anymcr
```

that should get you going.
