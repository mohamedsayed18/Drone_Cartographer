# Iris Cartographer SLAM

We will learn how to do SLAM using cartographer, with drone in gazebo

**Note**: if you are new to drone simulation and PX4 check this [project](https://github.com/mohamedsayed18/Drone_simulation)

To install the cartographer package check their [documentation](https://google-cartographer-ros.readthedocs.io/en/latest/index.html)

I build a launch file which launch a drone with depth camera in a Gazebo world

``` bash
roslaunch drone iris_depth_camera.launch
```

The topics needed for SLAM:

* the point cloud topic `camera/depth/points`
* the imu topic `/mavros/imu/data`

We now done with the drone, Let's run the cartographer

source the cartographer package:
`source install_isolated/setup.bash`

launch the Cartographer SLAM path: catkin_ws/install_isolated/share/cartographer_ros/launch/my_robot.launch

```bash
roslaunch cartographer_ros my_robot.launch
```

### How I created this launch file

I create a copy of the `backpack_3d.lua`

```bash
cp install_isolated/share/cartographer_ros/configuration_files/backpack_3d.lua src/drone_pkg/config/my_robot.lua
```

I created new launch file `my_robot.launch`

```bash
cp install_isolated/share/cartographer_ros/launch/backpack_3d.launch src/drone_pkg/launch/my_robot.launch
```

created a [URDF] of my robot, this file will just describe the important links not the whole of the robot.

In `my_robot.lua` edited the number of point clouds depending on the type of sensors you have
`num_point_clouds = 1,`

In the `my_robot.launch` I referred to `my_robot.lua` 

remapped the desired topic so cartographer can deal with it

```xml
    <remap from="imu" to="/mavros/imu/data" />
    <remap from="points2" to="camera/depth/points" />
```

## TODO

* make a detailed description of the launch, lau, and urdf