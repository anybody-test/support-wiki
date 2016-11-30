## How to add a rotating camera to record a model

Sometimes you may want to define nice and impressive videos for presentation purposes. The code snippet below defines a camera (CamRot) that is fixed on a segment floating around the model in a circle on a fixed height. It completely circles the model during the simulation. The code comes from the MoCap Model (AMMR 1.6.2) but it can easily be modified for any other model as well. It was developed on the AMS version 6. 0. 2. 4064 (64-bit version). The circle around the model is defined by making use of the AnyKinEqFourierDriver. The equations for velocity are simple math, they are also given in the code comments. Happy shooting.

You can set the background color:

```
    AnyScene scene = {
        BackgroundColor = {1,1,1};
    };
```

## Add the camera with settings:
```
   // Define the camera properties  
       AnyCameraLookAt CamRot = {
        Perspective =On; //enable perspective
        EyePoint = ..CamSeg.r;// fix camera on segment to be driven
        LookAtPoint = {0,0,0.8}; // look at fixed point in space
        UpPoint = {0,0,1}; // define up (z) direction
        FocalDist = 3;// see reference
        FocalHeight = 3*1.1;// see reference
        AnyScene &scene = .scene; // set Background to white
  
     // Define CamRecorder; note that a folder "cam_rot" must exist, for details see reference     
         AnyCamRecorder rec = {
          FileName = ..ImagePath + "cam_rot/cam_rot." + strval(Counter,"%05i") + "." + ..ImageExt;
          pxWidth = ..pxWidth ;
          pxHeight = ..pxHeight;
          Trig = ..Trig;
          ResetTrig = ..ResetTrig;
         };
       };
     };
```

## More settings and necessary variables:
```

   // get the maximum time for calculation of omega ("Freq" in the Fourier Drivers)
   AnyFloat maxTime= max(Main.Studies.InverseDynamicStudy.tArray);
```

```
   // calculate "Freq" in the Fourier Drivers
   AnyFloat omega = 1/maxTime;
```

Following code will create a segment, that will drive the camera around:

```
   //define Segment to be driven in order to fix the camera on it
   AnySeg CamSeg = 
   {
     Mass = 0;
     Jii = {0,0,0};
     r0 = {1,1,1};
     //      AnyDrawRefFrame drw = {};
     //      AnyDrawSeg frwSeg = {};
   };
```

Joint and drivers for the Camera segment:

```
   // kinematic measures between the camera and global reference system
   AnyFolder MeasureCam = {
     AnyKinLinear lin = 
     {
       AnyRefFrame &ref1 = Main.EnvironmentModel.GlobalRef;
       AnyRefFrame &ref2 = ..CamSeg;
     };
     AnyKinRotational rot = 
     {
       Type = RotAxesAngles;
       AnyRefFrame &ref1 = Main.EnvironmentModel.GlobalRef;
       AnyRefFrame &ref2 = ..CamSeg;
     };
   };
   // fix the segments rotations
   AnyKinEqSimpleDriver drvCamRot = 
   {
     DriverPos = {0,0,0};
     DriverVel = {0,0,0};
     AnyKinMeasure &rot = .MeasureCam.rot;
   };
   // camera is on static height of z = 0.8
   AnyKinEqSimpleDriver drvCamz = 
   {
     MeasureOrganizer = {2};
     DriverPos = {0.8};
     DriverVel = {0};
     AnyKinMeasure &lin = .MeasureCam.lin;
   };
   /* 
   define the circle in the form: 
   (1) x' = -w*r*sin(w*t)
   (2) y' = w*r*cos(w*t)
   x' is the derivate of the position x = r*cos(w*t)
   y' is the derivate of the position y = r*sin(w*t)
   w is the pulsatance (for one complete circle around the model, it depends on your maximum simulation time)
   t is the time (included in the Fourier Drivers)
   r is the radius that the camera should circle the model
   */
   // drive the x component of the segment according to (1)
   AnyKinEqFourierDriver drvx = 
   {
     MeasureOrganizer = {0} ;
     Type = Sin;
     Freq = .omega;
     A = { { 0,-0.5*2*pi}};
     B = { { 0,0}};
     AnyKinMeasure &lin = .MeasureCam.lin;
   };
   // drive the y component of the segment according to (2)
   AnyKinEqFourierDriver drvy = 
   {
     MeasureOrganizer = {1} ;
     Type = Cos;
     Freq =.omega;
     A = { { 0,0.5*2*pi}};
     B = { { 0,0}};
     AnyKinMeasure &lin = .MeasureCam.lin;
   };
```

Thanks to Tim Weber from OTH Regensburg for this explanation!