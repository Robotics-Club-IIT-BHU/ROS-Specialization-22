# Subpart-1 

<h2 align="center">RViz</h2>

**Visualisation** is the key which make robotics fun, Generally we want to see what the robot is seeing, how its joints, links are behaving while interacting with environment etc.... so for these purposes ROS has a package RViz(known as ROS Visualiser) to allow us to see and visualise all these things.

It allows us to....
 - See the image from the camera mounted on robot or in a world.
 - Visulaise and debug URDFs.
 - Odometry and Mapping of the environment like lidar maps.
 - set Markers for plotting out trajectories
 - visulaise transformations 
 - and so on.......

Before moving further, see these Videos for Rviz Introduction and feel



[Rviz Introduction by The Construct](https://www.youtube.com/watch?v=yLwr5Zhr_t8&t=137s)<br>
[More indepth usage](https://www.youtube.com/watch?v=6pep5xB4pEU)


Don't Worry if you don't understand few terms, you will learn about most of them as the camp progress. 
If you are unable to understand how we represent two frames and how we translate between them see [Simple Example](#simple_examples) section first
<br>

Before moving further with RViz, let's first learn about **tf or transforms(transformations)**.

<h2 align="center">Tf</h2>

In Pybullet camp, you found an example of transformation i.e., **euler2quaternion** or **quaternion2euler**, but robotics is not just limited to it. 

In ROS, there is a separate package which handles transformations, but one might be thinking...

<p align="center">
<img width = 500 height=300 src = "https://github.com/Robotics-Club-IIT-BHU/ROS-SummerCamp21/blob/final/assests/ask.jpeg?raw=true">
</p>

I will leave the answer on [Kostas Danillidis-Proff at University of Pennsylvania](https://www.coursera.org/lecture/robotics-perception/rotations-and-translations-eTSMz)

In the video you can find how different transformations need to be considered just for a single camera. Now add to that the plethora of sensors you can use on your robot like LaserScan, Encoders, GPS, IMU and so on. 
<br>

ROS Tf package is an abstraction which handles all transformations for you at all times. 

To learn more thoroughly about it Refer [ROS-Wiki/tf](http://wiki.ros.org/tf).

Also you can look about RViz from [ROS-Wiki/RViz](http://wiki.ros.org/rviz)(Although i think it will be hard for some of you to understand it directly from wiki for now..).

Basically it helps you by providing a transformation (both rotation and translational) between two frame of references. 


## Simple examples

Let me give an example to which you can relate, take the husky robot you had in Pixelate and LaRoboLiga and just as in Pixelate imagine there is an overhead camera. 
<br>
<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/patch-1/week2/media/Arena.png" width="50%"/></p>  
<br> 

Now imagine the camera to have one frame of reference and the husky robot to have a frame of reference **attached** to them.  
With the help of ArUco markers you managed to get the **relative translation** in the **XY directions** and the **relative orientations** in the **Yaw**. 
<br>  
So congrats you already know the root basics of Transformations!!!.
<br>  

Now you might be thinking *ohh that was so easy*, but wait. Add translation in the Z axis along with orientation change in the Roll and Pitch too and things start getting complicated.

This can still be done by hand with the help of some maths involving matrices. 
<br>  
Check out these three videos part of a series for an in-depth understanding: &emsp; [Part-1](https://www.youtube.com/watch?v=xYQpeKYCfGs&t=1s) &emsp; [Part-2](https://www.youtube.com/watch?v=HOv2DTaw38U) &emsp; [Part-3](https://www.youtube.com/watch?v=_B3KsLkX_3A&t=2s).  

<br>

**Complex Scenario**  
- Now lets consider a complex case where the point of references are not stationary like in the robot given below.
- Lets say we wanted to climb stairs, and we have the coordinates of the stairs from the Center of Mass of the robot.
- If we have the task to keep only the front most leg on the stairs, We would need the coordinate of the stairs from the frame of reference of the toe.  
- But this time the rotation and translation matrix is not constant like the previous examples as the position of the toe is changing relative to the Center of mass as the joint angles are changing. 
- So keeping this in mind we have first compute the forward Kinematics of the toes and then compute the translation and rotation matrix which is very hard considering your system may have multiple joints and multiple end effectors.
<br>

**Sounds scary?? Chill, the Tf package has got you covered.**  
<br> 

<p align ="center">
<img src = "https://github.com/Robotics-Club-IIT-BHU/ROS-SummerCamp21/blob/final/Task2/Subpart1/stoch.png"><br/>
</p>

## HANDS ON

Time to get your hands dirty, clean rather coz the Tf package does all the dirty work for you. :)    

Run below Packages Step by Step to get the flavour of Rviz and tf.

P.S. -> We are not going indepth of the underlying code(As our aim is to give you a headstart on how these are useful and not telling about every knitty-gritty details), if one is willing he/she can go through it for better understanding

 - [Learning Urdf Step by Step](http://wiki.ros.org/urdf/Tutorials) - Here you will learn how to visualise your Urdfs in Rviz. **Just Follow the steps as mentioned there and try to Run locally-> Learning URDF Step by Step Tutorial for first three section till now(Upto- Adding Physical and Collision Properties to a URDF Model).**
   
 - TF from ROS wiki [[1]](http://wiki.ros.org/tf/Tutorials/Introduction%20to%20tf) , [[2]](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28C%2B%2B%29), [[3]](http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28C%2B%2B%29), [[4]](http://wiki.ros.org/tf/Tutorials/tf%20and%20Time%20%28C%2B%2B%29),, [[5]](http://wiki.ros.org/tf/Tutorials/Time%20travel%20with%20tf%20%28C%2B%2B%29) Go through all these tutorials to get a good understanding of tf
       

## Task for this Subpart

From Pybullet camp, you are familiar with building urdfs(assuming only static models). In this task: 
- Download the folder [camp](https://github.com/san2130/ROS-Specialization-22/tree/patch-1/week2/camp) and inside this package you will see three folders launch, models, worlds.  
- Build the package and resolve the errors you might get. :)
- Now open up the models folder and under husky_robot_model you will find model.urdf, this is the urdf file you will be tweaking.  
- To visualize it run the visualize.launch file located in the launch folder using
``` 
roslaunch camp visualize.launch
```
(camp is the name of the package)  
- When Rviz opens up, change the fixed frame to husky_robot_model__base_link as this is the base link of the robot.  
- Now add the RobotModel using the Add button and now you should see be able to see the robot.  
<br>

### What you have to do (Task 1):  
- Study the launch file first and get an idea of what is happening.  
- Now add a **fixed box** to the top of the rod which will serve as the camera, by adding a new link and joint in the urdf file.  
<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/patch-1/week2/media/Screenshot%20from%202022-07-09%2015-35-19.png" width="100%"><br><i>Something like this</i></p>

- Add the Tf modal using the Add button to see the frames as shown in the above picture.  
- Now instead of a fixed joint use a **revolute** joint to attach the box.  
- To check the movement of the joint go to the launch file and replace the joint_state_publisher line with this
 ``` 
 <node pkg="joint_state_publisher_gui" type="joint_state_publisher_gui" name="joint_state_publisher_gui" output="screen" />
 ```
- Relaunch Rviz and now you will see a gui panel with sliders using which you can check out your joint movements.
<p align="center"><img src="https://github.com/san2130/ROS-Specialization-22/blob/patch-1/week2/media/Screenshot%20from%202022-07-09%2015-44-14.png" width="100%"><br><i>joint_publisher_gui</i></p>
<br>

### Submission Instructions  
- The modified Urdf file.
- A small screen recording of the camera box revolute joint moving using the joint_publisher_gui. 

### Useful Info
- [ ] You can use this link for reference [Visualising urdf tutorial from ROS for your urdf](http://wiki.ros.org/urdf/Tutorials/Building%20a%20Visual%20Robot%20Model%20with%20URDF%20from%20Scratch).  
- [ ] Don't get confused between robot_state_publisher and joint_state_publisher. **robot_state_publisher** is an inbuilt node which sets up the **TF tree of the urdf** for you while using **joint_state_publisher** you can **publish joint values** to the joints.
- [ ] Parameter "robot_description" stores the location of the urdf to load. There can only be one robot_description that can be viewed in Rviz at a time.

Happy `build`ing!!
