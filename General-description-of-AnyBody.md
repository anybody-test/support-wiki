###General Introduction into the AnyBody Modeling System:


1.	The AnyBody Modeling System is a **MusculoSkeletal Modeling System** for bio-mechanical simulations analyzing reactions within the human body or between the human body and an environment. Environment can be
  * something within (implant, e.g. knee or hip device),
  * something attached to (exoskeleton, e.g. knee brace or space-suit) or
  * something interacting (e.g. automotive seat, wheelchair, ...) with the human body. Everything in a STL format can be imported, used and analyzed in AnyBody.


2.	AnyBody is a modeling system that allows the user to model and analyze just about any musculoskeletal system, be it a human, a bird or a fish. However, the system comes with a **full body model** containing most bones, joints and muscles in the human physiognomy (over 1000 individual muscle fascicles). By default the body model represents the size and weight of an average European male (1.76m, 75kg), but the model can easily **adapted to your own needs**.
  * You can change height and weight and all individual segment lengths in the body.
  * You can make the body model stronger, or weaker. In orthopedics many times weak old women are modeled, while automotive customers very often want obese “Truck Driver” for ingress/egress” simulations. The more you know about your desired model the better.
  * You can even import subject specific bone geometries from CT scans to represent a specific patient for surgical planing or rehabilitation.


3.	AnyBody uses **motion as input**:
  * the motion can be recorded in a Gait Lab. This can be done using Vicon, the most common **motion capture** (MoCap) system, but AnyBody can handle of course MoCap data from many other systems (everything in C3D format).
  * In AnyBody you can also **define your own motion** by defining initial joint angles and joint velocities manually.
  *In many cases, the motion is simply defined by the human body’s connection with the environment, for instance by attaching the feet to the pedals of a bicycle. That means MoCap is not obligatory, however, makes it easier for difficult and complex motions.


4.	AnyBody uses **external forces as Input** to simulate the internal loads in the body, however, AnyBody is also able to predict contact and ground reaction forces using conditional contact.


5.	AnyBody uses the input (motion & ext. forces) and calculates the activations of the individual muscles responsible for this motion. This process is called **Inverse Dynamics**. AnyBody uses for skilled motions (& non-impact activities) in an optimization process, a muscle recruitment solver to solve what muscles (each single fascicle) will be used. That means you get **muscle forces, activations and joint reaction forces & moments** during this motion. Additionally, each model can be setup to obtain forces and moments in any point of interest, or loading scenario can be exported and further be analyzed with Finite Element Analyzes. 


6.	**EMG is not necessary** for our simulations, however, can be used as a validation of the simulation. There are several scientific papers in literature making these comparisons.


7.	**Application examples**:
  * **In orthopedics** you can develop new implant design simulating accurate reaction forces to minimize loads on joints and implants.
  * **In automotive ergonomics** you can optimize the package design as seat design, pedal steering wheel and handle positions.
  * **In ergonomics and work procedures** you can optimize work places to minimize loads on joints or high muscle activations in order to prevent fatigue.
  * **In the consumer product development** the final product design can be analyzed and optimized.
  * **In sports and high performance** new designs of sport equipments can be analyzed or developed.
  * **In Aeronautics or Aerospace** countermeasure devices for use in space can be developed and optimized to maximize activations on certain muscles to train them in the most efficient way.