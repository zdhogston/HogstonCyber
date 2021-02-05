# Hogstoncyber
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/zdhogston/HogstonCyber/blob/main/Images/Network-Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

  - Ansible-playbooks install-elk.yml and filebeat-playbook.yml were used to implent the live elk deploment. 

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancers protect a networks availability. The incoming traffic goes to the load balancer and it routes traffic to available servers to protect from attacks such as ddos or from server failure. The advantage of a jump box is that only one administrator will use it at a time, negating the need to scale. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat watches for log files/locations and collects log events
- Metricbeat records metric and statistical data from the operating system and from services running on the server

The configuration details of each machine may be found below.

| Name          | Function | IP Address | Operating System |
|---------------|----------|------------|----------------------|
| Jump Box      | Gateway  | 10.1.0.5   | Linux (ubuntu 18.04) |
| DVMA-Web1     |Web Server| 10.1.0.6   | Linux (ubuntu 18.04) |
| DVMA-Web2     |Web Server| 10.1.0.8   | Linux (ubuntu 18.04) |
| DVMA-Web3     |Web Server| 10.1.0.9   | Linux (ubuntu 18.04) |
| ELK           |Logs      | 10.0.0.4   | Linux (ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal, Local Machine IP address 

Machines within the network can only be accessed by SSH.
- the only machine that is able to SSH into ELK server is the Jump Box (10.1.0.5)

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses  |
|-----------|---------------------|-----------------------|
| Jump Box  |       No            | Personal Local Machine IP|
| DVWA-Web1 |       No            |         10.1.0.5         |
| DVWA-Web2 |       No            |         10.1.0.5         |
| DVWA-Web3 |       No            |         10.1.0.5         |
| Elk-Server|       No            |         10.1.0.5         |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automating configuration with ansible can be advantageous because it saves time and manpower. using playbooks, you can configure a large number of machines with one command. Once configurations are made, even someone without knowledge of the process, could run one command and configure hundreds of machines.

The playbook implements the following tasks:
- Install docker.io (the docker engine for running containers)
- install python3-pip (package used to install python software)
- install docker pip module (python client for docker)
- set the vm.max_map_count to 262144 increasing the memory for the VM to kibana standards
- download and launch docker elk container image sebp/elk:761 with published ports of 5601,9200,5044
- enabling the docker service to automatically start on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/zdhogston/HogstonCyber/blob/main/Images/Elk-dockerps-capture.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 - 10.1.0.6
- Web2 - 10.1.0.8
- Web3 - 10.1.0.9

We have installed the following Beats on these machines:
- Filebeat 

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat.yml file to /etc/ansible/roles/ directory.
- Update the configuration file to include the private IP of the Elk Server in the ElasticSearch and Kibana secitons
- Run the playbook, and navigate to Elk Server to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it? 
The playbook is filebeat-playbook.yml (TODO) and you copy it to /etc/ansible/roles/ directory. 

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? 
You would need to update filebeat.yml, and specific in /etc/ansible/hosts/ the private ip addresses of the machines into groups. For mine, I created groups 'webservers' and 'elk' so they playbooks only run actions on the designated hosts. 

- _Which URL do you navigate to in order to check that the ELK server is running? 
http://elk-server-public-ip:5601/app/kibana the elk server GUI to verify traffic incoming traffic.

