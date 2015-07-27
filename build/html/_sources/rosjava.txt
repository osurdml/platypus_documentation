Implementation of ROS Java on the Sami-Crw Software
===================================================

First ensure you are running the Java JDK 8 on Netbeans. The following instructions are assuming the original software package (from: https://github.com/nbbrooks ) is successfully installed and compiled on Netbeans (per the installation tutorial).

The next step is to import into Netbeans as a new project the revised code with the ROSJava functionality added to it. This code can be found on https://github.com/nrjl/sami-crw/tree/ROScomms.

Installing ROSJava
------------------

Install ROSJava onto the host computer. The lab currently uses ROS Indigo on Ubuntu 14.04. If you do not have ROS, you must first install it in order to get ROSJava. See: http://wiki.ros.org/indigo/Installation/Ubuntu for the ROS installation instructions. 

Once you have ROS installed, or have checked that it was installed previously and you are running Indigo, you may now install ROSJava. See: http://wiki.ros.org/rosjava/Tutorials/indigo/Installation for instructions for installing ROSJava. Select the “Source” install option, as this will later allow you to import ROSJava packages into Netbeans.

Importing Packages into Netbeans
--------------------------------

In Netbeans, in the sami-crw folder in the ‘Project’ tab, select ‘Libraries’. Right click on ‘Libraries’ and select ‘ADD JAR/Folder..’. This will open to a new screen where you can select which JAR files to import to Netbeans. Navigate to the folder where you have stored all ROSJava files. 
The folder which will contain the necessary JAR files for the standard ROSJava edition is:
/rosjava/src/rocon_rosjava_core/master_info/build/install/master_info/lib

Import all the JAR files in this folder:

- actionlib_msgs-1.11.8.jar 
- apache_xmlrpc_client-0.2.1.jar 
- apache_xmlrpc_common-0.2.1.jar
- apache_xmlrpc_server-0.2.1.jar
- commons-pool-1.6.jar
- com.springsource.org.apache.commons.codec-1.3.0.jar
- com.springsource.org.apache.commons.httpclient-3.1.0.jar
- com.springsource.org.apache.commons.io-1.4.0.jar
- com.springsource.org.apache.commons.lang-2.4.0.jar
- com.springsource.org.apache.commons.logging-1.1.1.jar
- com.springsource.org.apache.commons.net-2.0.0.jar
- dnsjava-2.1.1.jar
- geometry_msgs-1.11.8.jar
- gradle_plugins-0.2.1.jar
- guava-12.0.jar
- jsr305-1.3.9.jar
- junit-3.8.2.jar
- master_info-0.2.0.jar
- message_generation-0.2.1.jar
- nav_msgs-1.11.8.jar
- netty-3.5.2.Final.jar
- rocon_service_pair_msgs-0.7.10.jar
- rocon_std_msgs-0.7.10.jar
- rosgraph_msgs-1.11.1.jar
- rosjava-0.2.1.jar
- rosjava_test_msgs-0.2.1.jar
- rosjava_utils-0.2.0.jar
- std_msgs-0.5.9.jar
- tf2_msgs-0.5.11.jar
- uuid_msgs-1.0.5.jar
- ws-commons-util-1.0.1.jar
- xml-apis-1.0.b2.jar

Our code utilizes a message package, which does not come automatically with the standard ROSJava installation. You must retrieve this package in order to get the geography messages JAR into Netbeans.

First to get the package, in a new terminal window, run::

$ sudo apt-get install ros-indigo-geographic-info

Then you need to generate the rosjava versions of those messages. You need genjava::

$ sudo apt-get install ros-indigo-genjava

To publish a waypoint you need to first load the messages by sourcing the java setup bash::

$ source ~/rosjava/devel/setup.bash

Then naviage to rosjava folder (wherever it is on your computer)::

$ cd ~/rosjava
$ genjava_message_artifacts --verbose

That should make a bunch of messages.

To import the geography messages JAR into Netbeans, following the same method as before, in the sami-crw folder in the ‘Project’ tab, select ‘Libraries’. Right click on ‘Libraries’ and select ‘ADD JAR/Folder..’. This will open to a new screen where you can select which JAR files to import to Netbeans. Then navigate to the following:
/rosjava/build/geographic_msgs/build/libs

Import the following JAR file: 

- geographic_msgs-0.4.0.jar

Test the ROSJava coupled Sami-crw software
------------------------------------------

To test the functionality of the code, run the application in Netbeans. MAKE SURE THAT A ROSCORE IS RUNNING IN A TERMINAL WINDOW. (To run a roscore, simply open a new terminal window and type “roscore” at the command line.)


Once the application has launched and you have the roscore running in the terminal, in the Mission Monitor, select the “Add Simulated Boat” plan and hit the “Run” button. Add a SINGLE simulated boat at any point on the map of Waverly Lake. NOTE: You must make sure to change the “Number of boats” field to 1 from the default of 3. 

First check that the Publisher and Subscriber node have been declared and instantiated. To do this, open a new tab in terminal and run: rostopic list
You should see the following:
/crw_waypoint_sub
/crw_geopose_pub
/clear_waypoints
/rosout
/rousout_agg

Then try in a terminal window::

  $ rostopic pub -1 /crw_waypoint_sub geographic_msgs/GeoPose 
  '{position: {latitude: 44.642, longitude: -123.065, altitude: 0.0}, orientation: 
  {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}'


This should publish 1 message of type GeoPose to the /crw_waypoint_sub topic.

To clear this waypoint, type the following::

$ rostopic pub -1 /clear_waypoints std_msgs/String ‘clear’

This will publish 1 message of type String to the /clear_waypoints topic. It will stop the boat on its course to the current waypoint.
