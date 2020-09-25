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
| JumpBox       | DVWA          |10.0.0.10     |  Linux             |
| JumpBox       | DVWA          |10.0.0.12     |  Linux             |
| ELK-Server    | ELK Server    |40.117.212.165|  Linux             |
|               |               |10.1.0.4      |                    |


