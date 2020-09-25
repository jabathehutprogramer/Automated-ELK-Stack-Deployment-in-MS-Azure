# Automated ELK Stack Deployment 

## Purpose of this document:

	This document contains a description of intended topology and the process for establishing an ELK server to monitor and provide telemetry for the web servers already established. 


The files in this repository were used to configure the network depicted below. 
RedTeam Network Diagram: ![ RedTeam Network Diagram](https://github.com/jabathehutprogramer/Automated-ELK-Stack-Deployment-in-MS-Azure/blob/master/Diagrams/RedTeam%20Network%20Diagram.png)


The files included in this repository have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible playbook files may be used to install only certain pieces of it, such as Filebeat. 


## Description of the Topology 
The main purpose of this network is to have a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application. This allows having an image with known vulnerabilities to allow practicing pen-testing.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network. Load balancing also makes sure tha the load is shared by all server machines in the group. It also provides for some level of DoS protection by diverting the load to another machine if one of hte machines is inundated with the DoS attack.
The jump-box in the group does not provide any service to the outside world but instead, it is used as a control machine to configure and provision the web machines and the ELK-server. 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the changes and monitor system logs and metrics such as CPU load, and possible attacks such as sudo escalation failures and SSH login attempts.
The configuration details of each machine may be found below.  

| Machine Name  | Function      | IP address   |  Operating System  |
| ------------- |---------------|--------------|--------------------|
| JumpBox       | Gateway       |52.156.78.1   |  Linux             |
| Web-1         | DVWA          |10.0.0.6      |  Linux             |
| Web-2         | DVWA          |10.0.0.10     |  Linux             |
| Web-3         | DVWA          |10.0.0.12     |  Linux             |
| ELK-Server    | ELK Server    |40.117.212.165|  Linux             |
|               |               |10.1.0.4      |                    |


## Access Policies 
The machines on the internal network are not exposed to the public Internet. They get their traffic through the Load Balancer. These machines can also be accessed from the Jump-box for configuration and management purposes.

Only the ELK-server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:  104.220.125.86


A summary of the access policies in place can be found in the table below. 
 
|  Name         | Public Accessible | Allowed IP address |
| ------------- |-------------------|--------------------|
| JumpBox       | YES               |104.220.125.86      |
| Web-1         | NO                |10.0.0.1-254        |
| Web-2         | NO                |10.0.0.1-254        |
| Web-3         | NO                |10.0.0.1-254        |
| ELK-Server    | YES               |104.220.125.86      |


## Elk Configuration 
Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually, which is advantageous because it makes the configuration repeatable, reusable, and eliminates human error in the configuration process. 

To run the configuration you need to SSH into the jumpbox (configure the jumpbox, when created, with the proper public key from your machine). 

From there you need to check which docker containers are running using:
**sudo docker container list -a**   to get a list of the dockers available.

Start and attach to the proper docker using: 
**sudo docker start (docker name)**
**sudo docker attach (docker name)**

From there you will need to make sure that the /etc/ansible/hosts files is updated with the lines describing the [elkservers] and indicated the internal IP address and add the lines identifying the ansible interpreter as in the 

**/etc/ansible/hosts** file:
'''
&# This is the default ansible 'hosts' file.
&#
&# It should live in /etc/ansible/hosts
&#
&#   - Comments begin with the '#' character
&#   - Blank lines are ignored
&#   - Groups of hosts are delimited by [header] elements
&#   - You can enter hostnames or ip addresses
&#   - A hostname/ip can be a member of multiple groups

&# Ex 1: Ungrouped hosts, specify before any group headers.

## green.example.com
## blue.example.com
## 192.168.100.1
## 192.168.100.10

# Ex 2: A collection of hosts belonging to the 'webservers' group
 [elkservers]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

 [webservers]
## alpha.example.org
## beta.example.org
## 192.168.1.100
## 192.168.1.110
10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.10 ansible_python_interpreter=/usr/bin/python3
10.0.0.12 ansible_python_interpreter=/usr/bin/python3
# If you have multiple hosts following a pattern you can specify
# them like this:

## www[001:006].example.com

# Ex 3: A collection of database servers in the 'dbservers' group

## [dbservers]
##
## db01.intranet.mydomain.net
## db02.intranet.mydomain.net
## 10.25.1.56
## 10.25.1.57

# Here's another example of host ranges, this time there are no
# leading 0s:

## db-[99:101]-node.example.com

'''



