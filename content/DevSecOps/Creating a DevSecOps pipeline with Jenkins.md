---
title: Creating a DevSecOps pipeline with Jenkins
tag: cybersecurity, devsecops
---

First of all, let's setup a Jenkins instance on our machine. Since it has a simple installation and using a container can have a negative impact on performance or storage, I installed Jenkins directly on my machine. I'm using Ubuntu 22.04 on VMware Workstation Pro with 16GB of RAM, 100GB of storage and 8 processor cores. These specs are definitely not a limit or requirement. I will mention the minimal and recommended specs in the following sections.

For the installation of Jenkins I simply followed the Ubuntu/Debian section under Linux section in this link: https://www.jenkins.io/doc/book/installing/. It takes you through the installation of Jenkins itself and Java since Jenkins requires Java to run. You can also find minimal and recommended specs, installation guide, troubleshooting and many other useful information about Jenkins. It is very easy to follow but let me give you a spoiler:


Supposed that you have Java installed in your machine, simply run this command:
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
At the end of the installation, you can see that this command has:

- Setup Jenkins as a daemon launched on start,
- Created a ‘jenkins’ user to run this service,
- Directed console log output to systemd-journald,
- Populated /lib/systemd/system/jenkins.service with configuration parameters for the launch, e.g JENKINS_HOME and most importantly,
- Set Jenkins to listen on port 8080. 


After you are done with the installation, go to the Jenkins on your web browser. If you didn't do any custom configuration, it is probably in http://localhost:8080. This page will greet you:
![[initialJenkinsLogin.png]]
From the given path, receive the initial password and access Jenkins.



**This note will be continued.**
Below, there are just some random scribbles for the general steps.


General steps:

1-) Jenkins Setup (default setup to the machine will suffice)

2-) Jenkins configuration and access through browser

3-) New pipeline creation (not a freestyle one)

4-) Determining the project to be scanned in pipeline

5-) Define pipeline steps:
- Checkout 	
- Build 	
- Container Security 	
- SAST 	
- Dependency-Check 	
- SBOM 	
- SCA 	
- Git Secrets Detection 	
- DAST

6-) Jenkins pipeline part by part: Create your Jenkinsfile and run a build for every step to make sure that you are correct in terms of syntax and configuration. Of course it is not necessary but it is a good way to follow during the learning process.


