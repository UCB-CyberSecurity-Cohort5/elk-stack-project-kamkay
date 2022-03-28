## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text] (https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/NetworkDiagrams/VN1%2Belknet.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the following files may be used to install only certain pieces of it, such as Filebeat.

- [Ansible Configuration file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/ConfigFiles/ansible.cfg)
- [Filebeat Configuration file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/ConfigFiles/filebeat-config.yml)
- [Metricbeat Configuration file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/ConfigFiles/metricbeat-config.yml)
- [DVWA-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/install-dvwa.yml)
- [ELK-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/install-elk.yml)
- [Filebeat-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/filebeat-playbook.yml)
- [Metricbeat-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting traffic to the network.
Load balancers protect from distributed denial-of-service (DDOS) attacks.
The advantage of a jump box is that it is a secure computer that only allows specified connections into the network. This inturn restricts the number of possible connections into the network thus making it more secure than a network without a jump box. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat watches and monitors log files. It can also monitor locations users may be accessing the servers, as well as collect additional logging details. It is made as a lightweight shipper for forwarding and centralizing the collected log data. The collected log data is forwarded to either Elasticsearch or Logstash for indexing. 
[Filebeat Overview](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html)
 
- Metricbeat takes the metrics and statistics that it collects and ships it to the chosen output, typically Elasticsearch or Logstash. Metricbeat is able to help users moitor their servers by collecting metrics from the system and any services that may be running the server. 
[Metricbeat Overview](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html)

The configuration details of each machine may be found below.

| Name          | Function        | Public IP Address | Private IP Address | Operating system |
|---------------|-----------------|-------------------|--------------------|------------------|
| Host Computer | Access Control  | Dynamic           | Static             | MacOS            |
| JumpBox       | Gateway         | 104.210.40.73     | 10.0.0.4           | Linux            |
| Load Balancer | Load Balancer   | 13.88.100.22      | n/a                | Linux            |
| Web1          | Web Server      | 13.88.100.22      | 10.0.0.5           | Linux            |
| Web2          | Backup Server 1 | 13.88.100.22      | 10.0.0.6           | Linux            |
| Web3          | Backup Server 2 | 13.88.100.22      | 10.0.0.7           | Linux            |
| ELK server    | ELK server      | 20.98.87.172      | 10.1.0.4           | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK Server machine can accept connections from the Internet. Access to this machine is only allowed from the following devices:
- Host computer *Public IP* via TCP port 5601

Machines within the network can only be accessed by the installed ansible container located on the JumpBox. Additionally, the JumpBox is only accessible by the Host Computer via SSH.
- JumpBox *Public IP* 104.210.40.73
- Host Computer *Public IP* 67.182.112.174 via SSH

A summary of the access policies in place can be found in the table below.

| Name          | Network Security Group | Publicly Accessible | Allowed IP Addresses | Protocol | Port used |
|---------------|------------------------|---------------------|----------------------|----------|-----------|
| JumpBox       | RedTeamSG              | no                  | 67.182.112.174       | SSH/TCP  | 22        |
| Load Balancer | RedTeamSG              | no                  | 67.182.112.174       | HTTP     | 80        |
| Web1          | RedTeamSG              | no                  | 10.0.0.4             | SSH/TCP  | 22        |
| Web2          | RedTeamSG              | no                  | 10.0.0.4             | SSH/TCP  | 22        |
| Web3          | RedTeamSG              | no                  | 10.0.0.4             | SSH/TCP  | 22        |
| ELK Server    | ELKserverSG            | no                  | 67.182.112.174       | TCP      | 22        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows for easy management and installation of programs and services on any given machines.  
- This also allows for companies to be prepared for company growth as the yaml files allow for easy alterations to adapt for any specified needs on the machines being managed. 

The playbook implements the following tasks:
- Downloads and install Docker onto ELK VM
- Ensureds the virtual memory is sufficient enough to handle ELK
- Downloads and enables the docker elk container at bootup

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![DockerPSOutput.png](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/images/DockerPSOutput.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- *Web1* 10.0.0.5
- *Web2* 10.0.0.6
- *Web3* 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects log data of everything that is attempted on the machines being monitored. This includes and is not limited to user logins, any failed login or site access attempts and any other site logged events. 
- Metricbeat collects metrics and statistics such as location of user machine, machine CPU usage, as well as metrics pertaining to refer sites and etc from the web server machines. 

### Using the Playbooks
In order to use the Filebeat and Metricbeat playbooks, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Filebeat and metricbeat configuration and playbook files to the /etc/ansible file.
  - [Filebeat Configuration file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/ConfigFiles/filebeat-config.yml)
  - [Metricbeat Configuration file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/ConfigFiles/metricbeat-config.yml)
  - [Filebeat-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/filebeat-playbook.yml)
  - [Metricbeat-install playbook](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/Playbooks/metricbeat-playbook.yml)
- Update the hosts file to include the Webserver and ELK groups 
  - [Hosts file](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-kamkay/blob/main/hosts)
- Run the playbooks, and navigate to Kibana homepage to check that the installation worked as expected.
  - http://[ELKserver IP]/app/kibana#/home 