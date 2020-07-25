# ariitk_rtps_bridge

Note:
For ROS2 Dashing the following versions are to be installed:

Fast-RTPS version 1.8.2

fastrtpsgen 1.0.4

## ROS2 Docker Setup
Set up the Docker environment with ROS2 Dashing as mentioned in the gist:
https://gist.github.com/amartyadash/49c1fe5eb98b710d152ccddec13a090b

Update and upgrade the environment:
```bash
sudo apt-get update
sudo apt-get upgrade
```
Install some necessary packages we would be using later:
```bash
sudo apt-get install zip wget
```
## Setup the Workspace 

Follow the official ROS2 docs for setting up the environment & doing the first build with the example of turtlesim

https://index.ros.org/doc/ros2/Tutorials/Workspace/Creating-A-Workspace/

Note: the tutorial will create the dev_ws in the /root directory.

## Installing JDK8, Gradle
Java and gradle are dependencies for Fast-RTPS & Fast-RTPS-Gen. 
### JDK 8
The official documentation recommends JDK8. We can install that as follows:

```bash
sudo apt install openjdk-8-jdk openjdk-8-jre
```
Verify the installation:
```bash
java -version

openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1ubuntu1-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
```
Setup JAVA_HOME and JRE_HOME variables:
```
cat >> /etc/environment <<EOL
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
EOL
```
### Gradle
The current Gradle release is version 6.5.1, released on 30 Jun 2020. Unzip the distribution zip file in the directory of your choosing,
```bash
cd ~
wget https://services.gradle.org/distributions/gradle-6.5.1-bin.zip
mkdir /opt/gradle
unzip -d /opt/gradle gradle-6.5.1-bin.zip
ls /opt/gradle/gradle-6.5.1
LICENSE  NOTICE  bin  getting-started.html  init.d  lib  media
```
Configure your `PATH` environment variable to include the bin directory of the unzipped distribution:
```bash
export PATH=$PATH:/opt/gradle/gradle-6.5.1/bin
```
Verify installation
```bash
gradle -v

------------------------------------------------------------
Gradle 6.5.1
------------------------------------------------------------
```
## Install Fast-RTPS & Fast-RTPS-Gen
These have to be installed following this: https://dev.px4.io/master/en/setup/fast-rtps-installation.html

For Fast-RTPS:
```bash
cd ~
git clone --recursive https://github.com/eProsima/Fast-RTPS.git -b 1.8.x ~/FastRTPS-1.8.2
cd ~/FastRTPS-1.8.2
mkdir build && cd build
cmake -DTHIRDPARTY=ON -DSECURITY=ON ..
make
sudo make install
```
For Fast-RTPS-Gen:
```bash
cd ~
git clone --recursive https://github.com/eProsima/Fast-RTPS-Gen.git -b v1.0.4 ~/Fast-RTPS-Gen
cd ~/Fast-RTPS-Gen
gradle assemble \
gradle install
```
Check Fast-RTPS-Gen installation:
```bash
fastrtpsgen -version

openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~18.04-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
fastrtpsgen version 1.0.4
```
## px4_ros_com
Clone both `px4_ros-com` and `px4_msgs`
```bash
git clone https://github.com/PX4/px4_ros_com.git ~/dev_ws/src/px4_ros_com # clones the master branch
git clone https://github.com/PX4/px4_msgs.git ~/dev_ws/src/px4_msgs
```
Building the workspace
```bash
cd ~/dev_ws/src/px4_ros_com/scripts
./build_ros2_workspace.bash
```
