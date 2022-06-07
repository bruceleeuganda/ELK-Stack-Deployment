## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(ELK-Stack-Deployment/Ansible)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the metricbeat-playbook.yml file may be used to install only certain pieces of it, such as Metricbeat.

  - install_elk.yml
  - pentest.yml
  - filebeat-playbook.yml
  - metricbeat-playbook.yml
    

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting access to the network.
- Load balancers ensure that neither web VM will be overloaded and direct web traffic accordingly
- The advantage of a jump box enables secure and quick access between absible and the multiple VM deployments

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system Metrics.
- Filebeat will watch for traffic to the Virtual Machines
- Metricbeat will watch the system metrics of the Virtual Machines such as cpu and memory usage 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Host     | 10.1.0.5   | Linux            |
| Web-2    | Backup   | 10.1.0.6   | Linux            |
| Web-3    |Redundancy| 10.1.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk VM webserver machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My Home IP and any other whitelisted IP addresses

Machines within the network can only be accessed by SSH.
- The jump box is used to access all of the other VMs 
  IP address? (13.64.148.40 Public) (10.1.0.4 Private)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | Home IP              |
| Web 1,2,3| No                  | Jump Box 10.0.0.1    |
| ELK VM   | No                  | Jump Box and Home IP |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of using ansible to configure the machines is easy implementation and deployment across multiple web server virtual machines

The playbook implements the following tasks:

- ... Add the username of the machines into the ansible.cfg file
      Add Elk machine in the hosts configuration file
- ... The first task in the ansible playbook is to set vm.max_map_count to value 262144
      Following that the playbook will install docker.io and python3 
      Next the container sebp/elk:761 will be installed
      ports 5601 9200 and 5044 will be mapped
      Finally the container will be started and docker will be enabled

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(ELK-Stack-Deployment/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.1.0.5
  Web-2 10.1.0.6
  Web-3 10.1.0.8

We have installed the following Beats on these machines:
- Metricbeat
  Filebeat

These Beats allow us to collect the following information from each machine:

  Metricbeat will collect system data from the web VMs showing how much CPU, Memory, etc. is being used by each system
  Metricbeat can be configured to monitor for anomalies which could be inspected and viewed as a potential attack
  Filebeat will display incoming and outgoing file data from each VM 
  Filebeat can be configured also to monitor for malicious traffic and set up alerts

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the hosts file to /etc/ansible.
- Update the hosts file to include the ip addresses of the Web and Elk VMS
- Run the playbook, and navigate to kibana to check that the installation worked as expected.

- _Which file is the playbook?
   The playbooks are all .yml files
   Where do you copy it?
   into ~/etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine?
   The hosts file and the config files for each metricbeat and filebeat
   How do I specify which machine to install the ELK server on versus which to install Filebeat on?
   In the playbook the hosts option at the top should be changed to ELK or webservers. 
- _Which URL do you navigate to in order to check that the ELK server is running?
   http://20.25.35.134:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
ansible-playbook install-elk.yml
ansible-playbook pentest.yml
ansible-playbook filebeat-playbook.yml
ansible-playbook metricbeat-playbook.yml
