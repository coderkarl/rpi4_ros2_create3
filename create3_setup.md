# Create3 Setup  

## Create3 Overview, Firmware update 

[Create3 Getting Started](https://edu.irobot.com/learning-library/create-3-getting-started)  
Scroll to the bottom and click PDF Create3 Getting Started Manual to download the PDF.  

## Connect Create3 to your wifi

(https://iroboteducation.github.io/create3_docs/webserver/connect/)  
My ros2 foxy worked with my Create3 once I connected the robot to my home wifi network.  
I did not have to make any chages described in the Setup, ROS2 Setup section:  
  [ROS2 Setup](https://iroboteducation.github.io/create3_docs/setup/ubuntu2204/)  

## Create3 ros2 msg and package examples source code setup

[https://github.com/iRobotEducation/create3_examples](https://github.com/iRobotEducation/create3_examples)  
On you ubuntu machine:  
`cd ~/ros2_ws/src`  
`git clone https://github.com/iRobotEducation/create3_examples.git`  
`cd ~/ros2_ws`
Build all packages in the ros2_ws. The create3_examples repo is a meta package of several packages (the subfolders).  
`colcon build --symlink-install --cmake-args ' -DCMAKE_BUILD_TYPE=Release'`  
These are the same instructions as in the create3_examples github repo README.md except,  
we are putting these packages in our ros2_ws workspace instead of making a separate workspace like they did.  
See the following for using rosdep to install dependencies specified in a package:  
[rosdep](https://docs.ros.org/en/foxy/Tutorials/Intermediate/Rosdep.html#how-do-i-use-the-rosdep-tool)  
Make sure you have installed ros-dep:  
`sudo apt install ros-dev-tools`  
[ros2 install](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)  
