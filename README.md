## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/osugrad86/Cybersecurity-BootCamp-Project1/blob/main/Images/Project%201%20Network%20Diagram.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting external access via public IP to the network.
- Load balancers protect the availability aspect of security. Using a pool of servers to address incoming traffic, they mitigate Dos attacks
  Using a jump box, the enviroment allows control of the application deployment for our network.  Using Docker to configure and update our environment, gives flexibility and consisentcy with the 
  applications installation.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the enviroment and system logs.
- Filebeat helps generate and organize log files to send to Logstash and Elasticsearch. Specifically, it logs information about the file system, including which files have changed and when.
- Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.
| Name         | Function           | IP Address    | Operation System |
|--------------|--------------------|---------------|------------------|
| Jump box     | Gateway            | 13.68.227.24  | Linux            |
| Web 1 (DVMA) |                    | 20.25.15.56   | Linux            |
| Web 2 (DVMA) |                    | 20.25.15.56   | Linux            |
| Elk-Server   | Metrics via Kibana | 20.122.12.117 | Linux            |

### Access Policies
The machines on the internal network are not exposed to the public Internet. 

Only the Web virtual machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 20.25.15.56

Machines within the network can only be accessed by SSH.
- The network was configured to grant access over IP Address and I used my Home laptop's IP address to build the security rules.
  IP Address variable - see Special Note below.

### Project Challenges: 
**My internet service provider (ISP) uses dynamic assignment of my home laptop's IP address.  This requires an update to the NSG (Network Security Group) of my virtual network each time I started up the laptop. Using a tool like IP Monkey or IP Chicken to get the current IP address of my laptop, I needed to edit the NSG to reflect this new value.  Otherwise, I would not have access to any of the virtual machines.**

My home laptop was granted access to the ELK VM.  Its IP address varies see: Special Note above. 

### Lesson Learned: 
**When building out the virtual network in Azure, I was unable to load the resources I had created. I got a fetching error msg. (see below)**

![Azure Portal Fetch Error ](https://github.com/osugrad86/Cybersecurity-BootCamp-Project1/blob/main/Images/Fetch_Error.png)

**I also had my Anti-Virus Software deny access in the Azure portal with a message about the web address being blacklisted. I was able to update my Anti-virus software with a rule that added an exception to grant access to that web address. This prevented me from getting the fetching error and I was able to access all the resources again in the Azure portal.**

![Virus block](https://github.com/osugrad86/Cybersecurity-BootCamp-Project1/blob/main/Images/Virus-block.png)




A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible  | Allowed IP Addresses |
|--------------|----------------------|----------------------|
| Jump box     | Yes                  |                      |
| Web 1 (DVMA) | No                   | 20.25.15.56          |
| Web 2 (DVMA) | No                   | 20.25.15.56          |
| Elk-Server   | Yes                  |                      |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automation gives consistant deployment of the docker containers. This allows for easier management of the entire network. Any modifications or updates to the ansible code files (YAML,etc) is easily done and defined by the existing code.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installation of the docker application using apt install
- Installation of python3-pip using apt install
- Increase virtual memory in the enviroment using sysctl -w vm.max_map_count=262144
- Download and launch the Elk Container using image: sebp/elk:761

The following screenshot displays the result of running `docker ps` akfter successfully configuring the ELK instance.

![Elk Stack Container](https://github.com/osugrad86/Cybersecurity-BootCamp-Project1/blob/main/Images/Elk_Container_Running.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Elk server monitors Web 1 (DVMA) and Web 2 (DVMA)

We have installed the following Beats on these machines:
- Filebeats and Metricbeats

![Filebeats Screenshot](../../../blob/main/Images/Kibana-Filebeats.png)


![Metricbeats Screenshot](../../../blob/main/Images/Kibana-Metricbeats.png)


These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yaml file to /etc/ansible.
- Update the ansible.cfg file to include the remote_user for the hosts you will be updating.  The /etc/ansible/hosts file also need to update with the IP addresses of the Web VMs 
- Run the playbook, and navigate to kibana web page to check that the installation worked as expected.


- install-elk.yml copied to /etc/ansible on the control node (jump box)
- Changes are needed in both the Ansible.cfg and hosts files.  Ansible.cfg needs the remote user name update for the VM's being update via Ansible. The /etc/hosts file needs updated with the IP Address of the web VM.  _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- http://<Elk-Server Public IP>:5601/app/kibana _Which URL do you navigate to in order to check that the ELK server is running?_

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
