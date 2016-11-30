The three element muscle model is sensitive to tendon slack length variable (Lt0). Therefore, tendon slack length must be calibrated to ensure that the optimal muscle fiber length occurs at the desired/specified joint angle.

The problem, however, is that the calibration procedure can take a while to run. This presents an overhead when running many multiple models with the same scaling. The solution is to save the result of the calibration procedure and just load those results for later.

This small example model: [Cached_muscle_calibration.zip](https://github.com/AnyBody/support/blob/master/Wiki_Files/Caching_The_Result_Of_Muscle_Calibration/Cached_muscle_calibration.zip?raw=true) shows how to save the result of a calibration procedure so it can be reused in other models.

![image](https://cloud.githubusercontent.com/assets/22542671/20753893/c29f59b0-b708-11e6-8a64-efc1e486c5cd.png)
