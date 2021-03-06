:warning: This package depends on the *Pilz PSIR* robot which will be released soon! <br/>
-> Currently step `rosdep install ..` will fail

## Installation

```shell

# Install Ensenso SDK from!
# https://www.ensenso.com/support/sdk-download

# Create a new ROS workspace
mkdir -p ~/snp_demo_ws/src && cd ~/snp_demo_ws/src

# Download demo repository
git clone https://github.com/rosin-project/automatica18_scan_and_plan_demo.git

# Download dependencies
wstool init .
wstool merge ~/snp_demo_ws/src/automatica18_scan_and_plan_demo/snp_demo_deps.rosinstall
wstool up

# Reset ROS_PACKAGE_PATH
source /opt/ros/kinetic/setup.bash

# Install dependencies 
rosdep update && rosdep install --from-paths ~/snp_demo_ws/src --ignore-src

# Build workspace
cd ~/snp_demo_ws && catkin build 

# Source workspace
source ~/snp_demo_ws/devel/setup.bash

```


## Run application

```shell
# Simulation:
roslaunch snp_psir_bringup application_bringup.launch
# Info: Make sure to press play in Gazebo

# Real robot:
roslaunch snp_psir_bringup application_bringup.launch sim_robot:=false

```

## Systems settings

### CAN interface
Add in `/etc/network/interfaces` the following config for the [socketcan_interface]( http://wiki.ros.org/socketcan_interface).

```
allow-hotplug can0
iface can0 can static
    bitrate 1000000
    up ip link set $IFACE txqueuelen 15
```


### Remove local scan_parameter cache
Make sure to clear possible local `godel_robot_scan_parameters` since they would overwrite the ones of this repo (`snp_psir_bringup/config/robot_scan.yaml`).

```shell
rm -f ~/.ros/godel_robot_scan_parameters.msg
```

### Fix QT bug for Robot Planing Panel (might not be required anymore)
```shell
echo "export QT_NO_FT_CACHE=1" >> ~/.bashrc && source ~/.bashrc
```

