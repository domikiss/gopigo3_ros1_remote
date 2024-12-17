# gopigo3_ros1_remote

This package contains ROS1 launch files to launch distributed ROS1-based GoPiGo3 robot demo applications (tested under Ubuntu 20.04 and ROS Noetic). 

The following use cases can be demonstrated:

1. Baseline case: Robot simulation, RViz, SLAM, and navigation on the local machine (`gopigo3_sim_slam_nav.launch`)
2. Bringup a real GoPiGo3 robot remotely + RViz, SLAM, and navigation run on the local machine (`gopigo3_real_slam_nav.launch`)
3. Robot simulation and RViz on the local machine + SLAM and navigation run on a remote machine in a Docker container (`gopigo3_sim_remote_docker_slam_nav.launch`)
4. Bringup a real GoPiGo3 robot remotely + SLAM and navigation run on another remote machine in a Docker container + RViz run on the local machine (`gopigo3_real_remote_docker_slam_nav.launch`)

These packages must be available on the local machine in a Catkin workspace:

- [gopigo3](https://github.com/domikiss/gopigo3)
- [gopigo3_nav_sim](https://github.com/domikiss/gopigo3_nav_sim)
- [remote_launcher_ros1](https://github.com/domikiss/remote_launcher_ros1)
- and this package ([gopigo3_ros1_remote](https://github.com/domikiss/gopigo3_ros1_remote))

It is assumed that the remote machine contains a Docker image based on the [gopigo3_ros1_docker](https://github.com/domikiss/gopigo3_ros1_docker) repository. 

It is further assumed that the real robot and the other remote machine are accessible via SSH without having to enter a password, i.e., by creating an SSH key and installing it to these remote machines. If you are unfamiliar with this topic, check this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server).


## gopigo3_sim_slam_nav.launch

This launch file starts a Gazebo simulation, RViz, SLAM, and navigation nodes locally. The following launch arguments are available:

- `sim_scene`: Simulated scene. You can choose from the launch file names of the [gopigo3_gazebo](https://github.com/domikiss/gopigo3_nav_sim/tree/master/gopigo3_gazebo/launch) package (without the .launch extension). The default value is `gopigo3_turtlebot3_house`.
- `sim_gui`: Whether to start Gazebo GUI (default: true)
- `open_rviz`: Whether to start RViz (default: true)
- `joy_teleop`: Whether to start a joystick teleoperation node (if true, an Xbox controller should be connected, default: false)

Usage example (without joystick teleoperation):
```
$ roslaunch gopigo3_ros1_remote gopigo3_sim_slam_nav.launch
```


## gopigo3_real_slam_nav.launch

This launch file connects to a real GoPiGo3 robot, launches a roslaunch file on it, and starts RViz, SLAM, and navigation nodes locally. The following launch arguments are available:

- `ros_master_uri`: The ROS master URI in the form of `http://[ROS_MASTER_IP]:11311`. This parameter will be used for specifying the `ROS_MASTER_URI` environment variable in the remote session on the robot (default: the value of the local `ROS_MASTER_URI` environment variable)
- `robot user`: Username on the robot
- `robot_machine`: IP address (or hostname) of the robot 
- `robot_ros_package`: ROS1 package name available in the robot (typically the robot bringup package)
- `robot_roslaunch_file`: Launch file name in the ROS1 package
- `open_rviz`: Whether to start RViz (default: true)
- `joy_teleop`: Whether to start a joystick teleoperation node locally (if true, an Xbox controller should be connected, default: false)

The launch file specifies default values for the robot-specific arguments according to a real GoPiGo3 instance in our lab at BME AUT. You can replace them with appropriate values to avoid specifying these in the command line.

Usage example (without joystick teleoperation):
```
$ roslaunch gopigo3_ros1_remote gopigo3_real_slam_nav.launch
```


## gopigo3_sim_remote_docker_slam_nav.launch

This launch file starts a Gazebo simulation and RViz locally, while the SLAM and navigation nodes are started on a remote machine inside a Docker container. The following launch arguments are available:

- `sim_scene`: Simulated scene. You can choose from the launch file names of the [gopigo3_gazebo](https://github.com/domikiss/gopigo3_nav_sim/tree/master/gopigo3_gazebo/launch) package (without the .launch extension). The default value is `gopigo3_turtlebot3_house`.
- `sim_gui`: Whether to start Gazebo GUI (default: true)
- `open_rviz`: Whether to start RViz (default: true)
- `joy_teleop`: Whether to start a joystick teleoperation node locally (if true, an Xbox controller should be connected, default: false)
- `remote_user`: Username on the remote machine containing the Docker image (default: value of the local `USER` environment variable)
- `remote_machine`: IP address (or hostname) of the remote machine containing the Docker image (default: localhost)
- `ros_master_uri`: ROS master URI in the form of `http://[ROS_MASTER_IP]:11311`. This parameter will be used for specifying the `ROS_MASTER_URI` environment variable inside the remote docker container (default: value of the local `ROS_MASTER_URI` environment variable)
- `docker_image`: Name of the Docker image on the remote machine (default: `ros-noetic-nav:gopigo3`)
- `docker_ros_package`: ROS1 package name inside the container (default: `gopigo3_navigation`)
- `docker_roslaunch_file`: Launch file name in the ROS1 package (default: `gopigo3_slam_navigation.launch`)

Usage (example):
```
$ roslaunch gopigo3_ros1_remote gopigo3_sim_remote_docker_slam_nav.launch \
  remote_user:=[USER] remote_machine:=[IP.OF.REMOTE.MACHINE]
```


## gopigo3_real_remote_docker_slam_nav.launch

This launch file connects to a real GoPiGo3 robot, launches a roslaunch file on it, and starts RViz, SLAM, and navigation nodes locally. The following launch arguments are available:

This launch file connects to a real GoPiGo3 robot, launches a roslaunch file on it, while the SLAM and navigation nodes are started on a remote machine inside a Docker container. RViz is started locally. The following launch arguments are available:

- `ros_master_uri`: The ROS master URI in the form of `http://[ROS_MASTER_IP]:11311`. This parameter will be used for specifying the `ROS_MASTER_URI` environment variable in the remote session on the robot and inside the remote docker container (default: the value of the local `ROS_MASTER_URI` environment variable)
- `robot user`: Username on the robot
- `robot_machine`: IP address (or hostname) of the robot 
- `robot_ros_package`: ROS1 package name available in the robot (typically the robot bringup package)
- `robot_roslaunch_file`: Launch file name in the ROS1 package
- `remote_user`: Username on the remote machine containing the Docker image (default: value of the local `USER` environment variable)
- `remote_machine`: IP address (or hostname) of the remote machine containing the Docker image (default: localhost)
- `docker_image`: Name of the Docker image on the remote machine (default: `ros-noetic-nav:gopigo3`)
- `docker_ros_package`: ROS1 package name inside the container (default: `gopigo3_navigation`)
- `docker_roslaunch_file`: Launch file name in the ROS1 package (default: `gopigo3_slam_navigation.launch`)
- `open_rviz`: Whether to start RViz locally (default: true)
- `joy_teleop`: Whether to start a joystick teleoperation node locally (if true, an Xbox controller should be connected, default: false)

The launch file specifies default values for the robot-specific arguments according to a real GoPiGo3 instance in our lab at BME AUT. You can replace them with appropriate values to avoid specifying these in the command line.


Usage example (without joystick teleoperation):
```
$ roslaunch gopigo3_ros1_remote gopigo3_real_remote_docker_slam_nav.launch \
  remote_user:=[USER] remote_machine:=[IP.OF.REMOTE.MACHINE]
```
