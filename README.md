## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

C:\Users\jonat\Github\Images\Network Diagram.html

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

```

---

 - name: Elk-Paybook
   hosts: Elk
   remote_user: azureuser
   become: true
   tasks:

   - name: Install docker.io
     apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present

   - name: Install python3-pip
     apt:
      force_apt_get: yes
      name: python3-pip
      state: present

   - name: Install Docker Module
     pip:
      name: docker
      state: present

   - name: increase virtual memory
     command: sysctl -w vm.max_map_count=262144

   - ansible.posix.sysctl:
       name: vm.max_map_count
       value: '262144'
       state: present
       reload: yes

   -  name: download and launch a docker elk container
      docker_container:
       name: Elk
       image: sebp/elk:761
       state: present
       restart_policy: always
       published_ports:
       - "5601:5601"
       - "9200:9200"
       - "5044:5044"

```

This document contains the following details:
- Description of the TopologuW
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- The load Balancers that all incoming traffic will be shared by both Web servers. Having a jump box ensures that only authorized users will be able to
  connect to the web servers. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the netwrok and system metrics.
- Filebeat is used to detect changed in the file system
- Metricbeat is used to detect changed in the system metrics.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.7   | Linux            |
| Web1     |Web Server| 10.0.0.8   | Linux            |
| Web2     |Web Server| 10.0.0.9   | Linux            |
| Elk      |Monitoring| 10.1.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-76.31.19.199

Machines within the network can only be accessed by other machine in the network.
- Web1 and Web2 can access the Elk VM.
  IP Addresses:
  -10.0.0.8
  -10.0.0.9

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 76.31.19.199         |
|   Web1   | No                  | 10.0.0.7             |
|   Web2   | No                  | 10.0.0.7             |
|   Elk    | No                  | 10.0.0.7             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
-Ansible helps automate simple tasks allowing more time for other tasks. This also ensures that all the machines are set up the same way. 

The playbook implements the following tasks:
-Install Docker.io
-Install Python3
-Install Docker module
-Increase memory to 262144
-Download and Launch Docker Elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

C:\Users\jonat\Github\Images\Docker.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.8
10.0.0.9

We have installed the following Beats on these machines:
-Filebeat
-Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat detecs changes to the filesystem, mainly Apache logs. Metricbeat monitors changes in the system metrics. This is mainly used to detect SSH login
  attempts. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks file to the Ansible control node.
- Update the host file to include the VMs you want to run each playbook for. 
- Run the playbook, and navigate to http://[your.ELK-VM.External.IP]:5601/app/kibana to check that the installation worked as expected.
