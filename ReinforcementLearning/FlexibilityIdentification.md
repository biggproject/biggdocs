# Flexibility Identification 

Effective demand response is built on identification and exploitation of flexibility. We make a thermal model of a build that can be used to estimate the maximum gas flexibility in the next steps, which can be utilized by the Reinforcement learning based control algorithm. This flexibility can de defined as:

<img src="../figures/Flexibility/EqnFlexibility.png" alt="nested_cv" width="100"> 

Where the gas consumption is minimized for T steps based on the boiler output set points T<sub>o</sub><sup>s</sup>

# :card_file_box: Heating system
Space heating in domestic households can be achieved using water heating systems that can be used for space heating and/orproviding domestic hot water. 
A typical water heating system consists  of  a  radiator and  a  boiler  which  is  used  to  heat  the water. Heating  water  circulates  inside  radiator  and  delivers  a heat through surface layer. Externally, the radiator is in contact with the air in the room, which is warmed-up. Following figure shows a simplified version of the space heating system. Heated water leaves  the boiler  at  boiler  outlet  temperature  (T<sub>o</sub>)  and  cold water  enters  the  boiler  at boiler  inlet  temperature  (T<sub>i</sub>).  Theuser requests a room temperature (i.e., Room temperature set point (T<sub>r</sub><sup>s</sup>))

<img src="../figures/Flexibility/House.png" alt="nested_cv" width="300"> 

## :round_pushpin: Physical dynamics
### Space Heating - RC Model

<img src="../figures/Flexibility/RCNetwork.png" alt="nested_cv" width="300"> 

Conventionally,  thermal  modelling  the  building  is  done using gray-box models such as an RC network model. This models the thermal system using a network of resistances and capacitance and calculates heat flows across different network elements. We  define  an  extended  RC  model  for  thermal modeling  that  includes  boiler  outlet  and  boiler  inlet  temperatures.  
Figure below  shows  the  RC  network  for  thermal  modelling. This  represents  a  non-linear  system  of  ordinary  differential equations given by Equation below. 
Heat is added into the system by  increasing  the  boiler  outlet  temperature  (T<sub>o</sub>). Resistances and  capacitances  are  represented  by R and C respectively.  The  vector  given  by [T<sub>a</sub>,G,I] represents  the  disturbance vector  comprising of  outside  air  temperature,  solar  radiance and  internal  heat  gains. Parameter α represents  the  coupling between boiler outlet and boiler inlet temperatures.

<img src="../figures/Flexibility/EqnSpaceHeating.png" alt="nested_cv" width="500"> 


### Boiler outlet temperature

We assume that the boiler temperature depends on boiler set point and boiler inlet temperature. A high boiler set will increase the boiler outlet, similarly, a lower room temperature will  make  the  boiler  temperature  decay. We  can  estimate  the boiler outlet in the time step *t* + 1 using Following Equations.

<img src="../figures/Flexibility/EqnBoiler.png" alt="nested_cv" width="400">


### Gas modulation

Gas  modulation  for  by  the boiler  depends  on  the  boiler  configuration. We  calculate  gas consumption  using  the equations below,  where  total  gas  consumption depends  on  three  factors,  
namely,  
(i) G <sub>o</sub>:  the  baseline  consumption  required  to  heat  the  water  from inlet to outlet,  
(ii) G <sub>s</sub>:which  is  the  startup  consumption,  which  represents  boiler startup  cost  
(iii) G <sub>d</sub>:  which  is  the  consumption  decay  while running.  
The  second  and  third  terms  are  adjustments  in  the baseline gas consumption.

<img src="../figures/Flexibility/EqnGas.png" alt="nested_cv" width="200">



## :round_pushpin: Thermal Model

The  thermal  model M of  a  building  has, (i)  a  **PhyCell**, which is a sequential model of the dynamic system, and (ii) a Dense  network  (**NN**),  used  to  estimate  the  external  disturbances (T<sub>a<\sub>)

### PhyCell 

We introduce a physics cell (**PhyCell**) that implements the dynamics of space heating at each time step *t* and that can be optimized sequentially. 
Figure below provides the architecture of the **PhyCell**. The  hidden  parameters  (T<sub>i</sub> and T<sub>b</sub> ) are  carried  forward  to  the  next  step  in  the  sequence.

<img src="../figures/Flexibility/PhyCell.png" alt="nested_cv" width="500">

### Training Algorithm 

We unroll the model in time for K steps, i.e., the sequence length, to estimate output and gas consumption for next step.  
We  provide  a  pseudo  code  to  train  the  weights  of  the thermal model in Algorithm 1 To optimize the parameters of  PhyCell  we  feed  the  data  in  sequences.  Training  data  is divided into B sequences of K length each. Loss is calculated using the output values and back propagated through time to optimize the parameters.

<img src="../figures/Flexibility/Algorithm.png" alt="nested_cv" width="400">

We optimize the PhyCell after passing sequential inputs, and back propagating the accumulated loss in the unrolled sequence. Where lβ is  the  loss  of  the  parameters  (β)  of  PhyCell, that is used to satisfy the constraints.lw is the loss encountered by the neural network in predicting outside  ambient  temperature.  We  calculate  the  loss  for  each time step for the sequence, where at K<sup>th<\sup> step we calculate the error in room temperature and gas consumption. Estimated  room  temperature  and  gas  consumption are added to calculate l<sub>x<\sub>.


<img src="../figures/Flexibility/EqnLoss.png" alt="nested_cv" width="400">