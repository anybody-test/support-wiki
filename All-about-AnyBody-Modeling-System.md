+ [Does the AnyBody Modeling System allow plug-ins? For example, can the user specify his own muscle model or apply a new analysis?](#does-the-anybody-modeling-system-allow-plug-ins-for-example-can-the-user-specify-his-own-muscle-model-or-apply-a-new-analysis)
+ [Why did you choose to develop a text-based interface for constructing models rather than a graphical interface?](#why-did-you-choose-to-develop-a-text-based-interface-for-constructing-models-rather-than-a-graphical-interface)
+ [Can I export computation data to my spreadsheet?](#can-i-export-computation-data-to-my-spreadsheet)
+ [Save Load and Replay Results](#save-load-and-replay-results)
+ [Create a ButterworthFilter](#create-a-butterworthfilter)
+ [Use an input file](#use-an-input-file)
+ [Changing raw data inside AnyBody](#changing-raw-data-inside-anybody)
+ [Create an output file](#create-an-output-file)
+ [Understand the ExportEquilibrium file](#understand-the-exportequilibrium-file)
+ [Running AnyBody in batch mode](#running-anybody-in-batch-mode)
+ [Create and output function](#create-and-output-function)
+ [Is it possible to write Mathematical expressions in AnyScipt?](#is-it-possible-to-write-mathematical-expressions-in-anyscipt)
+ [CAD Files](#cad-files)
    - [What type of data is required to import a particular bone morphology?](#what-type-of-data-is-required-to-import-a-particular-bone-morphology)
    - [What is the source of your bone geometry data?](#what-is-the-source-of-your-bone-geometry-data)
+ [Graphics](#graphics)
    - [Make trajectories visible](#make-trajectories-visible)
    - [Display the center of mass of the entire model](#display-the-center-of-mass-of-the-entire-model)
    - [Create a camera](#create-a-camera)
    - [How to move the location of a CAD file](#how-to-move-the-location-of-a-cad-file)
+ [General](#general)
    - [Coordinate transformations local to global and visa versa](#coordinate-transformations-local-to-global-and-visa-versa)




## Does the AnyBody Modeling System allow plug-ins? For example, can the user specify his own muscle model or apply a new analysis?

Both yes and no! We do not have an interface (exit) that allows you to program your own bit of the analysis in like C or FORTRAN and link that to AnyBody. We would like to do so of course, but it is not on the here-and-now-to-do list. We have a few close collaborators where we made special arrangements though, but we have not yet done so in a more formal way. We do however plan to open as much as possible for customization of objects from the interface and muscle models is indeed one issue where we will. Another issue is the definition of the objective function for muscle recruitment. We are currently working on a new feature to the system that will allow the user to define his/her own muscle model. If the user can formulate the muscle strength as an analytical function of length and contraction velocity, then he can code it into his own muscle model and use it in the analysis. We are always very open for suggestions on model features, we ought to include. Any feedback on desired customization options and maybe even plug-ins are very welcome.


## Why did you choose to develop a text-based interface for constructing models rather than a graphical interface?

A model needs an underlying data structure. In systems with graphical interfaces such as CAD systems, the norm is that this data structure is hidden from the user, and he or she only sees the model through the graphical interface. This can work very well for many problems. However, a body model can be a very complicated set of data, and users will quickly find themselves in a situation where they desire a closer relationship with the data they are working on. So we have decided to let the user work directly on the data of the system. To help in this job, we will continue to develop better editing and browsing facilities in the system. This is much the same philosophy as well-known and popular systems such as Matlab and Tex.


## Can I export computation data to my spreadsheet?

Yes. In the chart view, when you click the "Copy to clipboard" icon, you are presented with a menu that allows you to choose to export data as text. If you do so, simply paste the data into your spreadsheet, and you get the curves in the picture represented as columns of numbers in the spreadsheet.


## Save Load and Replay Results

The save data function can be used to save results for later review. AMS let you save the output from a study into to a single file using the .h5 format. This is a convenient way to save all the output form the model. It possible later re-open the model, load the data and replay the model, or create additional graphs from the saved data.
To save the output, please follow these steps:

+ In the ModelTree locate the AnyBodyStudy from which you want to save the results from.
+ Then right click the folder “Output”
+ Select the option “Save data”
+ Select if the data should be saved as a flat or deep structure. This choice only has implications on how the output structure will look like and it does not impact how the data later can be loaded into AMS. Choosing the “deep” option will save the output as a structure resembling the output folder structure seen in the ModelTree.
+ Then select a name for the file

To load the output, please follow these steps:

+ In the ModelTree locate the AnyBodyStudy to which you want to load the results into.
+ Then right click the folder “Output”
+ Select the option “Load data”
+ Then select the file with the file browser

When the output file has been loaded it is possible to “Replay” the loaded data, please follow these steps:

+ Open the OperationTree
+ Locate the AnyBodyStudy you have loaded the data into
+ Select the Replay operation
+ Run it

It is also possible to create the actions listed above through macro’s. The reference manual has a small example displaying this; please search for “AnyDataMacroDemo”, in the reference manual and you will find a short description and a sample model. The sample files displays how to create an operation which creates the output and loads it again.

Extra info: The macro’s displayed in the reference manual example can also be made using macro strings, in this way the macros do not need to be defined through .anymcr files, but can be directly written in the script. The lines below will perform in the same way as the SaveDataDeepMarco operation defined in the reference manual example.

```
AnyOperationMacro SaveDataDeepMacroScriptVersion= { 
 MacroStr={
 "classoperation Main.ArmModelStudy.Output" + " " + strquote("Save     data") + " --file=" +"arm2d_flat.anydata.h5" 
    +  " --type=Deep"};
 };
```

The AnyOperationMacro’s naturally also works in the AnyBody console application AnyBodyCon.


## Create a ButterworthFilter

AnyFunButterworthFilter is a Butterworth filter extension to the general linear filter AnyFunLinFilterBase with a simpler interface. This makes it possible to import raw experimental data without prior filtering by external software.

The model demonstrates the use of the Butterworth filter facility. The model filters a dataset, composed of a series of sinus waves with a lowpass, highpass, bandpass and bandstop filter and uses the filtered data to create interpolation functions as function of time that can be plotted after a kinematic analysis.

The interpolation functions used throughout the file do not serve any other purpose than allowing one to plot the data using the charts. The filters allow you to filter any data given as an AnyFloat and it returns to you an AnyFloat with the filtered data.
![image](https://cloud.githubusercontent.com/assets/22542671/20749805/24ef16f6-b6f4-11e6-809c-e787b9770d59.png)

This sample code is shows how to make use of the filter

[AnyButterWorthFilter.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/AnyButterWorthFilter.any)


## Use an input file

The AnyInputFile can be used for reading in input files from text files and place the data in a matrix or vector. This matrix can then be used for interpolationdrivers ect. Try to load the model and once it has loaded please try to look in the model tree and explore the Data folder which now contains the data loaded from the file.

[InputFiles.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/InputFiles.any)

[InputTest.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/InputTest.any)


## Changing raw data inside AnyBody

Sometimes you have raw data which is not directly suitable for driving the model, it can be that you would like to change the units the sign etc. Instead of doing changes to the raw data it is possible to do these changes inside AnyBody. This is done by reading the data into the system using he AnyInputFile and the use this data in the interpolation function. When referring to the data it is possible to make changes to the raw data. The sample file illustrates how this is possible; it contains one segment and one prismatic driver.

[AdjustingData.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/AdjustingData.any)

[Data.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/Data.any)

## Create an output file

The AnyOutputFile object can be used for creating output files, which can be entirely customized. The example file illustrates this on a very small model.

[OutputFiles.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/OutputFiles.any)


## Understand the ExportEquilibrium file

This is some extra comments in addition to what can be found in the manual on the EquilEqExportFileName which can be created from the AnyBodyStudy.

This is a facility which can be used for exporting the entire equilibrium system to a text file.

The rows are each corresponding to one degree of freedom. In principle, you do not know any more about the rows. It is not like you might imagine that each row was liked to a certain kinematic property in the model (e.g. a kinematic measure), at least it does not have to be like this. A row can, in general, be associated with any combination of kinematics.

However, in the current format, each row is actually associated with one degree of freedom of a segment; so the first six rows are first 3 translational then 3 rotational degrees of freedom of the first segment and so on.

It is stated in this way because there is no guarantee that it will stay like this for all model setups. The data are output directly for the computational kernel of AnyBody, so it will reflect whatever algorithm that is forming the equations of motion.

The columns of Cm each correspond to one muscle and they are ordered like the list of "Strong Muscles" in the file.
Similarly, each column of Cr corresponds to a component of a reaction force with Type equal to On.


## Running AnyBody in batch mode

The AnyBody Modeling System comes with a command line version included. It is named AnyBodyCon.exe ("Con" for console application), and you will find it in the directory where you install AnyBody. The command line version is - as the name indicates - a version of the AnyBody Modeling System with the user interface stripped off. This means that it is executed from a DOS command prompt or - more importantly - by a batch file or another software application.


## Create and output function

One possible method for easily viewing data (in Chart FX) imported from an AnyFunInterpol {} function is to use this attached bit of code. It resamples the data to be consistent with tStart and tEnd used in your study. One note is that the abscissa in the Chart 2d is by default the global time. If you want to see the data plotted against its real T values, you need to change the Chart 2d abscissa to use the 'adjustedTime_Data' in the outputFunData2. The curves will be the same, just the axis labels will be different. [InterpOutputFun.Main.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/InterpOutputFun.Main.any) uses the data file: [P1.txt.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/P1.txt.any) (which you need to rename to P1.txt)


## Is it possible to write Mathematical expressions in AnyScipt?

Sure. Please refer to the section entitled "Variables and expressions" in the reference manual. Most algebraic functions you can think of are available.


# CAD files

## What type of data is required to import a particular bone morphology?

The input for AnyBody is simple STL files. They are converted by AnyBody to ease subsequent handling of the data, so you are not able to find STL files in the repository, but that is all you need to input your own geometry data. The STL files can also represent the environment such as wheelchairs, floors, tools, and other model elements.

## What is the source of your bone geometry data?

They are from 3D scans from plastic skeletons. They are actually not in a very good quality and we would like to get access to better data, but of course they need to be without all sorts of restrictions about their use; this is both due to our own usage and due our desire to place them in the in the model repository.

# Graphics

## Make trajectories visible

When developing models it is often convenient to be able to actually see the trajectory of a certain point (node) in the model such as a marker from a motion capture experiment or perhaps a joint center. The small example shows how this can be done by making use of the AnyChar object.

![image](https://cloud.githubusercontent.com/assets/22542671/20751818/4d451d62-b6fe-11e6-9f94-96bc7e8ffeb5.png)

[Trajectory2.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/Trajectory2.any)

## Display the center of mass of the entire model

This small model demonstrates how it is possible to create a visual object representing the collective center of mass for a number of segments. This is done using the object AnyKinCoM measure that can measure the center of mass for one or more objects. The output from this object is the center of mass measure in the global reference system. In order to visualize the object, a new dummy segment was created and using the AnyKinLinComb measure it has been driven to the location of the CoM, for the simple two bar mechanism in the model.

![image](https://cloud.githubusercontent.com/assets/22542671/20752045/73ec4e26-b6ff-11e6-9fd7-4f14603b185e.png)

The picture displays the center of mass as a blue sphere.

[CoMDisplay.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/CoMDisplay.any)


## Create a camera

This sample file cotains the definition of a moving camera using the AnyCamera class. Unlike the classic recorder of the model view, AnyCamera can fly around your model in complex paths. The results are impressive videos appreciated for presentations, exhibitions, etc.

AnyCamera object showing itself in the Model View

To use the camera you just have to include this file in the study folder of the Main file and run an analysis. Don’t forget to create a folder “MovingCamera” to store the images, otherwise it will not work. Several strategies for driving the camera are given as examples in the file, but of course you are free to create your own smart trajectories and do the best video ever...

[MovingCamera.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/MovingCamera.any)


## How to move the location of a CAD file

Usually CAD files with the same coordinate system as the segment they are attached to are used in the models. This makes it possible to attached directly the CAD file to the segment without more manipulation.

The best way to make the CAD file fit the model is to translate and rotate it using a CAD program such as Rhinoceros or similar. First you have to define a reference point common on both the STL file and the segment; it can be the origin of the segment or some bony landmark any point that you can locate with precision on both the STL file and the segment. This point will be the origin of the STL file. Open the file with the CAD software (rhinoceros for example) and move it to have the origin at the chosen point and the orientation of the frame matching with the one of the segment. Then you can create a node (if not existing already) at the location of the reference point on the segment and insert the CAD file in this node, and the only thing left is to scale properly the CAD file.

# General

## Coordinate transformations local to global and visa versa

How do you transform coordinates from local coordinate system to global and vise versa? This small example shows how this is done.

[CoordinateTransformations](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/AnyBody_Modelling_System/CoordinateTransformations.any)