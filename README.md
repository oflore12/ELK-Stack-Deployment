## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
 
![TODO: Path with the name of diagram](Images/Project1.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

  - [install-elk](https://github.com/oflore12/ELK-Stack-Deployment/blob/main/Ansible/install-elk.yml)
  - [filebeat-playbook](https://github.com/oflore12/ELK-Stack-Deployment/blob/main/Ansible/filebeat-playbook.yml)
  - [metricbeat-playbook](https://github.com/oflore12/ELK-Stack-Deployment/blob/main/Ansible/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Built


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient in addition to restricting DoS attacks to the network.

The load balancer protects the availability aspect of security by ensuring that traffic is distributed across multiple servers and by being able to mitigate DoS attacks. Some advantages of having a jump-box is the is the added layer of security for anyone trying to access the servers directly. Also, the jump-box allows for easier manipulation and configuration of all servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump-box and system networks.
- What does Filebeat watch for? 
Filebeat collects data about the file system. The log files collected are from those generated from Apache, Microsoft Azure tools, the Nginx web server and MYSQL databases.
- What does Metricbeat record? 
Metricbeat collects machine metrics, like uptime.

The configuration details of each machine may be found below.

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| Jump Box | Gateway   | 10.0.0.4   | Linux            |
| Web-1    | Server    | 10.0.0.5   | Linux            |
| Web-2    | Server    | 10.0.0.6   | Linux            |
| ElkVM    |Log Server | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ElkVM machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
- 73.212.151.158

Machines within the network can only be accessed by the jump-box-provisioner. The machines that were allowed access to the ELK VM is the private work
station with IP address: 73.212.151.158 and the jump-box with IP address: 10.0.0.4.


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses    |
|----------|---------------------|-------------------------|
| Jump Box | Yes                 | 73.212.151.158          |
| Web-1    | No                  | 10.0.0.4                |
| Web-2    | No                  | 10.0.0.4                |
| ElkVM    | Yes                 | 73.212.151.158,10.0.0.4 |
|Load Balancer| Yes              | Any                     |

### Elk Configuration

Ansible was used to automate configurations of the ELK machine. No configuration was performed manually, which is advantageous because of the flexibility behind it. Using an Ansible playbook allows for customized configuration based on the needs of the server.

The playbook implements the following tasks:
- install docker
- install python3-pip
- install docker modules
- increase virtual memory
- use more memory
- download and launch docker elk container
- enable service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps image](Images/sebp_dockerRunning.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat and metricbeat both collect data from the web servers created. Filebeat will log information of the file system, specifically which files have been changed and when. Metricbeat collects metrics from the system and the services running on each server.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to files directory.
- Update the filebeat-config.yml file to include the IP address of the Elk VM
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

The file of playbook is /etc/ansible/files/filebeat-config.yml and the file gets copied to /etc/filebeat/filebeat.yml


The file to update to make Ansible run the playbook on a specific machine is **filebeat-playbook.yml**. In order to get the correct machine to update you need to first specify the ‘hosts’ to install the playbook at the beginning of the YML document. By adding ‘webservers’ to the document will ensure that web-1 and web-2 servers are installed. And to add the elk machine you need to add ‘elk’. 

In order to add a group, you must edit the host file and add the IP address of the machine that belongs in the group. 

- The URL used to check that the ELK-Server is running is: http://[ELK MACHINE PUBLIC IP ADDRESS]/app/kibana http://52.188.19.173:5601/app/kibana

**NOTE:** update [ELK MACHINE PUBLIC IP ADDRESS] to your elk machine public IP address

**Bonus**, the specific commands the user will need to run to download the playbook and update the files are:

- command: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > filebeat-config.yml
- command: nano filebeat-config.yml
- update to include ELK server IP on line #1106 and #1806

output.elasticsearch:
hosts: ["10.1.0.4:9200"] <----- change line #1106

username: "elastic"
password: "changeme"

setup.kibana:
host: "10.1.0.4:5601" <----- change line #1806

- command: nano filebeat-playbook.yml (should already be in the files directory)
- [Use this yml file to create play book](https://github.com/oflore12/ELK-Stack-Deployment/blob/main/Ansible/filebeat-playbook.yml)
- save file
- command: ansible-playbook filebeat-playbook.yml

- This photo shows the successful filebeat playbook
![photo of successful filebeat playbook](Images/filebeat-playbook-ansible.png)

- This photo shows the URL being used to check if filebeat is working proplery
![photo of filebeat playbook on kibana](Images/kibana1.png)

- This photo shows the logs reported by the Filebeat playbook
![photo of filebeat logs](Images/kibana2.png)
- This photo shows the Syslog events reported by Filebeat
![photo of successfull filebeat playbook](Images/kibana3.png)

Steps can be repeated for metricbeat the url to use for curl command is 'https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat'
