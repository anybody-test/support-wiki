## Descriptions of the FrictionContactMuscles

In several cases not all boundary conditions in form of external forces have been measured. AnyBody has the possibility to predict those forces.

Typical applications can be found in the SeatedHuman demo example where the normal and frictional forces between a human and a seat are predicted.
Other cases are the prediction of ground reaction forces as done in Jung et al 2013, Jung et al 2012, Buchner 2013, among others.

In the ‘\AMMR\Body\AAUHuman\ToolBox\FrictionContactMuscles\’ folder, there are several AnyScript files which can be used for the modeling of the normal and frictional contact forces.

In many applications we want to predict these forces and AnyBody can do that by regarding those unknown forces as 
AnyGeneralMuscle forces. Then all unknown forces can be solved by the muscle recruitment optimization process.

In the above mentioned folder are following basic files and some class templates :
+ ContactSurfaceLinPush.any file: the most important file
+ ConditionalContactClass.any file: basic class template
+ ConditionalContact3PointsClass.any file: class template with 3 target points (tripod)
+ ConditionalContactFootPlaneClass.any file: class template aimed for the human foot

All the class templates use the ContactSurfaceLinPush.any file in principle.

So it is good for you to understand how this ContactSurfaceLinPush.any file works. In this chapter we will try to figure it out.
## 1. Input conditions for ContactSurfaceLinPush.any file

There are several input conditions which are required to use this ContactSurfaceLinPush.any file to build the normal and the frictional forces.
+ BaseObject (AnyRefFrame&): the base object of which role is similar to the base of the contact such as ground.
+ TargetObject (AnyRefFrame&): the target object which you expect to get the normal and the frictional forces from the BaseObject.
+ StrengthObject (AnyRefFrame&): should be the same object reference as TargetObject.
 UserDefinedLimitLow (AnyVar): the lowest limit of the vertical distance between the BaseObject and the TargetObject to judge whether the contact occurs or not. This should be a negative value.
+ UserDefinedLimitHigh (AnyVar): the highest limit of the vertical distance between the BaseObject and the TargetObject to judge whether the contact occurs or not. This should be a positive value.
+ UserDefinedRadiusLimit (AnyVar): the radius limit of the horizontal distance between the BaseObject and the TargetObject to judge whether the contact occurs or not. This should be a positive value.
+ Strength (AnyVar): the maximum strength (force) of the normal force component.
+ StaticFrictionCoefficient (AnyVar): the friction coefficient between the normal and the frictional components. This value should be between 0 to 1.
+ NormalDirection (AnyInt): the index of the normal direction of the contact with respect to the BaseObject (0: x-axis / 1: y-axis / 2: z-axis)
+ FrictionDirection1 (AnyInt): the index of the first frictional direction of the contact with respect to the BaseObject (0: x-axis / 1: y-axis / 2: z-axis)
+ FrictionDirection2 (AnyInt): the index of the second frictional direction of the contact with respect to the BaseObject (0: x-axis / 1: y-axis / 2: z-axis)
+ ScaleFactor (AnyVar): the scale factor for drawing force vectors

Following picture shows the geometrical relationship between the BaseObject and the TargetObject when the contact occurs.

When the TargetObject may be inside the virtual cylinder which can be defined by UserDefinedLimitHigh, UserDefinedLimitLow, UserDefinedRadiusLimit and the BaseObject, then the contact will occur.

![image](https://cloud.githubusercontent.com/assets/22542671/20753296/fd0167cc-b705-11e6-9b69-378842124661.png)

## 2. How will the normal and the frictional force be created?

In the ContactSurfaceLinPush.any file are several AnyGeneralMuscle class instances which can form the normal and the frictional forces from the BaseObject to the TargetObject.

Let’s assume that the NormalDirection value as 2 (z-axis). Then the FrictionDirection1 and FrictionDirection2 can be 0(x-axis) and 1(y-axis). And StaticFrictionCoefficient(μ) can be 0.5.

Following picture is the graphical representation of the individual force components which are acting on the TargetObject in the ContactSurfaceLinPush.any file.

![image](https://cloud.githubusercontent.com/assets/22542671/20753311/118b8254-b706-11e6-9562-b733ed3e83aa.png)

There are some relationships between different force components such as:

![image](https://cloud.githubusercontent.com/assets/22542671/20753319/202830be-b706-11e6-83e3-dbd5666239d7.png)
