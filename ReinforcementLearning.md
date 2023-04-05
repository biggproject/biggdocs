[Return to home](README.md)

# Reinforcement learning (UC15)

We build reinforcement learning based demand response for managing gas consumption in domestic households. We will use
the model developed in flexibility identification to define a RL agent that will learn to optimize the gas consumption
of the household. For detailed description of the reinforcement learning model and the control strategy used for flexibility identification and reinforcement
learning agent, please visit:

- [Reinforcement Learning Training](ReinforcementLearning/ReinforcementLearningTraining.md): Defines the reinforcement learning agent and the DR
  objective for gas consumption optimization.  
- [Boiler controller](ReinforcementLearning/ActionCoordinator.md): Description of the action coordinator, that uses the above defined Reinforcement Learning agent to control boilers during a DR-event.


