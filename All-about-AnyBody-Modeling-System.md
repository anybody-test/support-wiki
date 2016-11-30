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