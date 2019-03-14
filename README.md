# config-tomcat-EC2-instance

This tutorial introduces how to install tomcat 8.51 on AWS Ubuntu instance 14.04.

Start your AWS instance, and connect to it using SSH.
For this step, please refer to the previous tutorial on how to use AWS.

**Download Tomcat**

You can find latest version of tomcat 8.5.x from **http://tomcat.apache.org/download-80.cgi**. For example, the download link for tomcat 8.5.24 is 
**http://apache.mirrors.pair.com/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz**

Then use wget on your AWS machine to download it:

**wget http://apache.mirrors.pair.com/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz**

**For tomcat 9 url: http://apache.mirrors.pair.com/tomcat/tomcat-9/v9.0.16/bin/apache-tomcat-9.0.16.tar.gz**


After you download the tar.gz, decompress it to a desired destination (for example, I use /home/ubuntu/tomcat)


**mkdir /home/ubuntu/tomcat**

**tar xvf apache-tomcat-8.5.24.tar.gz -C /home/ubuntu/tomcat/ --strip-components=1**


**Configure Tomcat Memory**

By default, the maximum heap available for Tomcat may not be enough for large projects. Thus, we need to change the memory configuration by the following steps.

Go to **/home/ubuntu/tomcat/bin/**, and create a text file name setenv.sh, which contains the following lines:

**#!/bin/sh**

**env JAVA_OPTS="-Xmx512m -Xms128m"**

Xmx means the maximum heap memory available for Tomcat, and Xms means the initial memory available after each garbage collection. You may vary them based on your machine capacity.

Change Access Setting of host-manager and manager
host-manager and manager are two useful default applications provided by Tomcat to manage the Tomcat server. However, by default they can be accessed by only the machine hosting tomcat, i.e., localhost.

To change this, you need to change **/home/ubuntu/tomcat/webapps/host-manager/META-INF/context.xml** and **/home/ubuntu/tomcat/webapps/manager/META-INF/context.xml** by deleting the following lines:

      <Valve className="org.apache.catalina.valves.RemoteAddrValve" 

            allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
      
**Create a Tomcat User**

Now, create a tomcat user so that you can manage the tomcat server remotely.

Edit **/home/ubuntu/tomcat/conf/tomcat-users.xml**, and add the following line:


      <user username="tomcat" password="123456" roles="manager-gui,admin-gui"/>


By doing so, you've created a user named "tomcat", and its password is "123456". You've also granted two roles "manager-gui" and "admin-gui" to it.

**Start Tomcat**

Now you are ready to start tomcat by executing the following command:

**/home/ubuntu/tomcat/bin/startup.sh**

or go to **/home/ubuntu/tomcat/bin/**

**sh startup.sh**


If you see this, then it means your tomcat has started properly:

**_=/bin/env**

**JAVA_OPTS=-Xmx512m -Xms128m**

**Using CATALINA_BASE:   /home/ec2-user/tomcat**

**Using CATALINA_HOME:   /home/ec2-user/tomcat**

**Using CATALINA_TMPDIR: /home/ec2-user/tomcat/temp**

**Using JRE_HOME:        /usr/lib/jvm/jre**

**Using CLASSPATH:       /home/ec2-user/tomcat/bin/bootstrap.jar:/home/ec2-user/tomcat/bin/tomcat-juli.jar**

**Tomcat started.**

To access tomcat server you should use your EC2 instance public DNS or public IP with port number (8080)

**T**his tutorial is not created by me and all the credit goes to the guy who wrote this. I followed and edited this tutorial 
for my own configuration so i can go back to take a look at it after a while.

[UCI ICS](https://grape.ics.uci.edu/wiki/public/wiki/cs122b-2017-winter-project1-install-tomcat-on-aws#no1)

