# What's the difference between composition, dependency and aggregation
- Dependency: The Address object comes from outside, it's allocated somewhere else. This means that the Address and Employee objects exists separately, and only depend on each other.
- Composition: Here you see that a new Engine is created inside Car. The Engine object is part of the Car. This means that a Car is composed of an Engine.
- Aggregation : Here, the child object can exist outside the parent object. A Car has a Driver. The Driver CAN Exist outside the car.
