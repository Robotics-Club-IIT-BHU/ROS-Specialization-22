<h1 align="center"> Subpart-2 </h1><br>

# Navigation Stack

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

Now in gazebo you will see a blue 3 quarter circle. These are the laser rays projecting out from the lidar.   

<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-39-46.png"/><br><br></p>

And in Rviz you will see red dots, these are the objects reflecting back the laser rays. The red line is the wall in front.

<p align="center"> <br> <img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-16%2003-40-30.png"/>
<br><br>  

### If (you are seeing something similar) :
      then go ahead.
### Else:

<p align="center">
    <img src="https://c.tenor.com/pPKOYQpTO8AAAAAM/monkey-developer.gif" width=500/><br><b>Debug Time</b>
</p>
<br>  

## What is this ROS Navigation stack??  
Here's what ROS wiki [says](http://wiki.ros.org/navigation).  
Let me say the same thing to you in absolute layman terms. 
- Basically the nav stack is just another [package](https://github.com/ros-planning/navigation) 
- Behind controlling every robot in a mapped environment, there are 3 things:
   1. **Localization** (estimate the position of the robot in its surrounding w.r.t to some global frame) with the help of various sensors like camera, Lidar, IMU, encoders(odometry)
   2. **A path planner** that claculates the path to follow from current position to the goal.
   3. A **controller** that gives the appropriate torque command to the wheels for the bot to follow its path.
- The nav stack package does all of this for you. It takes in the Lidar scans and the odometry data and processes them all the time to constantly localize the bot.  
- It also has pre-written path planners like NavFn, TEB and DWA. 
- Further it also gives the final appropriate torque command to all the joints for the robot to follow the above path using the move_base package.  
- This is what you will be able to do after setting up nav stack. [working](https://www.youtube.com/watch?v=V32rff0pQy4)
- Basially once you have a map, you can command the bot to go anywhere in the map and it will take the **shortest path without any obstacles** and reach the goal.

## Task-Time
- We want you to setup the nav stack on husky by yourself, the only way to be confident in it. :smiley: 
- This is the nav stack [package](https://github.com/ros-planning/navigation) which you can clone or install it using sudo.  
- First you will have to **map the environment** using the [slam_gmapping](http://wiki.ros.org/gmapping) package and save the map created. 
- Then all you have to do is create a launch file and then launch [amcl](http://wiki.ros.org/amcl) for **localization** and [move_base](http://wiki.ros.org/move_base) for **movement**, which takes a global planner and a local planner(try out different planners and choose the best) as inputs and numerous other tunable parameters.  
- Now since the nav stack can be applied to any robot, there are a gazillion paramters you have to custom tune using hit-and-trial. It's time to learn **Dynamic_Reconfigure**.
<details>
    <summary><b>Dynamic Reconfigure</b></summary>
<br>
    <h2> Dynamic Reconfigure</h2>  
I guess you already know what this does - instead of manually tuning parameters ike PID constants for example, by stopping and restarting the simulation very time, you can directly change those parameters online, **on-the-fly**.  (Makes our life so so much easier :sweat_smile:)  
  
Fire up a terminal and open rqt  
```bash
rqt
```  
Now under the plugins tab -> Configuration -> Dynamic Reconfigure  
<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/main/week3/media/Screenshot%20from%202022-07-17%2002-01-24.png"/><br><i>Dynamic Reconfigure</i></p><br><br>

Now you should be able to see tunable nodes on the left, click on them and you will find all the tunable parameters with sliders.  

<p align="center"><img src="https://user-images.githubusercontent.com/6259829/62143462-50436500-b2f0-11e9-9812-6d105f8476bf.png"/></p><br><br>
</details>  

- If your implementation was correct, then on giving husky a goal using Rviz, it should reach it successfully. The better the tuning, the smoother the movement.    

## Submission instructions  
Give the husky a goal point within the map and record a video of husky reaching the goal. (upto whatever best you could achieve, partially working allowed)

**Phew!! That was a lot you learnt in the first two subparts. We are thrilled to see your hardwork and effort. :clap: :clap:** <br>
## Useful Resources  
- [The Construct](https://www.youtube.com/channel/UCt6Lag-vv25fTX3e11mVY1Q)  (Most useful channel for ROS, covers almost everytihng)
- [SLAM Map Building with TurtleBot](http://wiki.ros.org/turtlebot_navigation/Tutorials/Build%20a%20map%20with%20SLAM)
- [Autonomous Navigation of a Known Map with TurtleBot](http://wiki.ros.org/turtlebot_navigation/Tutorials/Autonomously%20navigate%20in%20a%20known%20map)
 
