[Return to home](../README.md)

This page provides the overview of the functions implemented for thermal model. For details about the thermal model and flexibility identification please visit [Flexibility Identification](../ReinforcementLearning/FlexibilityIdentification.md).

# :card_file_box: Thermal Model / Dynamics

### Details:
Here we implement the functions for modelling the dynamics of thermal model. These are the space heating, boiler and gas consumption models detailed in [Flexibility Identification](../ReinforcementLearning/FlexibilityIdentification.md).

## :round_pushpin: [RoomT_next](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/dynamics.py#L41)

### Description:

This is the calculation for room temperature for next time step. This is based on the space heating model.

### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BuildingT_: <code>float</code> or <code>array</code>. Building temperature for the timestep T
* _BoilerInletT_: <code>float</code> or <code>array</code>. Boiler Inlet temperature at time T
* _BoilerOutletT_next_: <code>float</code> or <code>array</code>. Boiler temperature at time T+dt
* _AmbientT_: <code>float</code> or <code>array</code>. outside ambient temperature at time T
* _dt_: <code>float</code> or <code>array</code>. Time difference between consecutive timesteps
* _Ci_: <code>float</code>. Capacitance of room (Represents the heat storage capacity of room)
* _Ri_: <code>float</code>. Resistance between room and boiler inlet 
* _Ro_: <code>float</code>. Resistance between room and boiler outlet
* _Ra_: <code>float</code>. Resistance between room and outside air
* _Rb_: <code>float</code>. Resistance between room and building thermal mass
* _Irradiance_: <code>float</code>. (Optional) Solar Irradiance at time T

### Return values: 
* _RoomT_next_: <code>float</code> or <code>array</code>. Room temperature at timeslot T + dt


## :round_pushpin: [BuildingT_next](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/dynamics.py#L17)

### Description:

This is the calculation for building temperature (temperature of the thermal mass of the building) for next time step. This is based on the space heating model.
### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BuildingT_: <code>float</code> or <code>array</code>. Building temperature for timestep T
* _dt_: <code>float</code> or <code>array</code>. Time difference between consecutive timesteps
* _Cb_: <code>float</code>. Capacitance of the building thermal mass. (This represents the storage capacity of building)
* _Rb_: <code>float</code>. Resistance between room and building thermal mass

### Return values: 
* _BuildingT_next_: <code>float</code> or <code>array</code>. Building temperature at timeslot T + dt


## :round_pushpin: [BoilerInletT_next](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/dynamics.py#L29)

### Description:

This is the calculation for boiler inlet temperature for next time step. This is based on the space heating model.
### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BoilerInletT_: <acode>float</code> or <code>array</code>. Boiler Inlet temperature at time T
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _Ci_: <code>float</code>. Capacitance of boiler (Represents the storage capacity of boiler)
* _Ri_: <code>float</code>. Resistance between room and boiler

### Return values: 
* _BoilerInletT_next_: <code>float</code> or <code>array</code>. Boiler outlet temperature at timeslot T + dt


## :round_pushpin: [BoilerOutletT_next](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/dynamics.py#L68)

### Description:

This is the calculation for boiler outlet temperature for next time step. This is based on a decay/growth model for the boiler temperature, where a1 and a2 are the variables that control the rate of change of boiler temperature. 

### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BoilerSet_next_: <code>float</code> or <code>array</code>. Boiler setpoint for the timestep T+dt
* _BoilerOutletT_: <code>float</code> or <code>array</code>. Boilter outlet temperature at time T
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _a1_: <code>float</code>. parameter a1
* _a2_: <code>float</code>. parameter a2

### Return values: 
* _BoilerOutletT_next_: <code>float</code> or <code>array</code>. Boiler temperature at the next timestep.


## :round_pushpin: [Gas_modulation](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/dynamics.py#L83)

### Description:
This is the calculation for the Gas modulation at the next time step. This is based on the boiler model. b1 and b2 are parameters  used for the internal model of the boiler.

### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _RoomSet_next_: <code>float</code> or <code>array</code>. Set point for room temperature at time T + dt
* _BoilerOutletT_: <code>float</code> or <code>array</code>. Boiler outlet temperature at time T
* _BoilerOutletT_next_: <code>float</code> or <code>array</code>. Boiler outlet temperature at time T+dt
* _BoilerSet_next_: <code>float</code> or <code>array</code>. Boiler set point for the time T+dt
* _BoilerInletT_next_: <code>float</code> or <code>array</code>. Boiler inlet temperature at time T+dt
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _mdot_: <code>float</code>. mass flow rate of water
* _cg_: <code>float</code>. specific heat capacity of water
* _b1_: <code>float</code>. parameter b1
* _b2_: <code>float</code>. parameter b2

