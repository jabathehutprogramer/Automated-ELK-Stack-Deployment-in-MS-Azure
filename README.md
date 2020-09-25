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
#### INSTERT /etc/ansible/hosts file linke here 

Then from the docker container run the command: 
**ansible-playbook /etc/ansible/install-elk.yml**

When you run it the screen output should look like this:

install-elk.yml output:![install-elk.yml output](https://github.com/jabathehutprogramer/Automated-ELK-Stack-Deployment-in-MS-Azure/blob/master/Diagrams/install-elk-output.png)

The playbook implements the following tasks: 
- Connects securely to the ELK server and gathers status on what is installed
- Installs docker.io
- Installs python3 pip
- Installs docker. This is needed so Ansible can then utilize that module to control docker containers
- Increases available memory. This is needed to allow for always restarting the ELK server 
- Last thing it does is start the ELK server and enables it to communicate over the established ports for ELK Kibana.

The following screenshot displays the result of running **docker ps** after successfully configuring the ELK instance. 
 
docker ps output: ![ docker ps output](https://github.com/jabathehutprogramer/Automated-ELK-Stack-Deployment-in-MS-Azure/blob/master/Diagrams/docker-ps.png)

## Target Machines & Beats 
This ELK server is configured to monitor the following machines: 

|  Name         |IP Address         |
| ------------- |-------------------|
| Web-1         | 10.0.0.6          |
| Web-2         | 10.0.0.10         |
| Web-3         | 10.0.0.12         |

We have installed the following Beats on these machines using the following curl command:

**curl -L -0 https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb**
	which is included in the ansible playbook file: **filebeat-playbook.yml** in this repository under Ansible files.
	## Inster linke here.
To check the installation, from the public machine navigate to:
**http://[40.117.212.165]:5601/app/kibana**     the IP address being that of the ELK Server.

These Beats allow us to collect the following information from each machine: the logs and metrics from our unique environments and document them with essential metadata from hosts, container platforms like Docker and Kubernetes,

BEATS output: ![BEATS output](https://github.com/jabathehutprogramer/Automated-ELK-Stack-Deployment-in-MS-Azure/blob/master/Diagrams/beats-output2.png)

## Using the Playbook 
In order to use the playbook, you will need to have an Ansible control node already configured. In our case it was already created and is in fact the jumpbox that was configured already. 

SSH into the control node and follow the steps below:
 - Copy the **filebeat-playbook.yml** file to **/etc/ansible** directory. 
 
- Update the **/etc/filebeat/filebeat-config.yml**  file to include:
   **Hosts:[“10.1.0.4:9200”]**
   **Username: “elastic”**
   **Password: “changme”** , this password can be changed later.
   
- Run the playbook, and navigate to already open  to check that the installation worked as expected. 


