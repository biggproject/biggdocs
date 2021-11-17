[Return to home](README.md)

### Implementations of the functions
- [biggpy](https://github.com/BeeGroup-cimne/biggpy#readme)
- [biggr](https://github.com/BeeGroup-cimne/biggr#readme)

# Reinforcement learning 

We build reinforcement learning based demand response for managing gas consumption in domestic households. We will use the model developed in flexibility identification to define a RL agent that will learn to optimize the gas consumption of the household. For detailed description of the thermal model used for flexibility identification and reinforcement learning agent, please visit:
- [Flexibility Identification](ReinforcementLearning/FlexibilityIdentification.md): Description of the thermal model of the building that can be used to estimate future gas flexibility. This is a sequential model to estimate the next time steps room temperature and gas consumption given the boiler and room temperate set points. 
  Functions implemented for thermal model are detailed in [Thermal Model](ReinforcementLearning/ThermalModel.md).  
- [Demand Response](ReinforcementLearning/DemandResponse.md): Defines the reinforcement learning agent and the DR objective for gas consumption optimization. The thermal model developed above can proxy for the virtual environment used to train the RL agent.
    Functions implemented for RL agent are detailed in [RL Classes](ReinforcementLearning/RLClasses.md). 