### Return values: 
* _Gas_modulation_: <code>float</code> or <code>array</code>. Gas modulation at the next timestep. This is directly related to consumption.


















# :card_file_box: Thermal Model / PhysicsCell

### Details:
This is **PhyCell** class. An object of this class is used as a main recurrent unit in the thermal model. A <code>forward()</code> method is implemented in the class, which takes the current hidden state **z**<sub>t</sub> and current inputs **x**<sub>t</sub>, and returns the next hidden state and output (**x**<sub>t+1</sub>, **z**<sub>t+1</sub>). The class also has <code>set_param()</code> and <code>set_param_grad()</code> methods to set the values of parameters of **PhyCell** and if they should be optimized. A <code>weight_loss()</code> methods returns the value of loss calculated for weights of the cell

Required parameters to be initialised:

R<sub>a</sub>,R<sub>b</sub>, R<sub>i</sub>, R<sub>o</sub>, C<sub>r</sub>, C<sub>i</sub>, C<sub>b</sub>, a<sub>1</sub>, a<sub>2</sub>, b<sub>1</sub>, b<sub>2</sub>, c<sub>g</sub>, m

## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20)

### Description:

Class **PhyCell** for space heating. 

### Input arguments:
* _paras_json_loc_: <code>str</code>. location of the *json* file containing the initial values of the parameters. A json file with default initial parameters, if supplied at the default file location. A proper *json* dictionary should have all the required parameters, i.e., all of  R, C, a and b. 

### Return values: 
* _self_: <code>object</code> or <code>array</code>. An object of class **PhyCell**

## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20). [forward()](https://github.com/biggproject/biggpy/blob/89d7c63800f4083a773f7eabd89aa2275113e75c/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L29)

### Description:

forward method for the **PhyCell** class.

### Input arguments:
* _inputs_: <code>tensor</code>. Inputs for the cell at time step t. dimensions: [Batch_size, x_features]
* _state_: <code>tensor</code>. Past state of the cell. dimensions: [Batch_size, z_features]
    
### Return values: 
* <code>tuple</code> a tuple containing the next output and state of the PhyCell

## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20). [set_param()](https://github.com/biggproject/biggpy/blob/89d7c63800f4083a773f7eabd89aa2275113e75c/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L53)

### Description:

forward method for the **PhyCell** class.

### Input arguments:
* _**kwargs_: <code>float</code> or <code>dict</code>. set the parameters of the **PhyCell**. Single parameter can be passed as a key value pair, for example Ri = 10.0, to set the value of Ri to 10.0. A dictionary containing all the parameters can also be passed.

### Return values: 
* nothing is returned

## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20). [get_param()](https://github.com/biggproject/biggpy/blob/89d7c63800f4083a773f7eabd89aa2275113e75c/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L46)

### Description:

Returns the dictionary of the parameters of the current instance of the **PhyCell** object.

### Return values: 
* _dict_: dictionary containing the parameters of the object


## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20). [set_param_grad()](https://github.com/biggproject/biggpy/blob/89d7c63800f4083a773f7eabd89aa2275113e75c/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L65)

### Description:

The forward method for the **PhyCell** class.

### Input arguments:
* _**kwargs_: <code>bool</code> or <code>dict</code>. indicate if the gradiant should be calculated for parameters of the **PhyCell**. A parameter will not be optimized if the gradiant for that parameter is set to False.


## :round_pushpin: [PhyCell](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L20). [param_loss()](https://github.com/biggproject/biggpy/blob/89d7c63800f4083a773f7eabd89aa2275113e75c/ai_toolbox/src/ai_toolbox/RL/ThermalModel/phycell.py#L38)

### Description:

loss for the parametrs of the **PhyCell**. This loss is calculated using the weight loss function defined in the training section of [Flexibility Identification](../ReinforcementLearning/FlexibilityIdentification.md).

### Return values: 
* _loss_: loss for the current parameters of **PhyCell** object instance.




# :card_file_box: Thermal Model / DenseNet

### Details:
This is used to create a dense neural network that to estimate the mapping of unknown disturbances (Outside temperature in our case).


## :round_pushpin: [DenseNet](https://github.com/biggproject/biggpy/blob/5e2d617ffdd256cefa56f156bab8f8f548716822/ai_toolbox/src/ai_toolbox/RL/ThermalModel/denseNet.py#L16)

### Description:

A function to create a DenseNet neural network with given layers.

### Input arguments:
* _layers_: <code>list</code>. a list of number of nodes in the layers of the network. for example, layers = [1,10,1] would create a neural network of 1 input node, 1 hidden layer 10 nodes, and 1 output node.
* _bias_: <code>bool</code> (optional). Parameter indicating whether a bias term should be included in the weights.

### Return values: 
* _DenaseNet_: <code>list</code> a list containing the layers of the network. this list can be based on the type of neural network library being used.
