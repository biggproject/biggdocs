[Return to home](README.md)

This page provides the overview of the functions implemented for thermal model. For details about the thermal model and flexibility identification please visit [Flexibility Identification](../ReinforcementLearning/FlexibilityIdentification.md).

# :card_file_box: Thermal Model / Dynamics

### Details:
Here we implement the functions for modelling the dynamics of thermal model. These are detailed by the space heating, boiler and gas consumption models detailed in [Flexibility Identification](../ReinforcementLearning/FlexibilityIdentification.md)

## :round_pushpin: RoomT_next

### Description:

This is the calculation for room temperature for next time step. This is based on the space heating model.

### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BuildingT_: <code>float</code> or <code>array</code>. Building temperature for the timestep T
* _BoilerInletT_: <code>float</code> or <code>array</code>. Boiler Inlet temperature at time T
* _BoilerOutletT_next_: <code>float</code> or <code>array</code>. Boiler temperature at time T+dt.
* _AmbientT_: <code>float</code> or <code>array</code>. outside ambient temperature at time T.
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _Ci_: <code>float</code>. Capacitance of room (Represents the heat storage capacity of room)
* _Ri_: <code>float</code>. Resistance between room and boiler inlet 
* _Ro_: <code>float</code>. Resistance between room and boiler outlet
* _Ra_: <code>float</code>. Resistance between room and outside air
* _Rb_: <code>float</code>. Resistance between room and building thermal mass
* _Irradiance_: <code>float</code>. (Optional) Solar Irradiance at time T

### Return values: 
* _RoomT_next_: <code>float</code> or <code>array</code>. Room temperature at timeslot T + dt



## :round_pushpin: BuildingT_next

### Description:

This is the calculation for building temperature (temperature of the thermal mass of the building) for next time step. This is based on the space heating model.
### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BuildingT_: <code>float</code> or <code>array</code>. Building temperature for the timestep T
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _Cb_: <code>float</code>. Capacitance of building thermal mass (Represents the storage capacity of building)
* _Rb_: <code>float</code>. Resistance between room and building thermal mass

### Return values: 
* _BuildingT_next_: <code>float</code> or <code>array</code>. Building temperature at timeslot T + dt


## :round_pushpin: BoilerInletT_next

### Description:

This is the calculation for boiler inlet temperature for next time step. This is based on the space heating model.
### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BoilerInletT_: <code>float</code> or <code>array</code>. Boiler Inlet temperature at time T
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _Ci_: <code>float</code>. Capacitance of boiler (Represents the storage capacity of boiler)
* _Ri_: <code>float</code>. Resistance between room and boiler

### Return values: 
* _BoilerInletT_next_: <code>float</code> or <code>array</code>. Boiler outlet temperature at timeslot T + dt


## :round_pushpin: BoilerOutletT_next

### Description:

This is the calculation for boiler outlet temperature for next time step. This is based on decay/growth model for boiler temperature, where a1 and a2 are the variables that control the rate of change of boiler temperature. 
### Input arguments:
* _RoomT_: <code>float</code> or <code>array</code>. Room temperature at time T
* _BoilerSet_next_: <code>float</code> or <code>array</code>. Boiler setpoint for the timestep T+dt
* _BoilerOutletT_: <code>float</code> or <code>array</code>. Boilter outlet temperature at time T
* _dt_: <code>float</code> or <code>array</code>. Time difference in current and next time
* _a1_: <code>float</code>. parameter a1
* _a2_: <code>float</code>. parameter a2

### Return values: 
* _BoilerOutletT_next_: <code>float</code> or <code>array</code>. Boiler temperature at next timestep.


## :round_pushpin: Gas_consumption

### Description:

This is the calculation for Gas modulation next time step. This is based on boiler model. b1 and b2 are parameters that can be used to model the internal model of the boiler.
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
* _Gas_consumption_: <code>float</code> or <code>array</code>. Gas consumption at next timestep.

