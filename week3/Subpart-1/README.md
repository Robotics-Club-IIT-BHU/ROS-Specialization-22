<h1 align="center">Subpart-1</h1><br>
<h2 align="center"> Gazebo</h2>

<p align="center"> <img src="https://www.clearpathrobotics.com/assets/guides/kinetic/moose/_images/moose_simulator_gazebo.png"/> <i><u>A Screen shot of Gazebo</u></i>
</p>

In this subpart we will learn about Gazebo. Gazebo is a 3D dynamic simulator just like Pybullet with the ability to accurately and efficiently simulate populations of robots in complex indoor and outdoor environments. While similar to game engines, Gazebo offers physics simulation at a much higher degree of fidelity, a suite of sensors, and interfaces for both users and programs.

Typical uses of Gazebo include:

- testing robotics algorithms,
- designing robots,
- performing regression testing with realistic scenarios

A few key features of Gazebo include:

- multiple physics engines,
- a rich library of robot models and environments,
- a wide variety of sensors,
- convenient programmatic and graphical interfaces

Enough definitions in this subpart we will simulate a robot and move it gazebo. Excited?

<div align="center">
    <img src="https://media.tenor.com/images/28b60454008ba0a45bcd512264fb7c59/tenor.gif"/>
</div>

### Installation (Only for ROS Melodic)

```
curl -sSL http://get.gazebosim.org | sh
```

then check if installation worked. Running below command should open up gazebo window.

```
gazebo
```  


## Follow-along Tasks

