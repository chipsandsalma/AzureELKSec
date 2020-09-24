# AzureELKSec
Michael Salma's repository for ELK Installation

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/CloudSecElkDiagram.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the FileMetricBeat-Playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

```
---
- name: installing and launching filebeat and metricbeat
  hosts: elk
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb -L -O

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: download metricbeat
    command: curl https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb -L -O

  - name: install metricbeat deb
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: drop in metricbeat.yml
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: enable metricbeat
    command: metricbeat modules enable docker

  - name: setup metricbeat
    command: metricbeat setup

  - name: start metricbeat
    command: service metricbeat start
```

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump-Box | Gateway  | 10.0.0.9   | Linux            |
| Web-1    |Web server| 10.0.0.10  | Linux            |
| Web-2    |Web server| 10.0.0.11  | Linux            |
| Web-3    |Web server| 10.0.0.13  | Linux            |
| ElkServe |Elk Stack | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
67.187.254.167

Machines within the network can only be accessed by the Jump-Box 10.0.0.9

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump-Box | yes                 | 67.187.254.167       |
| Web-1    | no                  | 10.0.0.9             |
| Web-2    | no                  | 10.0.0.9             |
| Web-3    | no                  | 10.0.0.9             |
| ElkServe | no                  | 10.0.0.9             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows for fast and accurate deployments.
The playbook implements the following tasks:
- Downloads filebeat
- Installs filebeat
- Starts filebeat service
- Downloads metricbeat
- Installs metricbeat
- Starts metricbeat service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/DockerPS.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.10 10.0.0.13 10.0.0.14

We have installed the following Beats on these machines:
- filebeat
- metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors log files and collects log events to then forward to Elasticsearch for indexing.
- Metricbeat collects metrics from the operating system and from services on the server to then forward to Elasitcsearch for indexing.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the FileMetricBeat-playbook.yml file to /etc/ansible/roles.
- Update the hosts file to include ELK group and list the server IP designated for installation.
-   nano /etc/ansible/hosts
- Run the playbook, and navigate to http://[ELK.PUBLIC.IP]:5601/app/kibana to check that the installation worked as expected.
-   ansible-playbook FileMetricBeat-playbook.yml
