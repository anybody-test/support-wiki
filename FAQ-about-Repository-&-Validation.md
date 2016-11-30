# Repository specific

## Where can I find the unscaled/standard parameters of the body (segments size and mass)?

When the model is scaled the scaling parameters are all included in the AnyMan file. There is a version of this AnyMan file used as reference, it contains the size and mass of all segments set to their unscaled value. It also contains the total unscaled weight and hight of the body. This file can be found in Body/AAUHuman/Scaling/AnyFamily/AnyMan.any.

##How do i read out reaction forces from a joint and which coordinate system is being used?

1. First you have to find the joint in question in the output tree, for the right hip joint this link will be Main.HumanModel.BodyModel.Right.Leg.HipJnt

2. Then you have to point at the contraints reactions of this joint, these are always found as .Constrains.Reaction

3. This means that the full link for the hip reactions is "Main.HumanModel.BodyModel.Right.Leg.HipJnt'.Constraints.Reaction

4. Then the next question is which coordinate system are these forces measured in? this is the first mentioned reference frame of the joint. So if you do not know which one this is then you have to locate the joint definition in the script. If you do not know where to find this you can look find the joint in the ModelTree and by a right click you will be able to select "Locate in script"

It may sound a bit complicated, so in one of the future releases of the repository a new folder containing selected output from the human model will be available this should make this easier.

# Validation

## What type of experiments do you recommend to perform in order to validate results obtained with the model/system?

You can imagine a number of different methods, and we have been trying them all on different models while we were developing the technology.

1. EMG versus activity. The advantage is that we are measuring on the individual muscles directly. The disadvantage is the problem of correlating EMG to muscle force and the fact that it is difficult or impossible to measure all muscles.

2. Moment arm validation. It is possible to compute the moment arm of the model's muscles and compare them to values from cadaver studies.
3. Prediction versus measured forces between the body and the environment. This is not a good method for gait, but in many other cases it works well. For instance, in pedaling, the pedal force can be divided into a radial and a tangential component. With two feet this gives us four forces to produce a single crank torque. There are infinitely many combinations of these four forces that can produce the correct crank torque, and the four forces are functions of the muscle recruitment. If the system predicts the correct pedal forces, then this indicated that the muscle recruitment is also reasonable.

## How sensitive are the results to the number of muscles included in the model?

This is not easy to answer accurately. Of course, all major muscles are important, their strength properties as well as their moment arms. In order to model moment arms reasonably, it is necessary to divide larger muscle into branches, so that the combined effort of the branches match the one of a complicated muscle complex. We believe that the model we have presented cannot be much simpler and still be a meaningful 3D model. On the other hand it is very difficult to find proper data for a more accurate model. There is certainly room for adjustments and improvements, which is also one of the ideas of the repository, namely that all users can help each other improve the models.

## How do you know that the muscle forces and activation patterns computed by the system are correct?

Oops, that's a difficult one. The fact is that we don't. What we know is that if a number of conditions are fulfilled, then the results are not way off. These conditions are: The model must correspond to the mechanics of the muscle system you want to investigate. What "correspond" means is a matter for the skilled user to determine. As with all computer modeling of natural phenomena, there is a level of model complexity that will give you the desired accuracy, but only experience can tell you in advance what that level is. If you have no experience, consult the literature. If there is no literature, try different levels of complexity and estimate the convergence of the result. The movement you are modeling must be skilled. This is because AnyBody to resolve the inherent indeterminacies of the living body assumes that the body operates optimally. This requires some level of skill. The movement must be so slow that the muscles have time to activate and de-activate as quickly as the simulation predicts. Muscle activation is an electro-chemical process. and it takes time. AnyBody does not take this time into consideration when it recruits muscle forces, so there is a danger that it may predict onset and offset of muscles faster than what the organism can really accomplish. Why do we know that, given these conditions, the result is not way off? Because the muscle recruitment of AnyBody honors a number of known properties of musculoskeletal systems: These systems are subject to the laws of nature, more specifically the laws of mechanics. Certain principles of equilibrium, motion, power, and energy are given by the laws of mechanics, and the simulations of AnyBody are entirely built on these. Muscles collaborate when they can. This means that if several muscles are spanning the same joint, then they share the load between them in such a way that stronger muscles carry more load than weaker muscles. This principle is honored by the simulations of AnyBody. Antagonistic muscles are a fact of life. They are muscles that appear to be working against the movement in the sense that they perform negative work. AnyBody predicts the presence of antagonistic muscles when they are necessary and even explains their purpose. When the load is increased, the muscles will collaborate in such a way that no muscle is loaded above its physiological strength, if there is another way of carrying the load. AnyBody honors this principle. If you get muscle activations above 1, then it is because there is no way to carry the applied load with the muscle configuration you have specified. The fact of the matter is that the simulation result of AnyBody as all other numerical analysis is inaccurate to a certain extent. But this does not mean that it is without value. You will find when you use AnyBody that detailed models of the musculoskeletal system can provide you with much, much more information than you can find from qualitative or experimental investigations alone.