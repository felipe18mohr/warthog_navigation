# warthog_navigation

![Warthog Navigation](images/navigation.png)

Create a workspace and clone the needed packages:
``` bash
$ mkdir -p ~/warthog_ws/src 
$ cd ~/warthog_ws/src
$ git clone https://github.com/warthog-cpr/warthog
$ git clone https://github.com/warthog-cpr/warthog_simulator
$ git clone https://github.com/warthog-cpr/warthog_desktop
$ git clone https://github.com/felipe18mohr/warthog_navigation
```

Install all dependencies and compile:
``` bash
$ rosdep install --from-paths src --ignore-src --rosdistro=noetic -y
$ cd ../ && catkin_make
```

In the **accessories.urdf.xacro** file of the *warthog_description* package, add the following lines:
```xml
  <xacro:include filename="$(find velodyne_description)/urdf/VLP-16.urdf.xacro" />
  <xacro:VLP-16 max_range="40" samples="600" hz="5" lasers="8">
    <origin xyz="0.15 0.0 0.7" rpy="0.0 0.0 0.0"/>
  </xacro:VLP-16>

  <xacro:include filename="$(find warthog_navigation)/description/urdf/kinect.urdf.xacro" />
  <xacro:sensor_kinect parent="base_link" cam_px="0.52" cam_py="0.0" cam_pz="0.54" 
                                          cam_or="0.0" cam_op="0.0" cam_oy="0.0" />
  <xacro:sim_3dsensor_kinect/> 
```


Change the odom0 and imu0 setting from the **control.yaml** file in the *warthog_control* package to the following:
```yaml
  odom0_config: [false, false, false,
                 false, false, false,
                 true, true, true,
                 false, false, true,
                 false, false, false]
            
  imu0_config: [false, false, false,
                true, true, true,
                false, false, false,
                true, true, true,
                false, false, false]
```

Launch navigation:
``` bash
$ source devel/setup.bash
$ roslaunch warthog_navigation main.launch
```

You can change the *main.launch* file with the launchs you want to perform your task.

To save a map using *octomap_mapping.launch*, use:
```bash
$ rosrun octomap_server octomap_saver -f map.bt
```
To load that map:
```bash
$ rosrun octomap_server octomap_server_node map.bt
```