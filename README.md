# ROS2 and ROS1 Bridging
For this tutorial, you will need to use Ubuntu 20.04.
First, install both ROS1 Noetic and ROS2 Foxy distributions on the same operating system. You can find the installation of related distributions from below links:

[ROS1 Noetic](http://wiki.ros.org/noetic/Installation)

[ROS2 Foxy](https://docs.ros.org/en/foxy/Installation/Alternatives/Ubuntu-Development-Setup.html)

After installing both of them you should be able to see both distributions under ~/opt/ros

```bash
cd ~/opt/ros
ls
```

It should show both of them
```bash
foxy  noetic
```
it is important to make sure these ros versions are not automatically sourced in .bashrc file. Check the .bashrc file:

```bash
gedit ~/.bashrc
```
Uncomment if there is any sourcing lines existing in .bashrc (sometimes its written automatically when you install a distribution)
```bash
#source /opt/ros/foxy/setup.bash
#source /opt/ros/noetic/setup.bash
```
It is also easier to create an alias for sourcing the distributions so you can call them easier in the terminals later. Write the following lines inside .bashrc
```bash
alias foxy='source /opt/ros/foxy/setup.bash'
alias noetic='source /opt/ros/noetic/setup.bash'
```

Open up a new terminal, divide it into two screens and make a distinction between ros1 and ros2 terminals easier. Write 'noetic' (if you made the alias) or source /opt/ros/noetic/setup.bash to the terminals you want for ROS1 and 'foxy' for ROS2. The terminals might look like this

![Brdige](https://github.com/kenanalperen/ros1-2/blob/main/bridge.jpg)

In one of the ROS1 terminals, write roscore so ros1 can work
```bash
noetic
roscore
```
In another ROS1 terminal, you can play a rosbag or echo your topic using something similar to below

```bash
noetic
rosbag play Downloads/franka_topopics_2024-04-13-08-44-40.bag 
```
In another ROS1 Terminal, first source Noetic, then on top of this source Foxy (you will have a warning similar to "ROS_DISTRO was set to 'noetic' before. Please make sure that the environment does not mix paths from different distributions.")

```bash
noetic
foxy
ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

Now you should have a bridge connecting ROS1 to ROS2 and you can see the topics from ROS1 in ROS terminals as well. Try looking at the topics inside ROS2 to se iif you can see them.

```bash
foxy
ros2 topic list
```
You can get more inside about the topic and message type by typing 

```bash
foxy
ros2 topic info /franka_a/cartesian_impedance_controller/equilibrium_pose
ros2 interface show geometry_msgs/msg/PoseStamped
```
You can also echo the topic coming from ROS1 inside ROS2 by 
```bash
foxy
ros2 topic echo /franka_a/cartesian_impedance_controller/equilibrium_pose
```

