# Multi-Robot Coverage Planning

## Software stack for Cooperative Multi-Robot Coverage for Cleaning Robots.

[![ros-ci](https://github.com/nocoinman/MRCP/actions/workflows/ros-ci.yaml/badge.svg)](https://github.com/nocoinman/MRCP/actions/workflows/ros-ci.yaml)
[![lint](https://github.com/nocoinman/MRCP/actions/workflows/lint.yaml/badge.svg)](https://github.com/nocoinman/MRCP/actions/workflows/lint.yaml)

Developed and Tested on **ROS Noetic + Ubuntu 20.04 + Gazebo 11**

## Features![](./media/img/pin.svg)

* Solution for autonomous Multi-Robot SLAM using Frontier based exploration
* Optimizer for finding optimal number of agents required to cover an occupancy grid
* Coverage path planner based on Boustrophedon Cellular Decomposition
* PID path tracking

## Build![](./media/img/pin.svg)

### Clone the project

```bash
git clone https://github.com/nocoinman/Multi-Robot-Coverage-Planning.git
cd MRCP
```

### Resolve dependencies using [`rosdep`](http://wiki.ros.org/rosdep)

```bash
rosdep install -y -i --from-paths ./src
```

### Build workspace

```bash
catkin_make --cmake-args -DCMAKE_BUILD_TYPE=Release
```
#### Before Run the code you should make sure that the GUI parameter in the multi_robot.launch value="true"
#### You can change the number of agents by uncomment the return 1 and comment the return n_agent in the optimizer.py file   def solve
### Run the tests to make sure everything is setup correctly (optional)

```bash
catkin_make run_tests && catkin_test_results build/test_results
```

## Usage![](./media/img/pin.svg)

### Shell 1

```bash
source devel/setup.bash
roslaunch simulation multi_robot.launch map:=map1
```
> Available maps: map1, map2, map3, map4

### Shell 2

```bash
source devel/setup.bash
roslaunch full_coverage_path_planner cover_map.launch map:=map1
```

### Unpause physics to start simulation

Gazebo by default starts in headless mode, with physics paused. **You'll have to unpause physics to start the simulation.**
You can do this in 2 ways.

For the shell geeks out there, you can unpause physics from the shell using `rosservice`

```bash
rosservice call /gazebo/unpause_physics
```

For the GUI fellas, we have conveniently added a custom panel to RViz to `pause` and `unpause` gazebo physics. Thank us later!

## Workflow![](./media/img/pin.svg)

Intended use of the software is as shown.

![](./media/img/workflow.png)

### Coverage Path Planning

Coverage path is computed using Boustrophedon Cellular Decomposition. The plan is then divided into equal sub-plans and 
assigned to each agent.

Here is a demo of the path planning algorithm in action, ran on the occupancy grid generated using Multi-Robot SLAM on a
complex office setting.

|![](./media/img/World-Office.jpg) | ![](./media/img/Coverage-Plan-Office.png) |
|:--------------------------------:|:-----------------------------------------:|

### Path Tracking

Agents follow the designated path asynchronously. This is accomplished using simple PID control based path tracking.

|![](./media/map1.gif) | ![](./media/map2.gif) |
|:--------------------:|:---------------------:|
|![](./media/map3.gif) | ![](./media/map4.gif) |

![](./media/img/efficiency.png)
