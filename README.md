## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Screen Shot 2022-05-21 at 11 44 53 AM](https://user-images.githubusercontent.com/105984059/169659468-6300de34-fe13-4042-b986-c9b0fb94b414.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - root@0d1a3e38ab7d:~# /etc/ansible/roles/filebeat-playbook.yml
    root@0d1a3e38ab7d:~# /etc/ansible/roles/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting traffic to the network.
- Load balancers protect against DDOS attacks by rerouting traffic, and the advantage of the jump box was to allow admins to access other points of the network and prevents Azure VMs from being exposed to a public IP

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat monitors log files or locations that you specify
- Metricbeat takes metrics and stats and ships to output

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function      | IP Address | Operating System |
|----------|----------     |------------|------------------|
| Jump Box | Gateway       | 10.0.0.4   | Linux            |
| Web 1    | Ubuntuserver  | 10.0.0.5   | Linux            |
| Web 2    | Ubuntuserver  | 10.0.0.7   | Linux            |
| ELKserver| Ubuntuserver  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.141.181.88

Machines within the network can only be accessed by Jump Box through SSH.
- Jump Box, 10.0.0.4 

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses |
|----------    |---------------------|----------------------|
| Jump Box     | Yes                 | 73.141.181.88        |
| Web 1/ Web 2 | No                  | 10.0.0.4             |
| Elkserver    | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible lets us quickly and easily configurations to multiple virtual machines

The playbook implements the following tasks:
- Create ELK VM in Azure
- SSH into Jump Box
- Attach Ansible and SSH to ELK VM and run playbooks

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<img width="1104" alt="DockerSC" src="https://user-images.githubusercontent.com/105984059/169659769-bb040377-cc2e-4f1c-a89c-17c0db5a45e1.png">


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5 
- Web-2: 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect log files from specific files, such as MS Azure.
- Metricbeat will be used to monitor VM stats, and tells us how the system is. 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to ansible folder.
- Update the config file to include correct IP
- Run the playbook, and navigate to http://(publicIP):5601/app/kibana to check that the installation worked as expected.

For Filebeat setup (same instructions for Metricbeat, but replace filebeat with metricbeat)
- Copy the Filebeat Config file to /etc/ansible 
- Update two sections: output.elasticsearch and setup.kibana. In output.elasticsearch, in hosts you want to use the ELK private IP, and leave the username and password alone. In setup.kibana, in host put the Elk private IP. Then run the playbook using ansible-playbook filebeat-playbook.yml
- Navigate to http://(publicIP):5601/app/kibana. Navigate to Logs > add log data > system logs > scroll down to module status and click check incoming data to check that it worked correctly. 
