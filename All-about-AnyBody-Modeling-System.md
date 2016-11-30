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