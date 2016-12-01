## Which Python version should one use?

It is recommended to use [PythonXY](http://python-xy.github.io/) for 32bit or AnacondaPython for 64bit AnyBody in order to avoid problems with configurations.

## Python/ C++ link in general

The AnyFunEX function class serves as a wrapper for externally implemented functions. It thereby provides a hook into the AnyScript expression engine for external code. Currently, C++ and Python code interfaces exist.

C++ code must be compiled into a DLL, whereas Python code can be supplied directly as script. Therefore, Python scripting is the choice for situations were the code needs to be modified frequently, whereas a C++ implemented DLL provide a more computational efficient interface.

AnyFunctions are polymorphe, i.e., they can hold multiple instances of the same function (multiple functions with the same function name). The difference between function instances lies in the lists of arguments. Each instance is a special AnyScript object, in AnyScript called a mono-function. The mono-functions are members of main AnyFunctions object, so that a polymorph function will have multiple mono-function member objects. In AnyFunEx objects, you specify your instances using objects of AnyFunExMonoBase-derived classes. For C++, one must use the class AnyFunExMonoC and the class AnyFunExMonoPy contains the interface for Python coded functions.

In the mono-function objects, one specifies the list of arguments of the particular instance of the function. The external code must of course match this list of arguments. Moreover the name of mono-function objects must be the same name as the function in the external code. In addition to the list of arguments, the return value must also be defined. The return is common for all instances of the function, i.e. for all mono-functions. It is therefore to be defined in main function (AnyFunEx) object where an AnyValue-derived member called Return must be defined. These three requirements must be fulfilled for proper interfacing to the externally coded function.

It should be noticed that the data interface for arguments and return value currently does not support all AnyValue-derived data types. Only the simplest types AnyInt, AnyFloat, and AnyString (and derived specializations) are supported.

Finally notice that, when called in AnyScript, it is always the name of the main function object that must be used, i.e. the name of the AnyFunEx object containing the mono-functions.

## When using the Force-Dependent-Kinematic function, how many times is this external function evaluated: with each iteration of the FDK solver or only once at the beginning of the quasi-static calculation of the kinematics?

To solve the Force-Dependent-Kinematics, the forces are calculated for each guess for kinematic positions. So since your python function is used to calculate forces based on positions, it should be evaluated in every iteration step (we are not aware of anything different).

## Small example using Python hook to make a simple differentiation

Small model, which displays how-to, do differentiation using Python

The model has one segment and one joint, the joint is driven with an interpolation function.

The input to the differentiation function is in this example the joint position, and the differentiation returns in this example the differentiated position, the velocity.

The python function works by saving the value to a file and for each timestep it reads the value from the previous step and do the differentiation.

Note: the first value is not to be trusted Note: the amount of timesteps will change the results, the differentiation is based on the calculated steps not a pertubation!

Try to run the kinematic analysis, then compare

```
Main.MyStudy.output.Model.result with Main.MyStudy.Output.Model.Jnt.Vel
```

The model consist of two files an AnyBody model and a python function [DifferentiationModel.zip](https://github.com/AnyBody/support/blob/master/Wiki_Files/Python_Hook/DifferentiationModel.zip?raw=true)

## Add modules to python

If you follow this [link](http://www.lfd.uci.edu/~gohlke/pythonlibs/), you find a list of downloads for extra modules and packages for python.

