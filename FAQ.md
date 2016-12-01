+ [Kinematics](#kinematics)
    - [Is it possible to introduce artificial joints, i.e. total knee replacements (TKR), or the introduction of an orthosis for the foot? Could a TKR be incorporated into the model to predict joint contact forces and ligament load sharing?](#is-it-possible-to-introduce-artificial-joints-ie-total-knee-replacements-tkr-or-the-introduction-of-an-orthosis-for-the-foot-could-a-tkr-be-incorporated-into-the-model-to-predict-joint-contact-forces-and-ligament-load-sharing)
    - [How can the AnyBody Modeling System handle closed chains in the model?](#how-can-the-anybody-modeling-system-handle-closed-chains-in-the-model)
    - [Does the system have a "welded" joint that creates a rigid connection between two segments?](#does-the-system-have-a-welded-joint-that-creates-a-rigid-connection-between-two-segments)
    - [What is the role of the initial positions of the segments?](#what-is-the-role-of-the-initial-positions-of-the-segments)
    - [Why is the Pos property of a driver zero once the kinematics has been solved?](#why-is-the-pos-property-of-a-driver-zero-once-the-kinematics-has-been-solved)
+ [Kinetics](#kinetics)
    - [Does the spine rhythm impose any stiffness on the model?](#does-the-spine-rhythm-impose-any-stiffness-on-the-model)
    - [Why is it ok to have a reaction force between the pelvis and environment if you have measured ground reaction forces?](#why-is-it-ok-to-have-a-reaction-force-between-the-pelvis-and-environment-if-you-have-measured-ground-reaction-forces)
    - [How should the force output from the joint be interpreted?](#how-should-the-force-output-from-the-joint-be-interpreted)
    - [I have a different force plate setup than used in the applications of the repository what should I do?](#i-have-a-different-force-plate-setup-than-used-in-the-applications-of-the-repository-what-should-i-do)
    - [How is it possible that the model will calculate automatically the reaction forces with the environment?](#how-is-it-possible-that-the-model-will-calculate-automatically-the-reaction-forces-with-the-environment)
    - [Can elastic elements (ligaments, cartilage..) be modelled?](#can-elastic-elements-ligaments-cartilage-be-modelled)
    - [How do you specify the center of pressure during the gait?](#how-do-you-specify-the-center-of-pressure-during-the-gait)
    - [Does the AnyBody Modeling System compute joint moments?](#does-the-anybody-modeling-system-compute-joint-moments)
    - [I have a body model that is supported by the floor/a chair/a bed/a bicycle/... Do I have to specify the reaction forces or can the model compute them automatically?](#i-have-a-body-model-that-is-supported-by-the-floora-chaira-beda-bicycle-do-i-have-to-specify-the-reaction-forces-or-can-the-model-compute-them-automatically)
    - [Can i constrain the reaction forces to have a certain direction?](#can-i-constrain-the-reaction-forces-to-have-a-certain-direction)
    - [How can i constraint the magnitude of a reaction force?](#how-can-i-constraint-the-magnitude-of-a-reaction-force)
+ [CAD files](#cad-files)
    - [What type of data is required to import a particular bone morphology?](#what-type-of-data-is-required-to-import-a-particular-bone-morphology)
    - [What is the source of your bone geometry data?](#what-is-the-source-of-your-bone-geometry-data)
+ [Validation](#validation)





# Kinematics

## Is it possible to introduce artificial joints, i.e. total knee replacements (TKR), or the introduction of an orthosis for the foot? Could a TKR be incorporated into the model to predict joint contact forces and ligament load sharing?

Yes. A very important point of our technology is that it is a MODELING system. This means that the users defines/constructs the models, and as such the user also has complete access to make any modification of the existing repository models. In fact, we are hoping that our users will continually add new models and modifications to the repository.

## How can the AnyBody Modeling System handle closed chains in the model?

Bicycling is a typical closed-chain example. The legs and crank together form a closed mechanical chain. The AnyBody Modeling System handles closed chains in the mechanism, but it is true that many other systems for mechanism analysis do not. There are really two distinct problems connected with closed chains. The first problem is in the kinematics where closed chains complicate the solution algorithm because make it impossible to resolve the positions by stepping from one joint to the next until the free ends of the mechanism are reached. The AnyBody Modeling System employs a very general method for position analysis and does not rely on assumptions of open chains. The second problem is in the computation of muscle forces. The classical way of doing inverse dynamics is to compute joint moments and subsequently distribute them onto the individual muscles. When there are closed chains present, the problem of resolving joint moments may not have a unique solution. However the AnyBody Modeling System does not go via joint moments. Instead it sets up a redundant system of equilibrium for the entire system including the muscles. This redundant system is subsequently solved uniquely via the system's advanced muscle recruitment algorithms.

## Does the system have a "welded" joint that creates a rigid connection between two segments?

Yes, the AnyStdJoint locks all mutual degrees of freedom between the segments it connects.

## What is the role of the initial positions of the segments?

The initial positions, only serves as the initial guess on the segments positions. This means that if the model assembles correctly one can be satisfied with the initial positions. They will have no influence on any analysis results. If the model is a closed chain mechanism then there might be several kinematic solutions that will fulfill the kinematic constraints on the model. In such a case the initial position can be used to ensure that the model chooses the correct solution, simply by providing initial positions which are close to the correct one.

## Why is the Pos property of a driver zero once the kinematics has been solved?

The position of the driver should be zero when the kinematics has been solved, this is because the position displays the error on the driver. If you are looking for the position of the measure being driven, please look at the position of this measure.

# Kinetics

## Does the spine rhythm impose any stiffness on the model?

The spine rhythm matrix does not represent the stiffness of the spine. Actually the spine model has no passive stiffness implemented at the moment. The SRmatrices.any file which defines the spine rhythm is for the kinematics only. The coefficients in the matrix specify the rotation of each lumbar vertebra to a certain fraction of the rotation of T12/Pelvis. The T12/Pelvis rotation is the input of the lumbar driver. The SRMatrixes define the shape of the spine between T12 and the pelvis.

## Why is it ok to have a reaction force between the pelvis and environment if you have measured ground reaction forces?

If you have a full body model you will also need a reaction to the environment to take up residual forces, in principle this residual should be zero but this will never the cased due to differences in kinematics, anthropometry, and skin artefacts, measuring noise also plays a role. If looking at a model with only the legs and the pelvis the issues are the same but in this case the reaction force will carry both the residual and the missing forces between the pelvis and the upper body.

## How should the force output from the joint be interpreted?

Answer: The reaction forces displayed in a joint are always given wrt to the first mentioned coordinate system used in the joint definition, so if in doubt you need to locate the joint definition. If looking at the hip joint these forces are displayed in the local frame of the femur and not in the frame of the pelvis. Since the hip joint is a spherical joint it has three components in the reactions: Reaction.Fout[0], Reaction.Fout[1] and Reaction.Fout[2]. Component [0] refer to the first axis (X in this case), components [1] refer to the second axis (Y in this case) and component [2] refer to the third axis (Z in this case). You can see in the legend box of the graph which colours correspond to which component. In case of a revolute joint there would be five constraints, three forces and two moments.

## I have a different force plate setup than used in the applications of the repository what should I do?

Basically you need to model this in the “environment.any” file, but first you need to look at the definition of your forceplate at www.c3d.org , in the manual you will find a forceplate section, there are several types defined. You will need to find yours and review its properties.
One very important thing to remember is that the sequence of the feet on the plates must fit with the experiment so if right foot is on the first plate, this should be reflected in the AnyReacForce between the right foot and the first plate.
Applying the forces you need to double check that the reference frame you are using for applying the force has a orientation according to the lab setup, this is of course essential!


## How is it possible that the model will calculate automatically the reaction forces with the environment?

The system computes reaction forces automatically. If your model is supported in a statically determinate fashion, then there is only one unique set of reaction forces that balances the system correctly, and AnyBody will compute if for you. What happens then if the system is not statically determinate? Just to give you an idea of such a situation, consider a body standing on hands and knees on the floor. It needs only three support points to be stable, but it has four points. AnyBody handles this case through its muscle recruitment. There are infinitely many combinations of the four support forces that balance the model, and the human body distributes the force between the two hands and two knees by contracting and relaxing muscles until a reasonable equilibrium is attained. You can think of a situation where the majority of the support force was carried by one of the hands. The body is likely to feel that this hand is overloaded and seek to redistribute the force to the two legs and the other hand until a more even load distribution was found. This is exactly what the AnyBody Modeling System does. It looks at all the muscle loads in the system and finds the distribution of muscle forces that optimizes the load distribution. In some cases it can improve the accuracy of the analysis tremendously if you have measured reaction forces available and impose them on the model rather than let the system compute them. This is particularly the case for gait and other very dynamic problems where a large part of the reaction force comes from balancing of inertia in the system. If you are modeling a measured gait cycle, then you have essentially measured positions, and they must be differentiated twice to get the accelerations the system needs to resolve the inertia forces


## Can elastic elements (ligaments, cartilage..) be modelled?

You can actually specify ligaments and other (passive element) forces that depend on position and velocity (in general springs and dampers with almost any explicit expression you can come up with). In particular, we have a ligament object class which is specially designed for ligament, but in general it is nothing else than an applied force. The downside is that the ligament forces cannot be a function of the muscle forces in this way, since they merely enters the inverse dynamics solution as applied forces. We hope to resolve this problem in a future version. One could currently model ligaments as simple muscles. This has, as far a I know, been done in some studies in the literature.

## How do you specify the center of pressure during the gait?

The ground force is measured in the gait experiment. It is measured by a force plate and the measurement is converted into a 3D force vector and the associated point of application (center of pressure). Pressure data measure by any other measurement system can similarly be converted into point loads. A point load is adequate in the model, where the foot is a single rigid segment. The position of the center of pressure is defined in the data set. We simply create a force plate (as a segment) in the model and add the measured forces to this segment. The position of the force plate is controlled by the measured data. The contact between the foot and the force plate is made using a reaction force that transfers all forces applied to the force plate to the foot.

## Does the AnyBody Modeling System compute joint moments?

In principle it does, since it completely resolves all mechanical aspects of the problem. But you are probably thinking whether the system initially computes joint moments and then proceeds to distribute them on the muscles spanning each joint. This is not the way the AnyBody Modeling System does it. Contrary to popular belief, it is really not a viable solution. The explanation is that because we have many bi-articular muscles, there is not unique solution to the joint moment distribution problem. Depending on the actions of the bi-articular muscles, net moment may be transferred from one joint to the other. Instead, the AnyBody Modeling System sets up a redundant system of equilibrium for the entire system including the muscles. This redundant system is subsequently solved uniquely via the system's advanced muscle recruitment algorithms. But the fact of the matter is that for joints with bi-articular muscles. the concept of joint moments really does not exist.


## I have a body model that is supported by the floor/a chair/a bed/a bicycle/... Do I have to specify the reaction forces or can the model compute them automatically?

The system computes reaction forces automatically. If your model is supported in a statically determinate fashion, then there is only one unique set of reaction forces that balances the system correctly, and AnyBody will compute if for you. What happens then if the system is not statically determinate? Just to give you an idea of such a situation, consider a body standing on hands and knees on the floor. It needs only three support points to be stable, but it has four points. AnyBody handles this case through its muscle recruitment. There are infinitely many combinations of the four support forces that balance the model, and the human body distributes the force between the two hands and two knees by contracting and relaxing muscles until a reasonable equilibrium is attained. You can think of a situation where the majority of the support force was carried by one of the hands. The body is likely to feel that this hand is overloaded and seek to redistribute the force to the two legs and the other hand until a more even load distribution was found. This is exactly what the AnyBody Modeling System does. It looks at all the muscle loads in the system and finds the distribution of muscle forces that optimizes the load distribution. In some cases it can improve the accuracy of the analysis tremendously if you have measured reaction forces available and impose them on the model rather than let the system compute them. This is particularly the case for gait and other very dynamic problems where a large part of the reaction force comes from balancing of inertia in the system. If you are modeling a measured gait cycle, then you have essentially measured positions, and they must be differentiated twice to get the accelerations the system needs to resolve the inertia forces upon which the ground forces will eventually depend. Every time you differentiate measured data you increase the inevitable measurement inaccuracy by an order of magnitude, so ground reaction forces are not strictly necessary, but they help a lot for dynamical problems.


## Can i constrain the reaction forces to have a certain direction?

It is actually possible to restrict the directions of the reaction forces to be within a certain direction, if they are created with the AnyGeneral muscles, it can not be done directly in the definitions of the constraints.
This exactly what is done in the GH joint in the shoulder!. The GH joint is a spherical joint, which reactions have been switched off. Switching off the reaction is possible in all joints, by changing the following property. Constraints.­Reaction.­Type, by default all its components will be on but you can remove reactions individually from the joint. For a revolute joint settings like Constraints.­Reaction.­Type={off,­off,off,on,­on}; will mean that all three forces reactions are switched off whereas the moment reactions is still On. In the GH joint we wanted to constrain the direction of the reaction force to fall within the glenoid cavity of scapula. To ensure this, eight pushing muscles now acts from the gh joint rotation center onto the edge of the glenoid cavity, to try to picture this setup it resembles the frame of a tipi tent!.
Since each of the muscles can only push there is no way that the resultant force can fall outside the cavity of the glenoid cavity.
The setup of the gh reactions can be found in the file body/aauhuman/­arm/ghreaction.­any, this is a good example on how to implement this.

## How can i constraint the magnitude of a reaction force?

This is basically not possible, but by redefining the reaction using an AnyGeneral muscle you can do something which may resemble it. If the reaction is replaced by a muscle which is very strong the activity of this muscle will not affect the activation of the regular muscles in the human model. If the strength of the muscle is gradually lowered the recruitment of the regular muscles in the body will gradually be affected. Generally speaking if the strength of the reaction is low enough the model will feel it like it is standing on a needle and it will try to recruit all muscles in a way to avoid loading this reaction, if this is possible. So by adjusting the magnitude of the strength of this muscle different results can be obtained, but it is not possible to have a certain upper bound on the magnitude.

# CAD files

## What type of data is required to import a particular bone morphology?

The input for AnyBody is simple STL files. They are converted by AnyBody to ease subsequent handling of the data, so you are not able to find STL files in the repository, but that is all you need to input your own geometry data. The STL files can also represent the environment such as wheelchairs, floors, tools, and other model elements.

## What is the source of your bone geometry data?

They are from 3D scans from plastic skeletons. They are actually not in a very good quality and we would like to get access to better data, but of course they need to be without all sorts of restrictions about their use; this is both due to our own usage and due our desire to place them in the AnyBody Managed Model Repository.

# Validation

## What type of experiments do you recommend to perform in order to validate results obtained with the model/system?

You can imagine a number of different methods, and we have been trying them all on different models while we were developing the technology.
    1) EMG versus activity. The advantage is that we are measuring on the individual muscles directly. The disadvantage is the problem of correlating EMG to muscle force and the fact that it is difficult or impossible to measure all muscles.
    2) Moment arm validation. It is possible to compute the moment arm of the model's muscles and compare them to values from cadaver studies.
    3) Prediction versus measured forces between the body and the environment. This is not a good method for gait, but in many other cases it works well. For instance, in pedaling, the pedal force can be divided into a radial and a tangential component. With two feet this gives us four forces to produce a single crank torque. There are infinitely many combinations of these four forces that can produce the correct crank torque, and the four forces are functions of the muscle recruitment. If the system predicts the correct pedal forces, then this indicated that the muscle recruitment is also reasonable.

## How sensitive are the results to the number of muscles included in the model?

This is not easy to answer accurately. Of course, all major muscles are important, their strength properties as well as their moment arms. In order to model moment arms reasonably, it is necessary to divide larger muscle into branches, so that the combined effort of the branches match the one of a complicated muscle complex. We believe that the model we have presented cannot be much simpler and still be a meaningful 3D model. On the other hand it is very difficult to find proper data for a more accurate model. There is certainly room for adjustments and improvements, which is also one of the ideas of the repository, namely that all users can help each other improve the models.


## How do you know that the muscle forces and activation patterns computed by the system are correct?

Oops, that's a difficult one. The fact is that we don't. What we know is that if a number of conditions are fulfilled, then the results are not way off. These conditions are: The model must correspond to the mechanics of the muscle system you want to investigate. What "correspond" means is a matter for the skilled user to determine. As with all computer modeling of natural phenomena, there is a level of model complexity that will give you the desired accuracy, but only experience can tell you in advance what that level is. If you have no experience, consult the literature. If there is no literature, try different levels of complexity and estimate the convergence of the result. The movement you are modeling must be skilled. This is because AnyBody to resolve the inherent indeterminacies of the living body assumes that the body operates optimally. This requires some level of skill. The movement must be so slow that the muscles have time to activate and de-activate as quickly as the simulation predicts. Muscle activation is an electro-chemical process. and it takes time. AnyBody does not take this time into consideration when it recruits muscle forces, so there is a danger that it may predict onset and offset of muscles faster than what the organism can really accomplish. Why do we know that, given these conditions, the result is not way off? Because the muscle recruitment of AnyBody honors a number of known properties of musculoskeletal systems: These systems are subject to the laws of nature, more specifically the laws of mechanics. Certain principles of equilibrium, motion, power, and energy are given by the laws of mechanics, and the simulations of AnyBody are entirely built on these. Muscles collaborate when they can. This means that if several muscles are spanning the same joint, then they share the load between them in such a way that stronger muscles carry more load than weaker muscles. This principle is honored by the simulations of AnyBody. Antagonistic muscles are a fact of life. They are muscles that appear to be working against the movement in the sense that they perform negative work. AnyBody predicts the presence of antagonistic muscles when they are necessary and even explains their purpose. When the load is increased, the muscles will collaborate in such a way that no muscle is loaded above its physiological strength, if there is another way of carrying the load. AnyBody honors this principle. If you get muscle activations above 1, then it is because there is no way to carry the applied load with the muscle configuration you have specified. The fact of the matter is that the simulation result of AnyBody as all other numerical analysis is inaccurate to a certain extent. But this does not mean that it is without value. You will find when you use AnyBody that detailed models of the musculoskeletal system can provide you with much, much more information than you can find from qualitative or experimental investigations alone.

# Muscle recruitment

## Is muscle activation in your model the same as EMG? Is muscle activation calculated by the AnyBody system is a % of isometric MVC? Can EMG become an input into the model?

The model predicts muscle activations that are mathematically defined as the muscle force divided by the strength of the muscle. We use the current strength of the muscle, which depends on the muscle model you apply in the muscle definition. The simplest muscle model use isometric MVC for all configurations of the muscle, but we also have a more complex 3-element Hill-type muscle model, where the strength depends on muscle length and contraction velocity. From this mathematical definition of Muscle Activation, it is obvious that it is not equivalent to EMG, though their variation patterns must be alike You can import EMG data for visualization along with computed data. You can also use EMG data to apply forces on the model, which will be a kind of prescribed muscle forces. But you cannot drive the model completely by EMG since it is an inverse dynamics model. To make an EMG driven model, you must apply a forward dynamics simulation, which is currently not available in AnyBody.

## Which kind of inverse dynamics equation do you use? Kane or Lagrange, or Euler? Where do I find more detailed information about the dynamics equations you are using?

The kernel of AnyBody's mechanical module uses what we often refer to as a Full Cartesian Formulation using a rigid-body set of coordinates for each segment (rigid body). The dynamic equations are basically the Newton-Euler equations, but they are transformed during the analysis. We believe that any approach for setting up the equations should produce the same results; efficiency and flexibility are the main issues here. Our approach largely follows the textbook of Nikravesh (University of Arizona).

## Can this software be used in forward dynamics calculation?

Forward dynamics simulations are not possible in AnyBody (yet). It is our hope to be able to include it at some point in the future. We have the whole mechanical engine but still there is some way to go. The requirements for making good inverse and forward dynamic solutions are somewhat different.

## How is time varying activation dynamics included in the analysis?

It is not. The muscle activation is allowed to change instantaneously, so we are assuming - like most other inverse dynamics approaches - that the activation dynamics is "fast enough".

##Does AnyBody include activation dynamics in the muscle modeling?

No. AnyBody is based on inverse dynamics, so it computes the movement first and afterwards recruits the muscle forces to realize it. Inherent to this process is the assumption that muscle forces are available as quickly as they are needed. AnyBody also assumes that the body recruits muscles "optimally". More precisely, this means that AnyBody makes the muscles cooperate maximally in such a way that fatigue is postponed as far as possible.

# Muscles/Ligaments

## Can I include elastic elements such as cartilage and ligaments in my model?

The short answer is no. AnyBody is a rigid body dynamics system, and the basic assumption is that all elements are non-elastic. However, the system's muscle model does include the effects of the serial-elastic and parallel-elastic elements of the muscles. Ligaments can in principle be modeled as muscles with no active element, but this is very difficult, and the model will be extremely sensitive to small variations in length of these ligament elements. AnyBody Technology is investigating better methods for modeling of such passive elastic elements.

## How does the system incorporate any forces imposed by a muscle interacting with a bone (i.e. wrapping)?

Muscles can interact with bones at fixed points (so-called via-points) and by wrapping surfaces. Currently, muscle lines can wrap over primitives such as spheres, ellipsoids, and cylinders. You can make any set of primitives for a given muscle.

## Can you get stress data for bones and color stress distribution like FEA software?

No not directly. AnyBody is not a FEA code. You can export force data from AnyBody which you may be able to apply to a FE model.
HOWEVER, Ozen Engineering, an AnyBody distributor located in the United States, has worked with the AnyBody developers in creating an interface that allows for the results from an AnyBody model to be used with commercial FEA software. An associated wiki-page regarding the Any2Ans software can be found here: Link To Any2Ans Wiki Page

## Are the material properties of the muscles, tendons, and bones included in the software?

The user can change all model properties. Typically, we operate with more general properties than actual material properties since we are not modeling the material itself as in a FE model. The muscle properties (and the properties of all other objects in the models) are in the (open) AnyScript text files. The software itself does not contain a fixed model, neither parameters nor topology.

## Why should I run a calibration when using the 3 elements muscle model? And how does it work?

One of the challenges in working with the detailed 3 elements muscle model is the dependency on defined tendon length, Lt0. The model is very sensible to this elastic element and a small variation in the length can be significant for the muscle activity. So how to find the right tendon length? The calibration study will do it for you. We believe that humans are equipped with tendons whose lengths are advantageous for our normal activities. We can use this knowledge to calibrate the tendon lengths for an individual of a certain size. Quite simply, we shall presume that the tendon lengths are calibrated by nature to provide the muscles with optimum fiber lengths at certain postures. And so does the calibration sequence in AnyBody: each group of muscle is associated with a certain set of joint postures for which the muscle is presumed to be at its neutral position and the tendon length is adjusted to give this muscle its optimal fiber length.

## What if a muscle model does not converge?

When using the 3 elements muscle model you might get an error stating that "Muscle model 'Main...' did not converge. Please check muscle length parameters." This is typically a calibration problem. The 3 elements model has to solve an internal kinematic between the tendon and the contractile element, depending on the length of each of them. An initial length is defined in the code for the tendon and the contractile element but it is not meant to be the optimal one and a calibration is needed to give those elements their optimal length in function of the model anthropometry. If the calibration is not run then problems can occur such as, for example, the tendons being too long and not leaving any space for the muscle between them. This problem will be solved by running the calibration sequence.

# AnyBody Modeling system

## Does the AnyBody Modeling System allow plug-ins? For example, can the user specify his own muscle model or apply a new analysis?

Both yes and no! We do not have an interface (exit) that allows you to program your own bit of the analysis in like C or FORTRAN and link that to AnyBody. We would like to do so of course, but it is not on the here-and-now-to-do list. We have a few close collaborators where we made special arrangements though, but we have not yet done so in a more formal way. We do however plan to open as much as possible for customization of objects from the interface and muscle models is indeed one issue where we will. Another issue is the definition of the objective function for muscle recruitment. We are currently working on a new feature to the system that will allow the user to define his/her own muscle model. If the user can formulate the muscle strength as an analytical function of length and contraction velocity, then he can code it into his own muscle model and use it in the analysis. We are always very open for suggestions on model features, we ought to include. Any feedback on desired customization options and maybe even plug-ins are very welcome.

## Why did you choose to develop a text-based interface for constructing models rather than a graphical interface?

A model needs an underlying data structure. In systems with graphical interfaces such as CAD systems, the norm is that this data structure is hidden from the user, and he or she only sees the model through the graphical interface. This can work very well for many problems. However, a body model can be a very complicated set of data, and users will quickly find themselves in a situation where they desire a closer relationship with the data they are working on. So we have decided to let the user work directly on the data of the system. To help in this job, we will continue to develop better editing and browsing facilities in the system. This is much the same philosophy as well-known and popular systems such as Matlab and Tex.

## Can I export computation data to my spreadsheet?

Yes. In the chart view, when you click the "Copy to clipboard" icon, you are presented with a menu that allows you to choose to export data as text. If you do so, simply paste the data into your spreadsheet, and you get the curves in the picture represented as columns of numbers in the spreadsheet.

# Repository specific

## Where can I find the unscaled/standard parameters of the body (segments size and mass)?

When the model is scaled the scaling parameters are all included in the AnyMan file. There is a version of this AnyMan file used as reference, it contains the size and mass of all segments set to their unscaled value. It also contains the total unscaled weight and hight of the body. This file can be found in Body/AAUHuman/Scaling/AnyFamily/AnyMan.any.

## How do i read out reaction forces from a joint and which coordinate system is being used?

    1) First you have to find the joint in question in the output tree, for the right hip joint this link will be Main.HumanModel.BodyModel.Right.Leg.HipJnt
    2) Then you have to point at the contraints reactions of this joint, these are always found as .Constrains.Reaction
    3) This means that the full link for the hip reactions is "Main.HumanModel.BodyModel.Right.Leg.HipJnt'.Constraints.Reaction
    4) Then the next question is which coordinate system are these forces measured in? this is the first mentioned reference frame of the joint. So if you do not know which one this is then you have to locate the joint definition in the script. If you do not know where to find this you can look find the joint in the ModelTree and by a right click you will be able to select "Locate in script"

It may sound a bit complicated, so in one of the future releases of the repository a new folder containing selected output from the human model will be available this should make this easier.