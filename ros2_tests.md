# ROS2 tests

## ROS2 Basics

This applies to both the rpi 4 (ros-foxy) and your ubuntu machine (ros-humble).  
After installing ros2 you should have been instructed to add the following source command to your ~/.bashrc file:  
`source /opt/ros/foxy/setup.bash`  
`source /home/<user>/ros2_ws/install/setup.bash`  
The second line is your custom ros2 workspace directory, which is already on the rpi 4.  

### Create 3 ros2 tests (not required for Your custom ros2 workspace or xv11 tests)

Once you have the create 3 connected to your wifi, try the following from your ubuntu machine terminal:  
`ros2 topic list`  
`ros2 topic echo /ir_intensity`  
`ros2 topic info /ir_intensity`  
`rqt_graph`  
`ros2 run turtlesim turtle_teleop_key --ros-args -r /turtle1/cmd_vel:=/cmd_vel`  
Drive the robot with the arrow keys (keep the turtle_teleop_key terminal active).  

### Your custom ros2 workspace

Setup the same custom ros2 workspace on your ubuntu machine:  
`mkdir -p ros2_ws/src`  
`cd ~/ros2_ws/src`  
`git clone -b ros2 https://github.com/coderkarl/xv_11_laser_driver.git`  
Note the -b ros2 option says to checkout the ros2 branch (vs the default ros1 branch).  
Change to the top level ros2_ws directory to build the source code using colcon.  
`cd ~/ros2_ws`  
`colcon build --symlink-install --packages-select xv_11_laser_driver --cmake-args ' -DCMAKE_BUILD_TYPE=Release'`  
You can simply use `colcon build` but I prefer to always build one package at a time.  
I have concluded the `--symlink-install` and `--cmake-args ' -DCMAKE_BUILD_TYPE=Release'` options should always be used.  
Repeat for another custom package (note this ros2 branch is the same in my forked coderkarl nav_sim repo  
`cd ~/ros2_ws/src`  
`git clone --branch ros2 --single-branch https://github.com/CentralIllinoisRoboticsClub/nav_sim.git`  
`cd ~/ros2_ws`  
`colcon build --symlink-install --packages-select nav_sim --cmake-args ' -DCMAKE_BUILD_TYPE=Release'`  
This build is likely to fail without following the dependency instructions in that repo README.md file.  

After a successful build, either open a new terminal (your .bashrc will be applied), OR type:  
`source ~/.bashrc`  
Note you really only need to re-run `source /home/<user>/ros2_ws/install/setup.bash`  

## XV11 Neato Lidar ROS2 driver

This is the xv_11_laser_driver package.  
First try to test this on your ubuntu machine.  
### Determine usb port of the lidar (note this will not likely be constant, check every time you plug it in).  
Plug in the lidar to your laptop using the usb cable provided with the lidar.  
In an ubuntu terminal:  
`ls /dev/ttyACM <HIT TAB TAB>`  
You should get just ttyACM0 or a list of options.  
If you get nothing, try `ls /dev/ttyUSB <HIT TAB TAB>`  
### Update the lidar port in the config file  
`cd ~/ros2_ws/src/xv_11_laser_driver/launch`  
`ls`  
`nano xv11_laser.launch.py`  
Note you can just do nano (Hit tab) because there is only one file.  
Find the string /dev/neato_laser or /dev/ttyACM* or /dev/ttyUSB*  and replace it with the correct port.  
neato_laser is a symlink you can setup based on properties of the usb device.  

### Run the xv11 ros2 lidar driver  
`ros2 launch xv_11_laser_driver xv11_laser.launch.py`  
If auto tab complete does not work or the package or launch file is not found, make sure you have done `source /home/<user>/ros2_ws/install/setup.bash`  

Once the lidar works on your laptop, try to repeat this with it connected to the rpi4.

### View the lidar with rviz2  
This will only work on your ubuntu machine (the rpi 4 does not have a full desktop gui).  
Whether you are running the xv11 ros2 lidar driver node on the rpi4 or your laptop, ros2 should pass the data to rviz2 over the network.  
`rviz2`  
In the left rviz2 menu, click Add at the bottom left.  
Go to the right By Topic tab and find the LaserScan message with the topic name "scan".  
At the top of the rviz2 menu, make sure the frame is "laser" or "base_link".  
Expand the LaserScan object in the menu and expand the topic section.  
Reliability: Reliable  
Durability:  Volatile  
If nothing shows up, run `rqt_graph` in a separate terminal. Refresh the graph. Hover over the arrow showing the /scan topic. Check the topic settings.  

## USB Symlinks
[How to make symlinks for usb ports in linux](https://community.openhab.org/t/how-to-make-symlinks-for-usb-ports-in-linux-extra-java-opts/89615)  
This is what I did on my laptop for the xv11 neato lidar:  
`sudo nano /etc/udev/rules.d/97-usb-serial.rules`  
`SUBSYSTEM=="tty", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="0483", ATTRS{serial}=="12345", SYMLLINK+="neato_laser"`  
