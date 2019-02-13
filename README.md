# SITL + Ardupilot + Gazebo + ROS Camera plugin
SITL + ArduPilot + Gazebo + ROS Camera Plugin (Software In Loop Simulation Interfaces, Models)


## Incomplete Warning :

This tutorial is not yet complete.... please be patient and wait...

## Description :

Finally an all in one tutorial for setting up your virtual drone using Ardupilot (Arducopter) + SITL in an complete 3d virtual enviroment provided by Gazebo.

At last but not least (Actually the most complicated to find examples of...) added the camera plugin from ROS to publish the images so you can take them outside Gazebo and do something with them...

Following are the requirements, setup steps and finally how to of each part.. 


## Requirements :
* Ubuntu (18.04 LTS) with full 3D graphics.
* Gazebo version 8.x or greater (Recomended 9.6)
* ROS melodic (Required to work with Gazebo)
* MAVROS

## Environment setup and configuration :
* STEP 1 - SITL Ardupilot
* STEP 2 - Ardupilot gazebo plugin (Original khancyr version)
* STEP 3 - 
* STEP 4 - Connect ArduPilot to ROS

## Gazebo 9 :

### Setup your computer to accept software from packages.osrfoundation.org.
````
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
````

### Setup keys
````
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
````

### Install Gazebo
````
sudo apt-get update
sudo apt-get install gazebo9
sudo apt-get install libgazebo9-dev
````

## ROS melodic installation :

Install ROS with sudo apt install ros-melodic-desktop-full (follow instruction here http://wiki.ros.org/melodic/Installation/Ubuntu).

### Configure your Ubuntu repositories
````
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

sudo apt update
````

### Test Gazebo
````
gazebo --verbose
````

### Install ROS melodic
````
sudo apt install ros-melodic-desktop-full
````

### Initialize ROS
````
sudo rosdep init
rosdep update
````

### Environment setup
````
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
````

### Dependencies for building packages
````
sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
````

## MAVROS installation :

MAVLink extendable communication node for ROS with proxy for Ground Control Station (See original instructions here http://ardupilot.org/dev/docs/ros-install.html#installing-mavros).

### Configure your Ubuntu repositories
````
sudo apt-get install ros-kinetic-mavros ros-kinetic-mavros-extras

cd ~/

wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh

chmod a+x install_geographiclib_datasets.sh

./install_geographiclib_datasets.sh
````

### For ease of use on a desktop computer, please also install RQT
````
sudo apt-get install ros-kinetic-rqt ros-kinetic-rqt-common-plugins ros-kinetic-rqt-robot-plugins
````

### Install catkin tools
````
sudo apt-get install python-catkin-tools
````

## SETP 1 - SITL Ardupilot installation :

Instructions taken from ardupilot.org (See original instructions here http://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html).

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
cd ~/ardupilot/ArduCopter
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

(See original instructions here https://github.com/khancyr/ardupilot_gazebo).

### Clone ArduPilot repository

````
cd ~/

git clone https://github.com/r0ch1n/ardupilot_gazebo

cd ardupilot_gazebo

mkdir build

cd build

cmake ..

# use make without any parameter if running in a VM
make -j4

sudo make install
````

### Set environment variables

````
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc

````

Set Path of Gazebo Models (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
````

Set Path of Gazebo Worlds (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_RESOURCE_PATH=~/ardupilot_gazebo/worlds:${GAZEBO_RESOURCE_PATH}' >> ~/.bashrc
````

Reload the path (log-out and log-in to make permanent):

````
source ~/.bashrc
````

### Test installation

Open one Terminal and launch SITL Ardupilot
````
sim_vehicle.py -v ArduCopter -f gazebo-iris --map --console
````

Open a second Terminal and launch Gazebo running ardupilot_gazebo plugin
````
gazebo --verbose worlds/iris_arducopter_runway.world
````

You should see a gazebo world with a small quadcopter right at the center



## SETP 4 - Connect ArduPilot to ROS using MAVROS :

Connect to Ardupilot from ROS (Ardupilot <–> MAVLink <–> ROS )  Original information taken from here http://ardupilot.org/dev/docs/ros-sitl.html
Note - Gazebo is not included in MAVROS so you cannot connect or access any of the Gazebo's Environments. Including the Cameras feed. We use a different approach to acces this information in STEP X.



### Run

New versions of MAVProxy and pymavlink are released quite regularly. If you are a regular SITL user you should update every now and again using this command

````
roslaunch apm.launch
````

