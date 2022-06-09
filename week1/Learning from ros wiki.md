# Task 1 for ROS Specialization

Lets Start out with Linux commands go through the compilation we have made here [LinuxCommands.md](LinuxCommands.md)

Thats should be all for your Linux Tutorial Now back to topic : )

## ROS Communication

This week you will basically be following the official tutorials 1-15 by [ROS wiki here](http://wiki.ros.org/ROS/Tutorials) Beginner Tutorial.  
We assume that you have already installed ROS and created a catkin workspace and don't forget to add these lines to your home .bashrc.
```bash
source /opt/ros/noetic/setup.bash
source ~/catkin_ws/devel/setup.bash
```

We will now start with creating a package. All our work will be contained inside this package.

### Create package
Follow this [Tutorial](http://wiki.ros.org/ROS/Tutorials/CreatingPackage).

Execute this command inside the **src folder** of your catkin workspace
```bash
~/catkin_ws/src $ caktin_create_pkg camp roscpp rospy tf2 ## camp is the package name and roscpp rospy tf2 are basic necessary dependencies  
```
should create a folder

         ├── catkin_ws
             └── src/
                 ├── camp/               ## package
                 |    ├── include/camp/  ## Contains header files
                 |    ├── src/           ## Contains source files or cpp files
                 |    ├── package.xml    ## Definition of package
                 |    └── CMakeLists.txt ## Definition for building and compiling your cpp files contains all dependencies
                 |   ...
                 └── CMakeLists.txt
                 
Now go back to catkin_ws directory and execute
```bash
~/catkin_ws $ catkin build
~/catkin_ws $ source catkin_ws/devel/setup.bash  ## when new packages are created it is necesaary to source the workspace again
```

Now one can also access this pkg from anywhere using ros commands
```bash
$ roscd camp
```

To know what the CMakeLists.txt and package.xml does check this out [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt) and [package.xml](http://wiki.ros.org/catkin/package.xml)  
These will always be auto-generated but if you are using additonal dependencies/compiling cpp files you will have to make some changes in them as shown in the below tutorials.  
<br>
<br>
Now there are just **Seven** basic important things to learn and you will never struggle thereafter
- Nodes
- Topics
- Services
- Messages and Srv files
- Publisher
- Subscriber
- Launch files
<br>  

### Nodes,Topics and Services

![topics](Nodes-TopicandService.gif)

Basically there are central nodes called **master** that collects all the new messages and distributes them within nodes. Basically **Messages** are feed or reading from sensors or useful information from some device/program called **Nodes**.<br/>
**Services** compared to messages just doesnt collect or send to one node but a trasaction of information takes place. Basically it is so that a node asks the other node to do something (could be to solve a mathematical problem, or Move the motors or turn on siren) And after that is completed the other node sends a confirmation or results. This Forward and backward motion of Information is termed as **Request** (one who asks for something) and **Response** (after processing the request).

For understanding them better we recommend you to follow the Tutorial by ROS (follow the steps and play with the simulation):
- Nodes [Tutorial](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes)
- Topics [Tutorial](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics)
- Services [Tutorial](http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams)
  
<br>  

### Messages and Srv files
These Topics and  Services have there own Data type called `msg` and `srv` to handle the communication effectively and robustly
these are similar to your definition of `struct`
```cpp
// structdef.h
struct {
  int id;
  string name;
  float vals[];
};
```
Similarly an example campmsg.msg file looks like this
```bash
## campmsg.msg
int64 id
string name
float64 val[]
```

Same goes for `srv` files it is very similar to a msg file and contains the input and output argument definitions of the service.
Example campsrv.srv file
```bash
## campsrv.srv
int64 a
int64 b
---
int64 sum
```

These messages are sent and recieved inside individual nodes using a mechanism called as **Publisher** which sends message to the **ros master**, And **Subscriber** which subscribes the message from master in a **Asynchronous** fashion using callbacks.  

The publisher publishes a value on a topic which is intercepted by an infinite loop in the subscriber and a callback function is then executed every time a message is received. For understanding them check out the tutorials. 
  
  
**(Try to do the tutorials both in C++ and Python. Its easier to write the code in Python but for real applications C++ runs faster and is lightweight)**.

- Msg and Srv [Tutorial](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv)
- Publisher and Subscriber [Tutorial](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) and [Run](http://wiki.ros.org/ROS/Tutorials/ExaminingPublisherSubscriber)
- Service Server and Client [Tutorial](http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29) and [Run](http://wiki.ros.org/ROS/Tutorials/ExaminingServiceClient)  
<br>  

### Launch
Basically all the nodes/scripts that you write are spawned by executing them but what if there are many such nodes to launch ? 
  

It will be too cumbersome to open so many terminals and execute them one by one. Instead we use what is called a **launch file**.  
This will automate spawning each node and now you will just be executing one command.

You can directly checkout its implementation here. [Tutorial](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#Using_roslaunch)
