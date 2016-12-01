## General Notes

When you get an error message in the AnyBody Modeling System, you will get an explanation of what happened. If this explanation is not sufficient, you can find additional info on the errors in the Reference Manual, in the Forum here at AnyScript.Org or in the following section:

## SPLine OOSol

0.0.2) ...Kinematic analysis completed. The kinematic constraints have been resolved. ERROR(OBJ.LIB.OOS1) : D:/Anybody/..l/MuscleDef.any : infraspinatus_1.SPLine : Unexpected exception in the library OOSol : OOSol exception : MISSING MESSAGE.

The SPline error means that the muscle failed to wrap correctly over the surface.

## xxx independent constraints and yyy unknowns

Position analysis failed : 99 independent constraints and 100 unknowns - attempts to continue (attempt no. 10) ERROR(OBJ.MCH.KIN3) : D:/A..3/A..n/E..s/r..y/GaitLowerExtremity.main.any : KinematicStudyForParameterIdentification.InitialCo nditions : Kinematic analysis failed in time step 0

That means there are more degrees of fredom than drivers in the model. If it is a marker driven model, it surely means that there are not enough markers. Otherwise a driver is missing. If the difference between independent constraints and unknowns is 1, it means that one driver direction is missing.

##Wrong initialization of AnyVarRef

ERROR(OBJ.DES1) : C:/.../LegCal1.any  : LegCalibrationStudy1.Internal_dependency  : Wrong initialization of AnyVarRef linking to value Lfbar in study LegCalibrationStudy1 : Dependency (Right-hand side or internal dependency) may be evaluated later than this AnyVarRef object. This is not allowed, because it could override the AnyVarRef value transfer. Location of Lfbar : C:\...\AMMRV1.4\Body\AAUHuman\LegTD\MusPar.any(827)

Due to a different handling of AnyVarRef in AnyBody 5.2 this error message will appear during the loading of the model. However, this error will not stop loading the model. It should still run. Did your client manage to load the model?

Solution 1: use AMS 5.2 and AMMR 1.5

Solution 2: use AMS 5.1 and AMMR 1.4