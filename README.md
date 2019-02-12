# SITL + Ardupilot + Gazebo + ROS Camera plugin
SITL + ArduPilot + Gazebo + ROS Camera Plugin (Software In Loop Simulation Interfaces, Models)

## Requirements :
Ubuntu (18.04 LTS) with full 3D graphics.

Gazebo version 8.x or greater (Recomended 9.6)

ROS melodic
SITL Ardupilot
ROS melodic (Required to work with Gazebo)


## ROS melodic installation :

Install ROS with sudo apt install ros-melodic-desktop-full (follow instruction here http://wiki.ros.org/melodic/Installation/Ubuntu).



## SETP 1 - SITL Ardupilot installation :

Instructions taken from ardupilot.org (See original instructions here http://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html)

### Clone ArduPilot repository

````
cd ~/
git clone https://github.com/r0ch1n/ardupilot
cd ardupilot
git submodule update --init --recursive
````

### Install some required packages

If you are on a debian based system (such as Ubuntu or Mint), we provide a script that will do it for you. From ardupilot directory :

````
Tools/scripts/install-prereqs-ubuntu.sh -y
````

Reload the path (log-out and log-in to make permanent):

````
. ~/.profile
````


### Finalize and test the installation

To start the simulator first change directory to the vehicle directory. For example, for the multicopter code change to ardupilot/ArduCopter:


````
cd ardupilot/ArduCopter
````

Then start the simulator using sim_vehicle.py. The first time you run it you should use the -w option to wipe the virtual EEPROM and load the right default parameters for your vehicle.

````
sim_vehicle.py --console --map
````

### Updating MAVProxy and pymavlink

New versions of MAVProxy and pymavlink are released quite regularly. If you are a regular SITL user you should update every now and again using this command

````
sudo pip install --upgrade pymavlink MAVProxy
````


This concludes the first step SITL Ardupilot installation. 

## SETP 2 - Ardupilot gazebo plugin installation :

Instructions taken from ardupilot.org (See original instructions here http://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html)


Ardupilot gazebo plugin (follow instruction here https://github.com/r0ch1n/ardupilot_gazebo)
