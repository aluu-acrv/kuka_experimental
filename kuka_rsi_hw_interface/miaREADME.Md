# KUKA Experimental + UAP KUKA + KUKA RSI

written by Amelia Luu, 12/09/19.

This goes through how to use the KUKA Experimental ROS package with RSI and UAP's KUKA KR90.

## Configuration Files
location: `/kuka_experimental/kuka_rsi_hw_interface/config`

make sure the number of joint names and controllers align with `kuka_hardware_interface.cpp` script's constructor. right now, it is treated as a 7dof robot to include the turntable. unsure if this is correct practice. 

`test_hardware_interface.launch` file loads rsi parameters `/test/test_params.yaml` and `/test/test_params_sim.yaml`. also loads the robot description (urdf) file, currently in `kuka_kr90uap_support` package.

runs the interface node, loads the joint and controllers configuration files also controller spawner and robot state publisher. 

lastly, rsi simulator is launched using ip `127.0.0.1` and port number `49152`. these values need to match what's in `/test/test_params_sim.yaml`.

## Simulation

in RVIZ show TF frames and robot model. 
```
rviz rviz
```

```
roslaunch kuka_rsi_hw_interface test_hardware_interface.launch sim:=true marvin:=false
```

to manipulate the joints using rqt gui
```
rosrun rqt_joint_trajectory_controller rqt_joint_trajectory_controller
```

## On KUKA KR90 3DP ROBOT
***TESTING ON ROBOT NEEDS TO BE VALIDATED!!!!***

the ros_rsi_ethernet.xml file needs to have the IP address of the pc with ROS.
the test_params.yaml file needs to have the IP address of the KUKA robot 

```
roslaunch kuka_rsi_hw_interface test_hardware_interface.launch sim:=false marvin:=false
```
the `/test/test_params.yaml` file needs to match 

`ros_rsi.src` file moves the robot to a starting position (see lines 68-69). **make sure this is valid.**

on KUKA teach pad will show **Programmed path reached (BCO)**
Continue.
Teach pad will show a warning **!!! Attention - sensor correction goes active !!!**
If this is successful, the hardware interface will show **Got connection from robot** in terminal.

to manipulate the joints using rqt gui
```
rosrun rqt_joint_trajectory_controller rqt_joint_trajectory_controller
```

### todo:
- hook up moveit with the rsi connection
- test hardware interface with actual robot 
- check if the turn table's 7dof system works or not
  - otherwise how do you do link the external kinematic up properly?
- check out rsi3 communication method