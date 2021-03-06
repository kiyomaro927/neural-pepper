# Pepper with Neuraltalk

This is a program to operate Pepper with a joystick, and do Neuraltalk.
To run this program, please set up [this server program](https://github.com/kiyomaro927/NeuraltalkServer).

## Preparation

### Ubuntu

You should run this app on Ubuntu because of using ROS.
ROS (Robot Operating System) is a middleware to operate robots simply and it supports Ubuntu officially.
If it is possible, a native installed machine is better than a virtual machine.

### ROS packages

When you get Ubuntu machine, open your terminal and run following commands to install ROS.

```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
```

```
$ wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
```

```
$ sudo apt-get update
```

```
$ sudo apt-get install ros-indigo-desktop-full
```

```
$ sudo rosdep init
$ rosdep update
```

```
$ echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```

```
$ sudo apt-get install python-rosinstall
```

If you have trouble installing ROS, check the [official page](http://wiki.ros.org/ja/indigo/Installation/Ubuntu).

### ROS packages relating to Pepper

Next, install ROS packages relating to Pepper.
First, install some additional ROS packages.

```
$ sudo apt-get install ros-indigo-driver-base ros-indigo-move-base-msgs ros-indigo-octomap ros-indigo-octomap-msgs ros-indigo-humanoid-msgs ros-indigo-humanoid-nav-msgs ros-indigo-camera-info-manager ros-indigo-camera-info-manager-py
```

And get official packages relating to pepper.

```
$ sudo apt-get install ros-indigo-pepper-.*
```

To develop around pepper, please run following commands.

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ git clone https://github.com/ros-naoqi/naoqi_driver.git
```

Then, get dependencies.

```
$ rosdep install -i -y --from-paths ./naoqi_driver
```

Finally, build the app.

```
$ source /opt/ros/indigo/setup.sh
$ cd ../ && catkin_make
```

I heard that catkin_make is does not work well in a paticular environment.
This command may solve the problem.

```
$ sudo apt-get install ros-indigo-velodyne-pointcloud
$ pip install catkin_pkg
```

To show details, please go to the [official page](http://wiki.ros.org/pepper/Tutorials).


### ROS packages relating to Joystick

Next, install ROS packages relating to joystick.
Install the packages.

```
$ sudo apt-get install ros-indigo-joy
$ sudo apt-get install ros-indigo-joystick-drivers
```

Get dependencies and compile them.

```
$ rosdep install joy
$ rosmake joy
```

To make sure that joystick is working, 

```
$ sudo jstest /dev/input/jsX
```

To change permission,

```
$ sudo chmod a+rw /dev/input/jsX
```

We need to select appripriate joy node.

```
$ rosparam set joy_node/dev "/dev/input/jsX"
```

### Microsoft translator API

Access [Windows Azure Marketplace](https://datamarket.azure.com/home) and register an application.

And install python SDK.

```
$ pip install microsofttranslator
```

Sorry to trouble you, but please tweak `/scripts/node_runner:34`.

## Run

First, bring up roscore.

```
$ roscore
```

Run the joy node.

```
$ rosrun joy joy_node
```

Connect to pepper.

```
$ roslaunch pepper_bringup pepper_full.launch nao_ip:=<yourRobotIP>
```

Start Neuraltalk server.

```
$ cd NeuraltalkServer
$ python neuralTalkServer.py
```

Run node_runner.

```
$ cd THIS_REPO/scripts/
$ python ./node_runner.py
```
