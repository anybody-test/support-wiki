Extrapolations not allowed
---

When using interpolation functions in the model for drivers, loads, etc. it may happen that you try to prescribe a time value in your study that does not match the time values defined in your interpolation functions. This error will occur if your tEnd has a higher value than the time vector used by your interpolations functions. Let’s say tEnd=10 and the time vector in the interpolation functions are T={0...9}; then you would get the error extrapolations not allowed. The same may happen if you have a tStart value lower than the first item in the time vector. 

Please try the following
1. click the error message you get, this will take you to the right place in the script.
2. find the object in the ModelTree, (you can do this by holding the cursor on the object in the script and right click then select locate in model tree).
3. for the object double click the time vector "T"
4. notice the first and last values of it!
5. locate the tEnd and tStart values of your study in the ModelTree 
6. reduce tEnd to be smaller than the last time vector value
7. increase tstart to be larger than the first time vector value if needed


If the input to the interpolation function is a kinematic measure (for example MyInterpolFun(MyKinMeasure.Pos[0]) you need to ensure that:
1. T vector must contain 0 because when the model loads any kinematic measure is zero and it will make a call to the interpolation function with zero
2. T vector must contain the value of your kinematic measure when all segments are in loading positions. The reason is that this is where the kinematic analysis starts and the interpolation function will be calculated for every step the solver needs to approach the solution

How-to display visually a reference frame 
---
When creating or modifying a model, it is sometimes important to be able to visualize the locations of reference systems. 
The AnyDrawRefFrame object can be used for this.

To add a reference system to a node or a segment in the body model, the following code can be used

1. First create a reference to the reference system, in this case a segment: AnySeg &MySeg  = Main.Model.......MySeg;
2. Then open up the reference and add the visualization coordinate system: MySeg ={ AnyDrawRefFrame drw={ScaleXYZ={1,1,1}*0.1;};};

Alternatively, skip item one and add the AnyDrawRefFrame directly in the segment or node itself.

If you do not remember the full name of the object you can do two things:
1. double click on the object in the ModelView it will automatically open the ModelTree highlighting the object which was clicked. Then right click on the highlighted object and select "Copy->Copy Complete Name" and paste in the name.
2. open the ModelTree and expand it until you have found the object, then click "Copy->Copy Complete Name" and paste in the name.

### Object already declared error
This is a loading error, typically this occurs when you load a model, and you have declared two times the same object in the same folder. To resolve the matter remove one of these objects or rename it. You can click on the links in the error message to locate the lines in the script.

Resolve kinematic problems
---

#### General

The description below is generic and applies to all type of models. If your model is driven by motion capture data please also see this section http://wiki.anyscript.org/index.php/How_to_setup_your_own_MoCap_driven_Model#Kinematic_analysis_fails

1. In the ModelTree locate the study, then right click and select "Object description" this will give a list of all drivers, joints etc in the current model.
2. Sometime a sketch on how the model is driven is a good idea, try to imagine what possibly could cause the problem. Pay attention to redundant drivers, it is not enough that the model DOF count is ok, redundant drivers may cause problems. 
3. If the kinematic error occurs in step 0, try to improve the starting conditions, so that the loading pos is closer to the position in the first time frame. The initial position can usually be changed in the Mannequin.any file.
4. Try to omit parts of the model like the arms or legs to detect where the root of the problem is located
5. If the model contains closed chain kinematics, replace these closed chains with joint drivers on the human. With all the closed chain kinematics removed, the model should run kinematically. A good place to find examples on joint drivers is in the FreePosture model.
6. Re-insert the closed chain constraints (in this case the marker drivers) one by one and remove joint drivers accordingly, making sure that a working model is maintained. This will make it easier to locate the problem.

#### Overdeterminate solver returning NaN
If the kinematic analysis fails and returns NaN it can be caused by these things:
1. A model containing two redundant kinematic constraints (Hard) which are perfectly identical will cause a NaN error. So if you have two identical revolute joints it may cause this. 
2. In some cases it may return this error if a driver is missing for a DOF in the model, so certain dof in the model is not driven. For example if there are no markers on the hand and there are no drivers on the wrist it may cause this error, because the rotations of the hand are undetermined.

Make the solution of the inverse problem stable?
---

Sometimes it can cause troubles to resolve recruitment errors. In general recruitment errors can be caused by an inadequate muscles configuration or missing constraints, that leaves one or several dof. in the model unsupported. The way to resolve this kind of error is to introduce constraints to the joints until it becomes stable; this allows you to detect which dof. is causing the problem, and once this is detected you can address the right solution for it. Please also double cehck the direction of the gravity vector in your study.

In the ModelTree locate the study, then right click and select "Object description" this will give a list of all drivers, joints etc in the current study, it will also provide a list of all active reactions etc., this migth help.

When using the 3 elements muscle model a common problem is to forget to run the [http://wiki.anyscript.org/index.php/FAQ#Why_should_I_run_a_calibration_when_using_the_3_elements_muscle_model.3F_And_how_does_it_work.3F calibration sequence]. Remenber to select RunApplication in the operation tree to run the calibration.

Resolve wrapping problems
---

#### OOS contact solver failiure

Sometimes wrapping of the muscles may cause the solver to fail and it will give an error starting with "OOS Contact solver for wrapping..."
Typically what may happen is that a muscle may slide onto the end cap of a wrapping cylinder and fail. Usually the muscles are not intended to wrap on the cylinder end caps.  

In general the remedy is to 
1. Exclude all muscles from the model except the one causing the problem.
2. Insert an AnyDrawParamSurface in the surface being penetrated to visualize it you can do this directly in the muscle. Typically by writing Surf1={ AnyDrawParamSurf drw={}; }; //assuming a Surf1 reference is existing! 
3. Try to understand the root of the problem, whether it is related to scaling or motion. 
4. Do modifications on either model or motion to solve the problem typically by making the cylinder longer.

If the problem is not related to end sliding then try to increase the StringMesh variable in the muscle this may help, but be aware that it will slow the model.

#### Penetration errors

Many muscles in the body are wrapped over bones and slide on the bony surfaces when the body moves. This means that the contact forces between the bone and the muscle are always perpendicular to the bone surface, and the muscle may in fact release the contact with the bone and resume the contact later depending on the movement of the body. Via point muscles are not capable of modeling this type of situation, so the AnyBody Modeling System has a special muscle object for this purpose.

A wrapping muscle is presumed to have an origin and an insertion just like the via point muscle. However, instead of interior via points it passes a set of surfaces. If the surfaces are blocking the way then the muscle finds the shortest geodesic path around the surface. Hence the name of the class: AnyShortestPathMuscle. The fact that the muscle always uses the shortest path means that it slides effortlessly on the surfaces, and hence there is no friction between the muscle and the surface.  

Sometimes one or some of the points defining a muscle turn out to be located inside one of the surfaces that the muscle can wrap on. In general this should be avoided because a part of the muscle path will be located inside the surface and this part of the path becomes unpredictable.

The penetration errors can occur at different times:
1. They may happen at load time when the model is in its initial position. If they only occur in this position they are not important. It just means the initial posture creates penetration. 
2. They may occur when running inverse analysis of the model. This can be important.

There can be different reasons for this, including:
1. If for example the arm is penetrating the thorax this may give a penetration warning. This is not an error related to the Body model but related to the way the model is being used. Here the remedy is to change the drivers of the model. 
2. In principle all surfaces in the model used for wrapping have been made in a way that makes them scale with the model. When defining a cylinder it is typically defined using three nodes. These nodes control the size and location of the cylinder. When the bone is scaled, these control nodes will also be scaled like any other node on the bone and consequently the cylinder will be scaled too. Ellipsoids are made in a similar way. This setup does not guarantee that penetration errors will never occur. If the via point and the surface are located on the same bone and are very close, different types of scaling and sizes could potentially create penetration errors. 

In general the remedy is to 
1. Exclude all muscles from the model except the one causing the problem.
2. Insert an AnyDrawParamSurface in the surface being penetrated to visualize it you can do this directly in the muscle. Typically by writing Surf1={ AnyDrawParamSurf drw={}; }; //assuming a Surf1 reference is existing! 
3. Insert an AnyDrawRefFrame in the node which is penetrating. 
4. Try to understand the root of the problem, whether it is related to scaling or motion. 
5. Do modifications on either model or motion to solve the problem
