## Musculoskeletal analyse

LIOAnyShoulder is an inverse dynamic musculoskeletal model of the shoulder joint. Such analysis allows computation of muscular and joint reaction forces based on a given motion. This study takes part of non-conforming total shoulder arthroplasty. The prosthesis component implantation and position should require two aspects:

+ Integration, *.stl format, of total shoulder arthroplasty 3D model.
+ Knowledge of matrix transformation between coordinate bone and implant systems.

Any methods could be perform to achieve these requirements. This tutorial will describe the basics concepts to use AnybodyTM software and start working on the musculoskeletal model of the shoulder joint.

# Musculoskeletal analyse

## 1. Basic concepts to use Anybody<sup>TM</sup> software

Anybody software user interface is composed of different parts.

![image](https://cloud.githubusercontent.com/assets/22542671/20788134/b570d4b0-b7af-11e6-8131-40268731a2d7.png)

**User interface windows of AnyBody software

| Legend        | Window name    | Description                                                                         |
| :-----------: |:--------------:| :----------------------------------------------------------------------------------:|
| A             | Editor Window  | Edition file windows *.any will contain the AnyScript<sup>TM</sup> text.            |
| B             | Model View     | Graphic visualisation model window. Access: Window > Model View (new)               |
| C             | AnyChart       | Graphic results display. Access: Window > Chart 2D/3D (new)                         |
| D             | Main Tree Widow| Model and operations display: hierarchical representations of the model defined in A|
| E             | Log window     | Information about calculation in progress, alerts and errors.                       |
| F             | Progress Window| Information about processing analyse.                                               |