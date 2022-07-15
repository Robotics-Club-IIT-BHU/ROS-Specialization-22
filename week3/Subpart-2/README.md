# Subpart-2
<h2 align="center"> Navigation Stack </h2>

## Follow-along Tasks

- We will be using the same package as in Subpart-1 and we will implement the ROS navigation stack on our husky.  
- First we have to add the Lidar sensor to our husky. 
```xml
<gazebo reference="laser">
    <sensor type="ray" name="head_rplidar_sensor">
      <pose>0.424 0 0.327 0 0 0</pose>
      <visualize>true</visualize>
      <update_rate>40</update_rate>
      <ray>
        <scan>
          <horizontal>
            <samples>720</samples>
            <resolution>1</resolution>
            <min_angle>-2.35619</min_angle>
            <max_angle>2.35619</max_angle>
          </horizontal>
        </scan>
        <range>
          <min>0.5</min>
          <max>20.0</max>
          <resolution>0.01</resolution>
        </range>
        <noise>
          <type>gaussian</type>
          <mean>0.0</mean>
          <stddev>0.01</stddev>
        </noise>
      </ray>
      <plugin name="gazebo_ros_head_rplidar_controller" filename="libgazebo_ros_laser.so">
        <topicName>/laser/scan</topicName>
        <frameName>laser</frameName>
      </plugin>
    </sensor>
  </gazebo>
```
(this publishes laser scan data to /laser/scan topic and is bound to the "laser" link)  

- Now launch visualize.launch again and add the LaserScan topic under the add by topic option.

```bash
~/catkin_ws $ roslaunch camp visualize.launch
```

Now in gazebo you will see a blue 3 quarter circle. This is the laser rays projecting out from the lidar.   

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-39-46.png"/><br><br></p>

And in Rviz you will see red dots, these are the objects reflecting back the laser rays. The red line is the wall in front.

<p align="center"> <br> <img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-40-30.png"/>
<br>  

### If (you are seeing something similar) :
      then go ahead.
### Else:

<p align="center">
    <img src="https://c.tenor.com/pPKOYQpTO8AAAAAM/monkey-developer.gif" width=500/><br><b>Debug Time</b>
</p>
<br>  

## What is this ROS Navigation stack??  
Here's what ROS wiki [says](http://wiki.ros.org/navigation).  
Let me say the same thing to you in very simple terms. 
- Basically the nav stack is just another [package](https://github.com/ros-planning/navigation) 
- Behind controlling every robot there are two things:
   1. Localization (estimate the position of the robot in its surrounding w.r.t to some global frame) with the help of various sensors like camera, Lidar, IMU, encoders(odometry)
   2. A path planner that claculates the path to follow form current position to the goal.
   3. A controller that gives the appropriate torque command to the wheels for the bot to follow its path.
- The nav stack package does all of this for you. It takes in the Lidar scans and the odometry data and processes them all the time to constantly localize the bot.  
- It also has pre-written path planners like NavFn, TEB and DWA. 
- Further it also gives the final appropriate torque command to all the joints for the robot to follow the above path using the move_base package.  

## Task-Time
This is a task very similar to what you did in LaRoboLiga.  
- In your launch file, launch the box_world.world. 
- Find the blue box, take husky to it and stop a few centimetres before the box.
- Submit a screen recording of husky completing the task.  

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-18-33.png"/><br><b>Enjoy!!!</b> :wink: </p>
