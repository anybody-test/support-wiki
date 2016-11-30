## Model philosophy

+ Most research groups start with a problem and build a model to solve that particular problem
+ We want to build general models, which can give information about a number of yet unknown problems

The goal is to develop general detailed models which:
+ can predict muscle, ligament and reaction forces for a given movement.
+ will facilitate sharing of the model.
+ will give the opportunity to scrutinize and improve the model by other groups

## Repository Model Structure

![image](https://cloud.githubusercontent.com/assets/22542671/20753939/faf0558a-b708-11e6-8884-50f19d012064.png)

## Body Model Structure

A modular block building technique, which makes it easy to change and connect different body parts, has been developed.

The philosophy is that when building for example a leg model, the model should be self-contained.

The Body parts does not contain any motion drivers for the body parts. These are added in the application.

![image](https://cloud.githubusercontent.com/assets/22542671/20754040/5ebe7e16-b709-11e6-8beb-8d8754ff5e32.png)

In the image, the Body parts have no drivers applied.

## The ShoulderArm Model

This model contains data from two different persons. Most of the data that has been used in this model comes from the Dutch Shoulder Group and can be found on the following [webpage](http://homepage.tudelft.nl/g6u61/repository/shoulder/overview.htm). The model is built using data from subject 2 from the VU study and subject 2 from the MAYO study. The files, which contains the name "forearm", are built on data from the MAYO study.

A shoulder rhythm is available in the repository it can be swicthed on and off, for full details on its implementation please see this report [Shoulder Rhythm Report](http://wiki.anyscript.org/images/2/25/ShoulderRhythmReport.pdf).

![image](https://cloud.githubusercontent.com/assets/22542671/20754108/a493e3ea-b709-11e6-8455-9caa582333d2.png)

**Important files**

The model contains the following files:

+ "Seg.any" Inertia properties of all segments and definitions of surfaces used for wrapping
+ "ClavicleMuscleGeometry.any" nodes for muscle attacments VU sub2
+ "HumerusMuscleGeometry".any" nodes for muscle attacments VU sub2
+ "RadiusMuscleGeometry.any" nodes for muscle attacments MAYO sub2
+ "ScapulaMuscleGeometry.any" nodes for muscle attacments VU sub2
+ "UlnaMuscleGeometry.any"nodes for muscle attacments VU sub2
+ "jnt.any" joint definitions for the shoulder and arm
+ "jnt.nomuscles.any" special joint file for the shoulder and arm to be used when no muscles is used, all joints has reaction forces applied
+ "ArtificialRakeForDeltoidMuscle.any" this the file which contains an artificial segment used for controlling the wrapping of the deltoid muscle
+ "AddOnOutsideBlockForKinematics.any" adding stuff for the scapulo-thoracic gliding plane to the thorax segment
+ "AddOnOutsideBlockForMuscles.any" adding wrapping geometries to segments which are not part of the block
+ "Glove.any" model of a glove which simulates the strength capabilities of the hand
+ "GloveMuscle.any" muscles for the glove here the strength of the glove can be adjusted
+ "muscle.any" muscle definitons for the shoulder and upper arm
+ "muscle-parameters-shoulder.any" muscle strength parameters for the arm
+ "muscle-parameters-shoulder-const.any" muscle strength parameters for the arm
+ "muscle-parameters-shoulder-const_simple.any" muscle strength parameters for the arm
+ "WristMuscle.any" this files adds artificial muscles to the wrist

**Used references**

The raw data from [here](http://homepage.tudelft.nl/g6u61/repository/shoulder/overview.htm) has been converted using a small Matlab program, which automatically transforms the global coordinates into local coordinates on the segments.
+ F.C.T. van der Helm and R. Veenbaas, Modeling the mechanical efect of muscles with large attachment sites: aplication to the shoulder mechanism. Journal of Biomechanics, vol. 24, no. 12, pp. 1151-1163, 1991
+ H.E.J. Veeger, F.C.T. van der Helm, L.H.V. van der Woude, G.M. Pronk and R.H. Rozendal, Inertia and muscle contraction parameters for musculoskeletal modelling of the shoulder mechanism. Journal of Biomechanics, vol. 24, no. 7, pp. 615-629, 1991
+ F.C.T. van der Helm, A finite element musculoskeletal model of the shoulder mechanism. Journal of Biomechanics, vol. 27, no. 5, pp. 551-569, 1994
+ R. Happee and F.C.T. Van der Helm, The control of shoulder muscles during goal directed movements, an inverse dynamic analysisJ. Biomechanics, vol. 28, no. 10, pp. 1179-1191, 1995
+ Van der Helm FC, Veeger HE, Pronk GM, Van der Woude LH, Rozendal RH. Geometry parameters for musculoskeletal modeling of the shoulder system Journal of biomechanics Vol. 25 no. 2, pp. 129-144, 1992 Note: this reference is used for the geometry used for the definition of many of the geometries which are used for muscle wrapping
+ DirkJan (H.E.J.) Veeger, Bing Yu, Kai Nan An, Orientation of axes in the elbow and forearm for biomechanical modeling Proceedings of the first conference of the ISG,1997
+ The segment coordinatesystem are according to the ISB proposal, please see http://internationalshouldergroup.org/files/standards97.pdf
+ H.E.J. Veeger, Bing Yu, Kai-Nan An and R.H. Rozendal, Parameters for modeling the upper extremity, Journal of Biomechanics, Vol. 30, No. 6, pp. 647-652, 1997
+ H.E.J. Veeger, F.C.T. van der Helm, L.H.V. van der Woude, G.M. Pronk and R.H. Rozendal,Inertia and muscle contraction parameters for musculoskeletal modelling of the shoulder mechanism. Journal of Biomechanics, vol. 24, no. 7, pp. 615-629, 1991

**Muscles spanning the wrist has been added the data for these muscles originates from these articles**

+ Jacobson, M. D., R. Raab, B. M. Fazeli, R. A. Abrams, M. J. Botte, and R. L. Lieber. Architectural design of the human intrinsic hand muscles. J. Hand Surg. [Am.] 17:804809, 1992.
+ Lieber, R. L., M. D. Jacobson, B. M. Fazeli, R. A. Abrams, and M. J. Botte. Architecture of selected muscles of the arm and forearm: Anatomy and implications for tendon transfer. J. Hand Surg. [Am.] 17:787-798, 1992.
+ Lieber, R. L., B. M. Fazeli, and M. J. Botte. Architecture of selected wrist flexor and extensor muscles. J. Hand Surg. [Am.] 15:244-250, 1990.
+ Muray, W.M.,T.S. Buchanan, and S.L. Delp. Scaling of peak moment arms with the elbow and forearm position J. Biomech. Vol. 28, pp. 513-525, 1995

**The model consist of the following joints:**
+ SC SternoClavicular: spherical joint
+ AC AcromioClavicular: spherical joint
+ GH Glenohumeral joint: spherical (normal reactions of the spherical joint is not used unstead they are created so that they fall within the glenoid cavity the file, please see the file GHReactions.any for details)
+ AI One dimensional constraint between the scapula and the thorax at the bonylandmark AI om scapula
+ AA One dimensional constraint between the scapula and the thorax at the bonylandmark AA om scapula
+ ConoideumLigament : the lenght of this segment is driven to a constant lenght
+ FE Flexion extension of the elbow: revolute joint
+ PS pronation supination joint or the forearm: combination of joints in distal and proximal end of the radius bone that leaves one dof free which is pronation/supination of the forearm
+ Wrist joint: created as two revolute joints where the axis of rotations are not coincident

## The Lumbar spine Model

The Lumbar spine contains 5 vertebrae with 3 DoF spherical joints in between, 188 muscle fascicles and intra-abdominal pressure.

The functional spinal units (FSU) are driven using a prescribed kinematic rhythm and by default facet joints are not employed due to the fact that most of the application do not focus on the lumbar spine section. However, several examples demonstrate possible mechanisms of facet joint incorporation and detailed modeling of the lumbar spine. The spinal muscles do not include the force-length-velocity relations (i.e. we use the so-called simple muscle model). The only input parameter in the muscle model is the cross-sectional area multiplied by a factor. Daggfeldt and Thorstensson (J.Biomech. 2003, 36: 815-825) didn't include the force-length-velocity relations either. Inclusion of the lumbar spine ligaments is optional and can be done as cumulative stiffness of FSU or as separate elastic elements. Similarly the intervertebral disc (IVD) stiffness can be used as a single cumulative value for a FSU or as linear and nonlinear functions for the disc only. This however is mostly used for the spine specific applications, where the level of detail is important. In other cases it has been shown that the torque production from ligaments might not be very important (Cholewicki and McGill, J.Biomech. 1992, 25: 17-28). The data of vertebrae dimensions and whole body parameters is taken from: Nissan and Gilad (J.Biomech. 19: 753-758, 1986) and mechanical properties of ligaments were taken from: Pintar et al. (J.Biomech. 25(11): 1351-1356, 1992).

The spine model contains a preliminary model of the Intra Abdominal pressure (IAP). In short the IAP is modeled as constant volume, which, when squeezed from the side by the transversus muscles extends the spine by pushing on the rib thorax and the pelvic floor. From the mathematical point-of-view, this lets the abdominal muscles function as spine extensors, and they become part of the whole recruitment problem. The limit of the IAP was set to 26600 Pa, which was based on measurements on well-trained subjects (Essendrop, M., 2003. Significance of intra-abdominal pressure in work related trunk-loading. Ph.D. Thesis, National Institute of Occupational Health, Denmark.) and using geometric/anatomic estimates of pressure surface area and area centroids, which in turn determines the effective moment arm of the pressure.

![image](https://cloud.githubusercontent.com/assets/22542671/20754241/531e53c8-b70a-11e6-9881-97353310dcbb.png)

You can read more about this lumbar spine model and some prelimary validation in the following article:

+ de Zee, M., L. Hansen, C. Wong, J. Rasmussen, and E.B. Simonsen. A generic detailed rigid-body lumbar spine model. J.Biomech. 40: 1219-1227, 2007.

Data of vertebrae dimensions and whole body parameters is taken from:

+ Nissan and Gilad (J.Biomech. 19: 753-758, 1986)

Some important facts about this model

+ The model does not include facet joints.
+ The muscles do not include the force-length-velocity relations (i.e. we use the socalled simple muscle model). The only input parameter in the muscle model is the cross-sectional area multiplied by a factor. Daggfeldt and Thorstensson (J.Biomech. 2003, 36: 815-825) didn't include the force-length-velocity relations either.
+ Ligaments are not included in this model, because we do not have the necessary mechanical properties. This is however not a problem, because it has been shown that the torque production from ligaments might not be very important (Cholewicki and McGill, J.Biomech. 1992, 25: 17-28).

The model contains a preliminary model of the Intra Abdominal pressure (IAP). In short the IAP is modelled as constant volume, which, when squeezed from the side by the transversus muscles extends the spine by pushing on the rib thorax and the pelvic floor. From the mathematical point-ofview,this lets the abdominal muscles function as spine extensors, and they become part of the whole recruitment problem. The limit of the IAP was set to 26600 Pa, which was based on measurements on well-trained subjects (Essendrop, M., 2003. Significance of intra-abdominal pressure in work related trunk-loading. Ph.D. Thesis, National Institute of Occupational Health, Denmark.) and using geometric/anatomic estimates of pressure surface area and area centroids, which in turn determines the effective moment arm of the pressure.

For more details please see this presentation [Abdominal pressure Presentation](http://wiki.anyscript.org/images/7/7e/AbdominalPressureModel.pdf).

**The following files are important:**

+ "Buckle.any" this is the main file for simulating the abdominal pressure
+ "SegmentsLumbar.any" this the segments for the lumbar part of the spine
+ "SegmentsThorax.any" this is the segment for thoracic part of the spine
+ "JointsLumbar.any" this file contains the joint definitions
+ "MuscleSpineLeft.any" this is the file which collects the muscles for the left side of the spine
+ "MuscleSpineRight.any" this is the file which collects the muscles for the right side of the spine
+ "MuscleParametersSpineSimpleLeft.any" the muscle strength parameters for the left side of the spine
+ "MuscleParametersSpineSimpleRight.any" the muscle strength parameters for the right side of the spine

