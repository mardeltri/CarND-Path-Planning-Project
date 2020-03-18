# **Path planning** 

[//]: # (Image References)

[image1]: ./images/state-machine.png "State machine"
[image2]: ./images/Simulator1.png "Simulator"
[image3]: ./images/Simulator2.png "Simulator"
[image4]: ./images/Simulator3.png "Simulator"


### 1. Introduction
The goal of this project is to build a path planner that creates smooth, safe trajectories for the car to follow. The highway track has other vehicles, all going different speeds, but approximately obeying the 50 MPH speed limit.

The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.

First, the state machine which has been implemented will be discussed. The path generation algorithm will be explained in the second section. Finally, the controller implemented to follow other cars is addressed in the last section.

![Simulator][image2]

### 2. State Machine
Three states have been implemented in the state machine:
* Keep lane
* Prepare to change of lane
* Change of lane

Keep lane is set when there are not cars close to our car. If a car is found in front of us, then there is a change of state to Prepare to change of lane. In this situation, the car waits until there is a slot in the other two lanes. When there is a free lane, it changes to Change of lane state and it stays there for 3 seconds. This time restriction have been observed to improved the behavior when there are two cars in front of us and they are at the same distance. 

![State machine][image1]

#### 1. Optimized lane changing
In order to optimize lane changing, a specif algorithm has been implemented. The optimization is carried out considering the distance between our car and the others, always changing to the lane wich has more free space available.

### 2. Path generation
The path consists of a spline which has been generated with [this library](https://kluge.in-chemnitz.de/opensource/spline/). In order to compute the sample points, first the original and desired final position have been specified. Later, several sample points have been computed considering the desired speed. It has been taken into account that the sample time of the simulator is 0.02 seconds. It should be noted that the car drives from point to point each sample step. Thus, the distance between the points defines the speed of the car. In addition, the two reference of frames have been considered, in effect, the spline has been computed in Frenet frame and it has been transformed to cartesian coordinates.

### 3. Controller
It worths mention that a PID controller has been design in order to keep a specific distance with the car in front of us. In order to do so, reference velocity has been splited in two components: pursued car velocity and control effort. The pursued car velocity is obtained from the sensor fusion data while the control effort is computed with the PID controller. Saturations, maximum velocity and maximum aceleration have been considered in the controller output.

![Simulator][image3]
![Simulator][image4]