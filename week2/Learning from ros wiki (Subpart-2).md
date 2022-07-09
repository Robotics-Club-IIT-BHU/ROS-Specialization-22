# Subpart-2  

```cpp
if("completed Subpart-1")
    cout<<"HURRAY enter Subpart-2"<<"\n";
else
    cout<<"Go back to Subpart-1 and complete it first"<<"\n";
```  

<br>  

This task will test your understanding of Transformations so feel free to refer back to the resources given for that. 
 

<h2 align="center">Welcome to RQT</h2>

RQt runs in the same way as a node.


```bash
# start roscore 
~$ roscore
```
```bash
# in another terminal run
-$ rqt
```
A similar window of this type will open
<img src = "https://github.com/Robotics-Club-IIT-BHU/ROS-SummerCamp21/blob/final/assests/rqt.png?raw=true">

RQT have different components varying from viewing node Graphs, logging errors/warnings, plotting variables, viewing images from camera on bot and other things. **We will go through three of them rqt_graph, rqt_plot, rqt_logging one by one**.

### 1. rqt_graph

Rqt graph is a GUI plugin from the Rqt tool suite. With rqt graph you can visualize the ROS graph of your application. On one window you can see all your running nodes, as well as the communication between them. The nodes and topics will be displayed inside their namespace.

**Why using rqt graph**?

When you develop with ROS you usually organize your work into packages and nodes. As your application grows (more sensors, more actuators, more ways to control your robot, …), so does your code base. So, you end up with many nodes and topics, and it might become harder to debug. Using rqt graph will help you mostly for those two things:

 - You’ll get a global overview of your system. This is really useful so you can take better decisions for the future new parts of your application.
 - When you have a bug somewhere due to communication between nodes, you will be able to easily spot the problem. Maybe a node is not correctly connected to another, or there are 2 nodes publishing on a given topic instead of just one, which is why you get some weird values on the subscriber side.
  
<p align = "center">
<img width = 500 height = 200 src = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTcjlPVOXaSmC4IDTvmhoGsicCVTpHrrnVbvA&usqp=CAU"><br/>
<b>An example of rqt_graph</b></br>
(Here Red one is the topic connecting publisher and subscriber)
</p>

Follow [this tutorial from Wiki](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics) as it explained very neatly about rqt_graph.

### 2. rqt_plot

Yes the word describes it right, we can use this to plot the data from the meassage on a particular topic. 
For example below plot describes the x and y coordination of a bot varies with time.

<p align="center">
<img width = 600 height = 400 src = "https://www.programmersought.com/images/386/21b565afa70361bca5cca940ba31e8ca.png">
</p>

**Why it is used**?
 - It can be used to see the values of a variable and compare it with theoretical ones.
 - Can be used to compare the values coming from sensor and your estimation of that value.
 - And more importantly it allows you to argue about the behaviour of your robots in real time and can allow us to identify any errors(if present).

We can provide you with a demonstration of rqt_plot using turtlesim package available in ROS, but [this article](https://roboticsbackend.com/rqt-plot-easily-debug-ros-topics/) has presented in an intutive way and reduces our headache :sweat_smile:

## Its Getting Boring ???
<p align="center">
<img src = "https://github.com/Robotics-Club-IIT-BHU/ROS-SummerCamp21/raw/final/Task2/Subpart2/node.gif?raw=true">
    
Well i know it is but you are now done with boring theory and can jump on using these things with your own robot!!!! 
</p>

 **We again have a task for you on what you just learnt!! Head towards the [Task_2](https://github.com/harshmahesheka/ROS-Specialization-22/blob/main/week2/Task_2.md) Page.**