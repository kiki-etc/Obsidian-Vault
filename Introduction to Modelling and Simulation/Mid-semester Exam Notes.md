A simulation is a technique for studying real world dynamic systems by imitating their behavior using a mathematical model of the system implemented by programming.

When to use a simulation:
- To study internals of complex systems
- Optimize an existing design
- When building the system is practically impossible
- To test before working with the real system, if the system is dangerous or destructive
- To study importance of variables
- Verify analytic solutions
- For prediction and sensitivity analysis
- Examining effect of environmental changes

When simulation use should be avoided:
- If simulation cost outweighs the benefits
- If resources are insufficient
- If common sense is enough to get the result
- If an analytic solution is available
- If expert advice on designing a model or interpreting the results is unavailable
# Modeling
A model is a representation of an actual system - physical, abstract or mathematical.
A system, on the other hand, is a set of entities of the real world. The entities' behaviors and relationships are well defined under specific conditions, and define the system and its behavior.
## System State Variable
It is a collection of all the necessary information that defines a system at any given point in time. It depends on the purposes of the investigation and may be eliminated or added during the modeling process.
Defining the system state variables help to keep track of the system state and classify the model of the system state.
### Entities
An entity is an object of the system. They can be dynamic (i.e. it's possible to have multiple instances of the same entity) or static (i.e. there can only be one instance of said entity)
### Attributes
The properties of an entity are called its attributes. Dynamic entities' attributes are local while static entities' are global.
### Resources
A resource is an entity that provides service to dynamic entities. Resources can provide service to more than one dynamic entities. If the resource can provide service to only one dynamic entity at a given time then the resource uses a list to keep track of the entities waiting for service.
#### List Processing
- First in, First out queues
- Last in, First Out queues (Stack)
- Based on properties of an attribute (Priority Queue)
- Shortest processing time scheduling
- Randomly
### Activities & Delays
An activity is a duration of time allocated for an entity's process. ==It should be known prior to the commencement==. The beginning and end of the process can be scheduled.
An activity can be:
- A constant time
- A random value from a statistical distribution
- A result from the model equation
- A result from computation of inputs of the **event** state

A delay is an indefinite duration caused by some combination of system conditions.
### Event
The beginning and end of an activity or delay is an event. The value of the system state variables' attributes change when the event occurs. The change of the system is defined by these changes. The model of the system can be classified based on these changes.
## Model Classification
### Static vs Dynamic
These mutually exclusive categories describe a systems representation with respect to time. Static Simulation Models represent a system at a particular point in time. Dynamic Simulation Models represent a system as it evolves over time.
### Deterministic vs Stochastic
Deterministic models do not contain any probabilistic components, therefore fixed inputs always yield the same outputs. Stochastic models involve probabilistic elements, which creates uncertainty of output.
### Continuous vs Discrete
Discrete models represent systems whose state changes at discrete points of time. For continuous models, the changes of the system occur without break as the time evolves.
## Steps of Modeling
1. Analyze the problem (study the situation and problem, classify it)
2. Formulate a model (gather data and make simplifying assumptions, set up goals)
3. Model abstraction (design the model by analyzing important features)
4. Determine variables and units (determine independent and dependent variable and relationships)
5. Solve the model
6. Model implementation ()
7. Verify and interpret the model solution
8. Execution
9. Output analysis
10. Report on model
11. Recommendation
