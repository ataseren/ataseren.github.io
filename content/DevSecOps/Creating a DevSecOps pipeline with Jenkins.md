---
title: Creating a DevSecOps pipeline with Jenkins
tag: cybersecurity, devsecops
---

First of all, let's setup a Jenkins instance on our machine. Since it has a simple installation and using a container can have a negative impact on performance or storage, I installed Jenkins directly on my machine. I'm using Ubuntu 22.04 on VMware Workstation Pro with 16GB of RAM, 100GB of storage and 8 processor cores. These specs are definitely not a limit or requirement. I will mention the minimal and recommended specs in the following sections.

For the installation of Jenkins I simply followed the Ubuntu/Debian section under Linux section in this link: https://www.jenkins.io/doc/book/installing/. It takes you through the installation of Jenkins itself and Java since Jenkins requires Java to run. You can also find minimal and recommended specs, installation guide, troubleshooting and many other useful information about Jenkins. It is very easy to follow but let me give you a spoiler:

Supposed that you have Java installed in your machine, simply run this command:
```bash
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

After the command is complete, go to the Jenkins on your web browser. If you didn't do any custom configuration, it is probably in http://localhost:8080. This page will greet you:
![initial_jenkins_login.png](initial_jenkins_login.png)
From the given path, receive the initial password and access Jenkins. You may need root access to reach the path.

Choose "Install suggested plugins". We will install required plugins for the steps of our pipeline one by one, from the "Available plugins" section that will be mentioned:
![plugin.png](plugin.png)


These are the suggested plugins:
![default_plugin_install.png](default_plugin_install.png)


After the plugin installation, you are asked to create an admin user for Jenkins. You can choose "Skip and continue as admin" option but you have to use the initial password to access Jenkins. Therefore, I suggest you to create a user. You can use the same credentials as your machine's credentials for easiness:
![create_first_admin_user.png](create_first_admin_user.png)


You can directly choose "Save and Finish" option since our Jenkins is set up locally instead of on a cloud system and we will conduct all scans locally:
![url_config.png](url_config.png)


If you are seeing this page, congrats! You have successfully installed Jenkins to your system:
![dashboard.png](dashboard.png)

Now, let's continue with creating a pipeline.

By clicking on "New Item", you will get to this page where Jenkins provides many options. On this page, we will choose "Pipeline" option. In this option, we will create our pipeline from scratch, by coding a Jenkinsfile. "Freestyle project" option provides a better UI and easier way to create a pipeline. However, some steps do not fit easily in Freestyle and once you learn how to create a pipeline from scratch, you will automatically learn how to use other options since this is the base of Jenkins pipelines:
![item_options.png](item_options.png)


On the opened page, you will see the general options for your project. You can leave them like that for now since our purpose is to learn the basics of a security-based scan and we will test a project that is intentionally vulnerable and isn't a part of a continuing development process:
![pipeline_general_settings.png](pipeline_general_settings.png)


This is the section which we will create the pipeline from the scratch:
![pipeline_section.png](pipeline_section.png)


Now, let's have an overview of steps of an example security scanning pipeline:
- Checkout: Cloning the project from a Git repository	
- Build: Building the project since some steps require a built project
- SAST: Static Application Security Testing tests upon the code and built project
- Container Security: If there is a container of the project, you can test everything on the container
- Dependency Check: Performs a scan on dependencies of a project and reports if there are any vulnerabilities on them
- SBOM: Software Bill Of Materials is a list of components in a piece of software. SBOMs are useful to determine used technologies in a project and can be used for additional security scans.
- SCA: Software Composition Analysis can be used to automate the identification of vulnerabilities in entire container images, packaged binary files and source code.
- Git Secrets Detection: Can prevent accidental code-commit containing a secret
- DAST: Dynamic Application Security Testing approaches security from the outside. It requires applications be fully compiled and operational to identify network, system and OS vulnerabilities.

During my learning process, I ran a build for every step to make sure that the code is correct in terms of syntax and configuration. I will follow the same way throughout the note to both show it to you better and remind myself the process.

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


