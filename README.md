 ### create a workspace
 ```
  mkdir −p ~/ros/workspaces/hsrb_ws/src
  cd ~/ros/workspaces/hsrb_ws
  catkin build
  source devel/setup.bash
  catkin build ics_gazebo controller_tutorials
 ```

 ### launch HSRB robot in the saved scenario:
 ```
 roslaunch ics_gazebo hsrb.launch world_suffix:=tutorial  
 ```
 ### start keyborad control
 ```
 rosrun key_teleop key_teleop.py
 ```
 ### remap the publisher of the key_teleop node
 ```
 rosrun topic_tools relay /key_vel /base_velocity 
 ```
 # 5.2 
 ## 1. launch a simulation of the robot on the empty world
 ```
 roslaunch ics_gazebo hsrb.launch world_suffix:=empty
 ```
 ## 2. Run the next command in another terminal to list all the controllers currently running on the robot. (Start simulation in Gazebo first)
 ```
 rosservice call /hsrb/controller_manager/list_controllers
 ```
 ## 3. - Publish a geometry_msgs/Twist message to the /mobile_base_controller/cmd_vel topic using the rostopic node at a rate of three times per second.
 ```
 rostopic pub /base_velocity geometry_msgs/Twist "linear:
  x: 0.5
  y: 0.0
  z: 0.0
 angular:
  x: 0.0
  y: 0.0
  z: 0.0" -r 3
 ```
 ## 5. publish a command to the arm controller
 ```
rostopic pub /hsrb/arm_trajectory_controller/command trajectory_msgs/JointTrajectory "header:
  seq: 0
  stamp:
    secs: 0
    nsecs: 0
  frame_id: ''
joint_names: [arm_flex_joint, arm_lift_joint, arm_roll_joint, wrist_flex_joint, wrist_roll_joint]
points:
  - 
    positions: [0, 0, 0, 0, 0]
    velocities: []
    accelerations: []
    effort: []
    time_from_start: 
      secs: 1
      nsecs: 0"
 ```
# 6.3
```
catkin build 
roslaunch ics_gazebo hsrb.launch world_suffix:=tutorial2
roslaunch rover_controller rover_controller.launch
```

# 6.4
 ## 1. launch a simulation of the robot on the empty world
 ```
 roslaunch ics_gazebo hsrb.launch world_suffix:=tutorial2
 ```
 ## 2. In another terminal run the next command to show controller list
 ```
 rosservice call /hsrb/controller_manager/list_controllers
```
## 3. If head_trajectory_controller is running, stop it with the next command.
```
export ROS_NAMESPACE=/hsrb
rosrun controller_manager controller_manager kill head_trajectory_controller
```
 ## load new head ontroller with 
 ```
 roslaunch controllers_tutorials new_head_controller_hsrb.launch
```
## 4. Stop the controller
```
rosrun controller_manager controller_manager kill new_head_controller
```