- We will be using the same package as in week 2, just **replace** the **worlds** folder in [camp](https://github.com/san2130/ROS-Specialization-22/tree/main/week2/camp) with [this](https://github.com/san2130/ROS-Specialization-22/tree/main/week3/Subpart-1/worlds) and we will make husky move in Gazebo.
- If you have checked the urdf you will find 4 joints one for each wheel, now there are two options-
   1. Configure each joint by specifying the PID constants for each joint and tune them.  
   2. The easy way - simply use the skid_steer_drive plugin. We go with this way for now.  
- Think of a plugin as reusable code which is available for you to use. Refer to [this](https://classic.gazebosim.org/tutorials?tut=ros_gzplugins) for all the gazebo plugins available. 
- Now add the following lines to your urdf within the robot tags. 
```xml
<gazebo>
    <plugin name="skid_steer_drive_controller" filename="libgazebo_ros_skid_steer_drive.so">
      <updateRate>40.0</updateRate>
      <leftFrontJoint>husky_robot_model__front_left_wheel_joint</leftFrontJoint>  
      <rightFrontJoint>husky_robot_model__front_right_wheel_joint</rightFrontJoint>
      <leftRearJoint>husky_robot_model__rear_left_wheel_joint</leftRearJoint>
      <rightRearJoint>husky_robot_model__rear_right_wheel_joint</rightRearJoint>
      <wheelSeparation>0.57</wheelSeparation>
      <wheelDiameter>0.34</wheelDiameter>
      <robotBaseFrame>husky_robot_model__base_link</robotBaseFrame>
      <torque>60</torque>
      <topicName>cmd_vel</topicName>
      
      <commandTopic>cmd_vel</commandTopic>
      <odometryTopic>odom</odometryTopic>
      <odometryFrame>odom</odometryFrame>
      
      <broadcastTF>true</broadcastTF>
   </plugin>
</gazebo>
```
(go through the parameters carefully like joint names and topic names, you will know their use later)  

- Now to **add** the gazebo launch lines in the launch file visualize.launch from the previous week. 
```xml
    <!-- This block of code is to call empty_world.launch file to fire up gazebo with
                empty world and then load world1 from our pkg -->
  <arg name="world" default="empty"/>
  <arg name="paused" default="false"/>
  <arg name="use_sim_time" default="true"/>
  <arg name="gui" default="true"/>
  <arg name="headless" default="false"/>
  <arg name="debug" default="false"/>
  
  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="$(find camp)/worlds/world1.world"/>
    <arg name="paused" value="$(arg paused)"/>
    <arg name="use_sim_time" value="$(arg use_sim_time)"/>
    <arg name="gui" value="$(arg gui)"/>
    <arg name="headless" value="$(arg headless)"/>
    <arg name="debug" value="$(arg debug)"/>
  </include>

<!-- Coordinates of bot which we need to spawn -->
    <arg name="x" default="0"/>
    <arg name="y" default="0"/>
    <arg name="z" default="0"/>
    <arg name="roll" value="0.0"/>
    <arg name="pitch" value="0.0"/>
    <arg name="yaw" value="1.57"/>
    
    <!-- Calling spawing node to spawn our robot in gazebo -->
   <node name="husky_spawn" pkg="gazebo_ros" type="spawn_model" output="screen" 
    args="-urdf -param robot_description -x $(arg x) -y $(arg y) -z $(arg z) -R $(arg roll) -P $(arg pitch) -Y $(arg yaw) -model husky" />
```
- Open up the bashrc in your home directory using
```bash
~/nano .bashrc
```
At the end add the following line to **speed up loading the world** (do this step only once and make sure the path you enter below is the path of the worlds folder on your system)
```bash
export GAZEBO_MODEL_PATH=~/catkin_ws/src/camp/worlds
```  

- Now launch the file

```bash
~/catkin_ws $ roslaunch camp visualize.launch
```


You can see gazebo window and a bot spawned in it. Something similar to this.

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-13-05.png?raw=true"/></p>
<br>  

### If (you can see robot spawned) :
      then go ahead.
### Else:

<p align="center">
    <img src="https://c.tenor.com/pPKOYQpTO8AAAAAM/monkey-developer.gif" width=500/><br><b>Debug Time</b>
</p>
<br>  


- Now fire up another terminal and run the following command in it

```bash
 rostopic pub -r 1 /cmd_vel geometry_msgs/Twist "linear:
  x: 1.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0"
```
(PS: Use double tab after /cmd_vel to autocomplete)   

Now you can see the bot moving forward, similarly you can make it turn with the angular velocity z parameter. Go on and play.  
You can also try out the teleop_twist_keyboard with this 
```bash
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```  
Note here that **cmd_vel** is the **topic** to which you are **publishing**, this value is intercepted by the skid_steer_plugin which does everything for you.  
In teleop_twist_keyboard, its the same, you publish to the cmd_vel topic, so if the urdf has some other topic name it won't work without changing the topic name in teleop_twist_keyboard.py.

- Now we will be adding the camera sensor, add these lines to the urdf: (replace camera_link with the name of your camera link name)
```xml
<gazebo reference="camera_link">
    <sensor type="depth" name="intelrealsenseD435i">
      <always_on>1</always_on>
      <visualize>true</visualize>
      <camera>
        <horizontal_fov>1.57</horizontal_fov>
        <image>
          <width>640</width>
          <height>480</height>
          <format>R8G8B8</format>
        </image>
        <depth_camera>

        </depth_camera>
        <clip>
          <near>0.1</near>
          <far>100</far>
        </clip>
      </camera>
      <plugin name="intelrealsense" filename="libgazebo_ros_openni_kinect.so">
        <alwaysOn>true</alwaysOn>
        <updateRate>10.0</updateRate>
        <cameraName>camera</cameraName>
        <frameName>camera_depth_frame</frameName>
        <imageTopicName>/camera/color/image_raw</imageTopicName>
        <depthImageTopicName>/camera/depth/image_rect_raw</depthImageTopicName>
        <pointCloudTopicName>depth/points</pointCloudTopicName>
        <cameraInfoTopicName>/camera/color/camera_info</cameraInfoTopicName>
        <depthImageCameraInfoTopicName>/camera/depth/camera_info</depthImageCameraInfoTopicName>
        <pointCloudCutoff>0.2</pointCloudCutoff>
         <pointCloudCutoffMax>10.0</pointCloudCutoffMax>
        <hackBaseline>0.07</hackBaseline>
        <distortionK1>0.0</distortionK1>
        <distortionK2>0.0</distortionK2>
        <distortionK3>0.0</distortionK3>
        <distortionT1>0.0</distortionT1>
        <distortionT2>0.0</distortionT2>
        <CxPrime>0.0</CxPrime>
        <Cx>0.0</Cx>
        <Cy>0.0</Cy>
        <focalLength>0.0</focalLength>
      </plugin>
    </sensor>
  </gazebo>
```

- Thats it our urdf is ready, now get ready to visualize everything. Excited??
- Launch visualize.launch again and in Rviz using the add by topic option add the **image** topic and **Odometry**. Also add the axes from display type and change fixed frame to husky_robot_model__base_link.  
(Again note odom topic was used in the urdf skid_steer_drive plugin :wink:)  

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-14-31.png?raw=true"/><br><br>  <br><br>
    <img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-14-11.png?raw=true"/></p>  
<br>



### Getting feed from the Camera

Now after having the camera spawned and getting its data published on a topic we can access this in our code.
- Use **cv_bridge** for accessing images from a camera. The camera feed is going straight to the topic /camera/color/image_raw (this name comes from the urdf under camera plugin :wink:).  
Try rostopic echo on it and you will see the image pixel values in a weird encoded format. Now this is a ROS msg, we have to convert this to an Opencv image. Relax, the cv_bridge package has got you covered. Install it using (inside the **src** folder of your **workspace**):

```bash
git clone https://github.com/ros-perception/vision_opencv
```
- Now to do the conversion:
This depends on you the language you prefer. (Do python first :wink:)
for cpp you have tutorial here [[c++]](http://wiki.ros.org/cv_bridge/Tutorials/UsingCvBridgeToConvertBetweenROSImagesAndOpenCVImages) and for [[python]](http://wiki.ros.org/cv_bridge/Tutorials/ConvertingBetweenROSImagesAndOpenCVImagesPython)  

Using this you can easily access the camera image and apply opencv functions on it.

### Controlling the robot using OpenCV

We can roughly control the robot with the odomtery data from the odom topic which gives the xyz position,orientation and twist velocity.  
  
**Note: This is just a good model in a simulator where you have exact value about the angle but in real world using odometry data is very erroneous as the encoder sensor readings keep drifting over time.**

## Task-Time
This is a task very similar to what you did in LaRoboLiga.  
- In your launch file, launch the box_world.world. 
- Find the blue box, take husky to it and stop a few centimetres before the box.
- Submit a screen recording of husky completing the task.  

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-18-33.png"/><br><b>Enjoy!!!</b> :wink: </p>

