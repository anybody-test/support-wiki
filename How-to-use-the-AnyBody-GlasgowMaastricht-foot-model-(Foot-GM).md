## What Models are available?

In the AMMR v1.6 you can find under beta two models that are using the detailed foot model: the AnyBody- Glasgowmaastricht Foot Model. FreePostureFootGM A model that is based on the FreePosture models that includes the detailed foot model. It has all 26 separate segments of the foot and all joints. It can be driven by joint angles in the mannequin file.

MoCapFootGM model A model that is based on a typical MoCap model. It uses motion capture (C3D) and pressure data to drive the model. It has all 26 separate segments of the foot, joints, ligaments and muscles.

## What input is needed for the MoCap-FootGM model?

In order to run the MoCap-FootGM model with new data you need following data: - Input: Motion capture data including force plate data in form of a C3D file (NameofFile.c3d). - Input: Pressure Data from a pressure plate (exported from RSScan: NameofFile-PressureData.txt) - TrialSpecificData: Height, Weight of subject, initial guess on segment lengths - Scaling: picked points from Surface scan of the subjects foot

## Additional files in the MoCap-FootGM model!

**DorsiFlexionMedial & DorsiFlexionLateral**

These are some sort of rhythms and can be used for any subject.

**BareFootWalk-CoeffVec**
The first time you ran a subject you have to select "First_Run" to read the CeoffVec out of the input data. This is very time consuming, so you can write that data into the CoeffVec text file. For any additional run, you can switch off the "First_Run" and use this text file instead of reading out the data of the input.

## How to make a picked_point file!

The picked point file includes the coordinates of following points:

```
"heel back"
"heel plantar"
"heel medial"
"heel lateral"
"achilles tendon"
"melleolus medial"
"malleolus lateral"
"navicular tuberosity"
"navicular dorsal"
"midfoot lateral"
"first meta head lateral"
"metatarsal dorsal"
"metatarsal plantar"
"big toe tip"
"second toe tip"
"fift toe tip"
```

You can import a surface scan in meshlab and identify the points in there with the picked points tool. The points can be identified as shown in this [video](http://www.youtube.com/watch?v=BmB-88o8uYY&feature=youtu.be):

## The model utilize python, which version should be used?

Please use Python (64bit) if you use AMS (64 bit) and similarly for 32bit, if the versions does not match each other it may result in model load errors we recommend using Anaconda.