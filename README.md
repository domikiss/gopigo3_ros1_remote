# gopigo3_ros1_remote

This package contains ROS1 launch files to launch distributed ROS1-based GoPiGo3 robot demo applications (tested under Ubuntu 20.04 and ROS Noetic). 

The following use cases can be demonstrated:

1. Robot simulation and RViz on the local machine + SLAM and navigation run on a remote machine in a Docker container
2. Real GoPiGo3 robot + SLAM and navigation run on a remote machine in a Docker container + RViz on the local machine

These packages must be available on the local machine in a Catkin workspace:

- [gopigo3](https://github.com/domikiss/gopigo3)
- [gopigo3_nav_sim](https://github.com/domikiss/gopigo3_nav_sim)
- [remote_docker_launcher_ros1](https://github.com/domikiss/remote_docker_launcher_ros1)
- and this package (gopigo3_ros1_remote)

It is assumed that the remote machine contains a Docker image based on the [gopigo3_ros1_docker](https://github.com/domikiss/gopigo3_ros1_docker) repository. 

It is further assumed that the remote machine is accessible via SSH without having to enter a password, i.e., by creating an SSH key and installing it to the remote machine. If you are unfamiliar with this topic, check this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server).

## gopigo3_sim_remote_docker_slam_nav.launch

This launch file starts a Gazebo simulation and RViz locally, while the SLAM and navigation nodes are started on a remote machine inside a Docker container. The following launch arguments are available:

- `sim_scene`: Simulated scene. You can choose from the launch file names of the [gopigo3_gazebo](https://github.com/domikiss/gopigo3_nav_sim/tree/master/gopigo3_gazebo/launch) package (without the .launch extension). The default value is `gopigo3_turtlebot3_house`.
- `sim_gui`: Whether to start Gazebo GUI (default: true)
- `open_rviz`: Whether to start RViz (default: true)
- `remote_user`: User of the remote machine
- `remote_machine`: IP address (or hostname) of the remote machine containing the Docker image
- `ros_master_uri`: ROS master URI in the form of `http://[ROS_MASTER_IP]:11311` (default: the current value of the ROS_MASTER_URI environment variable)
- `docker_image`: Name of the Docker image on the remote machine (default: `ros-noetic-nav:gopigo3`)
- `docker_ros_package`: ROS1 package name inside the container (default: `gopigo3_navigation`)
- `docker_roslaunch_file`: Launch file name in the ROS1 package (default: `gopigo3_slam_navigation.launch`)
- `docker_roslaunch_args`: Launch arguments for the launch file (optional)

Usage (example):
```
$ roslaunch gopigo3_ros1_remote gopigo3_sim_remote_docker_slam_nav.launch \
  remote_user:=[USER] remote_machine:=[IP.OF.REMOTE.MACHINE]
```

## gopigo3_real_remote_docker_slam_nav.launch

Coming soon...
