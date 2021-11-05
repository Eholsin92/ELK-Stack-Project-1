# ELK-Stack-Project-1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[![unnamed.png](https://i.postimg.cc/J7sm12PW/unnamed.png)](https://postimg.cc/dDcg6nbW)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project file may be used to install only certain pieces of it, such as Filebeat.

  - /etc/ansible/my-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

-Load balancing ensures that the application will be highly secure, in addition to restricting unauthorized and excess access to the network.

-Load balancers protect against DDoS attacks by allowing traffic to be distributed across however many web servers are available to avoid overloading a server with all of the incoming traffic. 

-Using a jump box allows the administrator to have a single node that can be used to configure the rest of the network and allow the admin to have a single node to protect, secure, and monitor decreasing the attack vectors. 

-Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic/usage.

-Filebeat watches logs and locations that you specify and sends any events or changes to your ELK stack. 

-Metricbeat records and monitors system performance such as system CPU, memory use, memory load, etc. 

The configuration details of each machine may be found below.

| Name     	| Function     	| IP Address 	| Operating System 
|----------	|--------------	|------------	|------------------	|
| Jump Box 	| Gateway      	| 10.0.0.4   	| Linux            	|
| Web 1    	| Web server   	| 10.0.0.6   	| Linux            	|
| Web 2    	| Web server   	| 10.0.0.7   	| Linux            	|
| ELK VM   	| Log Analysis 	| 10.1.0.4   	| Linux            	|



### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox and Web server machines can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 
- 75.176.52.211

Machines (Webservers and ELK VM) within the network can only be accessed by the Ansible container on the Jump Box Provisioner.
- The Ansible container within the Jump Box Provisioner is the only machine allowed to access my ELK VM. The IP address of the Ansible container is 10.0.0.4. 

A summary of the access policies in place can be found in the table below.

| Name     	|  Publicly Accessible 	| Allowed IP Address           	            |
|----------	|----------------------	|------------------------------	            |
| Jump Box 	| Yes                  	| 75.176.52.211 - My IP        	            |
| Web 1    	| No                   	| 10.0.0.4 - Jumpbox (Ansible) 	|
| Web 2    	| No                   	| 10.0.0.4 - Jumpbox (Ansible) 	|
| ELK VM   	| No                   	| 10.0.0.4 - Jumpbox (Ansible) 	|


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it ensured all configurations were carried out in the exact same way and takes out the variance of human error. It also allows the ability to configure multiple machines at the same time using the same configuration file. 

The playbook implements the following tasks:
- Increase memory of ELK VM
- Install docker.io
- Install python3-pip
- Download and install the ELK container
- Ensure the ELK contain is enabled and running on the VM

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[![Elk-docker-ps.png](https://i.postimg.cc/856SbFFz/Elk-docker-ps.png)](https://postimg.cc/mzBJLgNv)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.0.0.6
- Web 2: 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects log data and events from locations and files that you specify, like changes to configuration logs. 
- Metricbeat collects system metrics that the OS and various services on the machine are using, such as CPU usage. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat and metricbeat yml files (/etc/ansible/ansible/roles/filebeat-playbook.yml and /etc/ansible/roles/metricbeat-playbook.yml) to your current ansible folder. 
- Update the /etc/ansible/hosts file to include the hosts you want to work with. For example I wanted to work on my web server machines so I added  
“10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3”
Under the [webservers] section of hosts. This allows you to group together certain machines so that when running a playbook you can run the same commands on all the machines within the group. 

- Run the filebeat and metricbeat playbooks, and navigate to the web servers (or whatever machines you ran the playbook to affect) to check that the installation worked as expected. You can run sudo service filebeat status and sudo service metricbeat status to ensure both programs are downloaded and actively running on the machines. 
You can also go to http://[yourIPaddress]:5601/app/kibana and navigate to filebeat and metricbeat to ensure the data is coming in correctly. 

- Which file is the playbook? Where do you copy it?
 - The playbook for filebeat is /etc/ansible/roles/filebeat-playbook.yml. The playbook for metricbeat is /etc/ansible/roles/metricbeat-playbook.yml. Copy both of them into the /etc/ansible folder for ease of finding them. 
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
Update the ansible hosts file to include the IP address of the machine(s) you wish to run the playbook on. These will be grouped in different names (ex: [webservers], [elk]) which can be added to hosts in an ansible playbook to ensure the playbook only runs on the machines in the group specified in the playbook. 
- Which URL do you navigate to in order to check that the ELK server is running?
http://[yourIPaddress]:5601/app/kibana. Mine was http://20.85.241.248:5601:/app/kibana  


To run the playbook for filebeat use the command: ansible-playbook filebeat-playbook.yml
To run the playbook for metricbeat use the command: ansible-playbook filebeat-playbook.yml

