+ [Is there any post processing of Muscle force Fm is needed such as Root Mean Square, Average or any other (Kindly suggest any) to interpret or comparison between different conditions](#is-there-any-post-processing-of-muscle-force-fm-is-needed-such-as-root-mean-square-average-or-any-other-kindly-suggest-any-to-interpret-or-comparison-between-different-conditions)
+ [What variable of the muscle in our simulation results is equivalent to the EMG data at real time, Fm, Ft, Muscle activity, average activity or any ?](#what-variable-of-the-muscle-in-our-simulation-results-is-equivalent-to-the-emg-data-at-real-time-fm-ft-muscle-activity-average-activity-or-any-)
+ [Does Overload Muscle Configuration causes error in the analysis?](#does-overload-muscle-configuration-causes-error-in-the-analysis)
+ [Is it reasonable to observe that agonist and antagonist muscles are active at the same time?](#is-it-reasonable-to-observe-that-agonist-and-antagonist-muscles-are-active-at-the-same-time)
+ [How is physiological coactivation predicted by the objective functions in the muscle recruitment problem?](#how-is-physiological-coactivation-predicted-by-the-objective-functions-in-the-muscle-recruitment-problem)
+ [How to make use of the muscle normalization factor available in the AnyBodyStudy](#how-to-make-use-of-the-muscle-normalization-factor-available-in-the-anybodystudy)


## Is there any post processing of Muscle force Fm is needed such as Root Mean Square, Average or any other (Kindly suggest any) to interpret or comparison between different conditions

There is no statistical post-processing included in the models. However, Anybody is a Modeling System, you can create all kind of functions in it. If the value is available at the time, you can create all sort of statistical or mthematical computations. In your ModelView you see a tab with "Globals" and "Functions" to you left, please have a look in there. On the other hand, in "Classes" you can easily make an AnyOutputFile and output the wanted values and do postprocessing in MatLab, SPSS, etc...

## What variable of the muscle in our simulation results is equivalent to the EMG data at real time, Fm, Ft, Muscle activity, average activity or any ?

Muscle activity can be compared to EMG (and has been done in several occasions) please have look here:

https://github.com/AnyBody/support/wiki/Validation-examples

http://wiki.anyscript.org/index.php/...%26_Validation

## Does Overload Muscle Configuration causes error in the analysis?

Muscle activations can be over 1 = 100% of it's maximum, you will get a warning "overload muscle configuration"

## Is it reasonable to observe that agonist and antagonist muscles are active at the same time?

It depends on the function of the muscles. If the muscles are uni-articular, recruiting both agonist and antagonist muscles is against effort minimization. Therefore, a muscle recruitment solver will only activate each at a time. However, if an agonist muscle spans two or more joints [or degrees of freedom (dof) to be more accurate], it might be activated at the same time that antagonist muscles (at one of those dof the multi-articular muscle spans) are on, which is due to the equilibrium requirement.

Although sufficiently bio-fidelic models have 3D muscle lines of action that span multiple joints, in a simplified case, if you have a 2D model with uni-articular muscles, there will not be any antagonistic coactivation.

## How is physiological coactivation predicted by the objective functions in the muscle recruitment problem?

The objective functions used in AnyBody are those presented and commonly used in the literature to solve the problem of muscle redundancy. The sum of (Fi/Ni)^P is the polynomial objective function. Ni is a muscle-related parameter; studies have used physiological cross sectional area, muscle maximum isometric force, etc. Ni's are in one sense normalization factors to make different (Fi/Ni)^P comparable to each other. If Ni's are muscle maximum isometric force parameters, then stronger agonist muscles will be recruited more; however, it also depends on the muscle moment arms. In AnyBody, the default Ni is the instantaneous muscle strength. If a simple muscle is used, Ni will be equal to the muscle isometric force parameter F0; however, if a three or two-element Hill muscle models is chosen, Ni will be equal to fce(l)*fce(v)*F0*cos(alpha) where fce(l) and fce(v) are the normalized force-length and force-velocity relationships of the contractile element component and alpha is the pennation angle of the muscle fiber. Therefore, Fi/Ni always models muscle activations.

The commonly used objective functions in the literature are based on minimizing some forms of force, activation, or metabolic energy, which all more or less will lead to minimum antagonistic coactivation. However, if there is an evidence in the experimental data that there should be a certain agonistic or antagonistic coactivation ratio, you can readily implement this using time-variant constraints on the muscle activations or forces. Read more about how to do this by looking up AnyMuscleActivityBound class in the AnyScript Reference Manual.

## How to make use of the muscle normalization factor available in the AnyBodyStudy

The search term InverseDynamics.Criterion.PrimaryTerm.Weight_SearchName allows a normalization factor to be added to the muscle this small example displays one possible use of the feature. [MuscleNormalizationFactorModel.main.any](https://raw.githubusercontent.com/AnyBody/support/master/Wiki_Files/Muscles/MuscleNormalizationFactorModel.main.any)
In the webcast "Grand Challenge Knee: Patient-Specific Musculoskeletal Modelling Of Total Knee Arthroplasty" at around 26 min, there is a explanation on the concept and it's use, please see [video](https://www.youtube.com/watch?v=8U5oWctbI54).
