# Team Members and contribution
<h3>Bhushan Thakur & Rohit Nambisan</h3>

* Responsible for provisioning and configuring the Jenkins server.  

* Bhushan was also responsible for using cloud service provider DigitalOcean to deploy both the application to the cloud.

<h3> Kiran Krishnan Balakrishnan & Vishal Kumar Seshagirirao Anil</h3>

* Kiran Krishnan was responsible for creating iTrust job application.

* Vishal Kumar was responsible for creating Checkbox.io application.

* Both of them were also responsible for creating ansible playbook which was used to post jobs to the jenkins server.

<b>Youtube Link (Using Vagrant)</b> : https://www.youtube.com/watch?v=PXKcIXspejo&feature=youtu.be

(Using Digital Ocean Droplets)
<b>Link to checkbox.io</b> : http://198.211.96.26/ <br>
<b>Link to Jenkins Server</b>: http://165.227.84.97:8080/ <br>
<b>Link to iTrust </b>: http://198.211.109.140:8080/iTrust <br>

<h3> Jenkins Provisioning and Configuring Issues </h3>
  - <b>Python version issue:</b>The ansible-playbook that we ran required python2 support but our box containning jenkins had default python3 installed.
  So some of the pip packages were not installed properly. To resolve this we added a line in inventory as follows. 
 
 ```
 
 ansible_python_interpreter=/usr/bin/python
 
 ```
  - <b>Memory Issue:</b> Initally configured jenkins in 512 MB ubuntu 16.04 box, and while trying to run very long jobs, the server used to crash. It was difficult to troubleshoot as there was no proper error logging. The issue was resolved by increasing the memory to 1 GB. 

  - <b>API issue:</b> We had trouble with jenkins REST API, we failed to realize that we had to provide jenkins user credentials along with the jenkins crumb token. This wasted a lot of our time. 

  - <b>Connectivity issue:</b> While trying to ssh from on jenkins server to provision new droplet, we faced ssh key verification failed. For this we disabled the host_key_checking in ansible.cfg.
  
  -<b>Post-build issue:</b> Jenkins was initially not allowing to run provision ansible playbook as a post-build task, and we resolved by creating another jenkins job and triggering it after the build jobb is done.
  
<h3>Building iTrust Application issues</h3>

  - <b>Lower Case table names:</b> This was a minor issue, but really was difficult to find. The mysql server by default didnt allow to create tables with lower case names. This error was only visible when the mvn package command was ran. This was resolved by enabling it in the mysql config file. pip3 and required 
  
<h3>Building Checkbox.io Application issues</h3>

  - <b>Pymongo Dependency:</b> Pymongo could not be installed with pip3 and required pip to do so. 
 
  - <b>Lack of documentation:</b> We did not find one validated piece of document to give step by step instructions to deploy checkbox.io manually
  
  - <b>MongoDB repository:</b> We faced a few issues while trying to install MongoDB using the Ansible module

Jenkins Page: </br>
 
![Deployed jenkins server](/jenkins.png)

iTrust Page: </br>

![Deployed iTrust page](/itrust.png)

Checkbox.io Page: </br>
 
![Deployed checkbox page](/checkbox.png)

# VCL Issues
The VCL comes with a limited fixed size RAM. We used this VM to provision the Jenkins server which tends to get heavy when there are multiple jobs being triggered and run. This slowed down our overall end-to-end testing and runs. 

# Virtualbox Issues
Certain versions of the application have a bug due to which the the newly created VM's route is not addded to the kernel routing table. Due to this issue, we often could not establish an SSH connection between the host and the VM.

# DigitalOcean Issues
The way I structured deployment to the digitalocean was a modular approach. One module being responsible for getting the digital ocean image and other being responsible for installing the stuff into that image. This structure allowed us for bet ter organization of our code. 
<h3>Issues</h3>
*<b>SSH Key Verification: </b> Whenever we create a new image using DigitalOcean we can pass a ssh key to the image. This ssh key is stored in the digital ocean account under a certain name. The code that we wrote allowed us to create new images dynamically but we forgot to put unique name to it, each time we create the image. This caused SSH key verification problem, as the newly created image had the previously stored ssh key from the account. <br>

*<b>No route to host:</b> Whenever we tried to ssh into newly created images, we got error to SSH host not found. We fixed the problem by waiting for the image and then SSH it.

```
local_action= wait_for host={{host_name}} state=restarted
```
# Special Note
We as a group are very thankful to the Professor Parnin and TA Shaurya Garg for allowing us one extra day to figure out the roadblocks. Last few days were very busy, we toiled day and night to sort out the minor issues. But in the end, the effort put in allowed us to look more into details about the tools that we used. And we are hopeful that this knowledge gained will serve us better in the future milestones. 
