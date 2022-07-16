<h1 align="center">Subpart-3</h1>  

## Flashback Time  
Remember in Subpart-1 we mentioned there are [two options](https://github.com/san2130/ROS-Specialization-22/tree/main/week3/Subpart-1#follow-along-tasks)? 
- Now its time to implement the first option since for real robots and robotic arms even in simulation, you will have to write your own controllers.

## Follow-Along Tasks  
- Remove the skid_steer_drive plugin from the urdf.  
- Install ros-control (replace noetic with your respective ROS distribution)
```bash
sudo apt-get install ros-noetic-ros-control ros-noetic-ros-controllers
```
- A Gazebo plugin needs to be added to your URDF that actually parses the tags you will be adding and loads the appropriate hardware interfaces and controller manager. By default the gazebo_ros_control plugin is very simple, though it is also extensible via an additional plugin architecture to allow power users to create their own custom robot hardware interfaces between ros_control and Gazebo.  
**Add** these lines to the **urdf** file (you need to add it within the robot tag).
```xml
<gazebo>
    <plugin name="gazebo_ros_control" filename="libgazebo_ros_control.so">
    <robotNamespace>/bot</robotNamespace>
    </plugin>
</gazebo>  
```

- Now add these transmission tags to the urdf within the robot tag again
```xml
<transmission name="tran1">
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="husky_robot_model__front_left_wheel_joint">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
    </joint>
    <actuator name="front_left_motor">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>

<transmission name="tran2">
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="husky_robot_model__front_right_wheel_joint">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
    </joint>
    <actuator name="front_right_motor">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>

<transmission name="tran3">
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="husky_robot_model__rear_left_wheel_joint">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
    </joint>
    <actuator name="rear_left_motor">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>

<transmission name="tran4">
    <type>transmission_interface/SimpleTransmission</type>
    <joint name="husky_robot_model__rear_right_wheel_joint">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
    </joint>
    <actuator name="rear_right_motor">
        <hardwareInterface>hardware_interface/EffortJointInterface</hardwareInterface>
        <mechanicalReduction>1</mechanicalReduction>
    </actuator>
</transmission>
```  
- Now create a config file inside a config folder within camp(for better organization) in the bot_control.yaml with the following lines
```xml
bot:
  # Publish all joint states -----------------------------------
  joint_state_controller:
    type: joint_state_controller/JointStateController
    publish_rate: 50  

  # Position Controllers ---------------------------------------
  front_left_wheel_controller:
    type: effort_controllers/JointPositionController
    joint: husky_robot_model__front_left_wheel_joint
    pid: {p: 100.0, i: 0.01, d: 10.0}
  front_right_wheel_controller:
    type: effort_controllers/JointPositionController
    joint: husky_robot_model__front_right_wheel_joint
    pid: {p: 100.0, i: 0.01, d: 10.0}
  rear_left_wheel_controller:
    type: effort_controllers/JointPositionController
    joint: husky_robot_model__rear_left_wheel_joint
    pid: {p: 100.0, i: 0.01, d: 10.0}
  rear_right_wheel_controller:
    type: effort_controllers/JointPositionController
    joint: husky_robot_model__rear_right_wheel_joint
    pid: {p: 100.0, i: 0.01, d: 10.0}
```  
- Create a launch file bot_control.launch in the launch directory and paste following code in it.  
```xml
<launch>

  <!-- Load joint controller configurations from YAML file to parameter server -->
  <rosparam file="$(find camp)/config/bot_control.yaml" command="load"/>

  <!-- load the controllers -->
  <node name="controller_spawner" pkg="controller_manager" type="spawner" respawn="false"
    output="screen" ns="/bot" args="front_left_wheel_controller front_right_wheel_controller rear_left_wheel_controller rear_right_wheel_controller joint_state_controller"/>

  <!-- convert joint states to TF transforms for rviz, etc -->
  <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher"
    respawn="false" output="screen">
    <remap from="/joint_states" to="/bot/joint_states" />
  </node>

</launch>
```  
- Now fire up two terminals and launch visualize.launch in one and bot_control.launch in another. 
```bash
~/catkin_ws $ roslaunch camp visualize.launch
~/catkin_ws $ roslaunch camp bot_control.launch
```
Now to see whether controllers are spawned use rostopic list command to list out all the topics, you should see output something similar to this 
```bash
~/catkin_ws $ rostopic list
/bot/joint1_position_controller/command
/bot/joint1_position_controller/pid/parameter_descriptions
/bot/joint1_position_controller/pid/parameter_updates
/bot/joint1_position_controller/state
/bot/joint2_position_controller/command
/bot/joint2_position_controller/pid/parameter_descriptions
/bot/joint2_position_controller/pid/parameter_updates
/bot/joint2_position_controller/state
/bot/joint3_position_controller/command
/bot/joint3_position_controller/pid/parameter_descriptions
/bot/joint3_position_controller/pid/parameter_updates
/bot/joint3_position_controller/state
/bot/joint4_position_controller/command
/bot/joint4_position_controller/pid/parameter_descriptions
/bot/joint4_position_controller/pid/parameter_updates
/bot/joint4_position_controller/state
/bot/joint_states
```
(there will be more topics but these should definitely be there)
