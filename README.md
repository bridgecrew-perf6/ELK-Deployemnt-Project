## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

 
![azure_network_elk- drawio](https://user-images.githubusercontent.com/20432932/161847538-42a1b056-e38c-4005-8f3c-2bddd82fd709.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

  -	[My First Playbook]
  -	[Hosts] 
  -	[Ansible Configuration]
  -	[Ansible ELK Installation and VM Configuration]
  -	[Filebeat Config]
  -	[Filebeat Playbook]
  -	[Metricbeat Config]
  - [Metricbeat Playbook]

**_This document contains the following details:_**
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly **_functional and available_**, in addition to restricting **_traffic_** to the network.
- What aspect of security do load balancers protect?
     * **_•	Load balancers add resiliency by rerouting live traffic from one server to another if a server falls prey to DDoS attack or otherwise becomes unavailable._**
- What is the advantage of a jump box?
    * **_A Jump Box Provisioner is also important as it prevents Azure VMs from being exposed via a public IP Address. This allows us to do monitoring and logging on a single box. We can also restrict the IP addresses able to communicate with the Jump Box, as we've done here._**

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **_network and system logs._**

- What does Filebeat watch for?

  * **_•	Filebeat monitors the log files or locations that you specify, collect log events, and forwards them either to Elasticsearch or Logstash for indexing._**

- What does Metricbeat record?
  * **_•	Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash._**

The configuration details of each machine may be found below.

| Name      | Function     | IP Address             | Operating System |
|-----------|--------------|------------------------|------------------|
| Jump Box  | Gateway      | 10.1.0.4/20.213.81.197 | Linux            |
| Web-1     | UbuntuServer | 10.1.0.5/20.211.160.88 | Linux            |
| Web-2     | UbuntuServer | 10.1.0.6/20.211.160.88 | Linux            |
| DVWA-VM3  | UbuntuServer | 10.1.0.7/20.211.160.88 | Linux            |
| ELKserver | UbuntuServer | 10.2.0.4/20.205.40.104 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **_Jump-Box-Provisioner_** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- **_Workstation My Public IP through TCP 5601._**

Machines within the network can only be accessed by **_Workstation and Jump-Box-Provisioner through SSH Jump-Box._**

Which machine did you allow to access your ELK VM?
-	**_Jump-Box-Provisioner IP: 10.1.0.4 via SSH port 22._**

 What was its IP address?
**_Workstation My Public Ip via port TCP 5601._**


A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses                     |
|-----------|---------------------|------------------------------------------|
| Jump Box  | Yes                 | 20.213.81.197 (Workstation IP on SSH 22) |
| Web-1     | No                  | 10.1.0.4 on SSH 22                       |
| Web-2     | No                  | 10.1.0.4 on SSH 22                       |
| DVWA-VM3  | No                  | 10.1.0.4 on SSH 22                       |
| ElkServer | No                  | Workstation My Public IP using TCP 5601  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible?
  * **_There are multiple advantages, Ansible lets you quickly and easily deploy multiple applications through a YAML playbook._**
  * **_You don’t need to write custom code to automate your systems._**
  * **_Ansible will also figure out how to get your systems to the state you want them to be in._**

The playbook implements the following tasks:

- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
•	Specify a different group of machines:
```
  - name: Config elk VM with Docker
    hosts: elk
    become: true
    tasks:
```
•	Install Docker.io
```
  - name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present
 ```
•	Install Python-pip
```
  - name: Install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  # Use pip module (It will default to pip3)
  - name: Install Docker module
    pip:
      name: docker
      state: present
      `docker`, which is the Docker Python pip module.
  ```   
   
•	Increase Virtual Memory
```
 - name: Use more memory
   sysctl:
     name: vm.max_map_count
     value: '262144'
     state: present
     reload: yes
  ```   
     
•	Download and Launch ELK Docker Container (image sebp/elk)
```
 - name: Download and launch a docker elk container
   docker_container:
     name: elk
     image: sebp/elk:761
     state: started
     restart_policy: always
     
  ```
•	Published ports 5044, 5601 and 9200 were made available
```

     published_ports:
       -  5601:5601
       -  9200:9200
    -  5044:5044 
```


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

### ELKserver

![13 1](https://user-images.githubusercontent.com/20432932/161848056-fc5209f3-3e5a-4a52-99ac-eefa257b9a52.png)


### Jump-Box-Provisioner
 
![13 2](https://user-images.githubusercontent.com/20432932/161848081-ab4ae524-9122-44a2-a91f-65ceea0d75fd.png)

### Web-1
 ![13 3](https://user-images.githubusercontent.com/20432932/161848119-563d5315-41fc-4b35-afcb-4cf2ac7945a6.png)


### Web-2
 ![13 4](https://user-images.githubusercontent.com/20432932/161848149-74ce2614-5eac-429e-aaac-96f7c54f1e7e.png)


### DVWA-VM3
 
![13 5](https://user-images.githubusercontent.com/20432932/161848163-3cc71188-b974-4a81-b671-e29afff15808.png)



 ### Target Machines & Beats

 This ELK server is configured to monitor the following machines:
 -	Web-1: 10.1.0.5
 -	Web-2: 10.1.0.6
 -	DVWA-VM3: 10.1.0.7


 We have installed the following Beats on these machines:
- Filebeat
 
![Filebeat_data_successful](https://user-images.githubusercontent.com/20432932/161848194-27cdade7-0436-4a5e-bf1d-badfca95542c.png)

- Metricbeat
 
![Metricbeat_data_successful](https://user-images.githubusercontent.com/20432932/161848217-eb6f179e-88cc-474b-b77f-2943c2a1d947.png)


These Beats allow us to collect the following information from each machine:
 -	Filebeat will be used to collect log files from very specific files such as Apache, Microsoft Azure tools and web servers, MySQL databases.
 -	Metricbeat will be used to monitor VM status, per CPU core stats, per filesystem stats, memory stats and network stats.

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
•	Verify the Public IP address to see if it has changed. What is My IP?
•	If changed then update the Security Rules that uses the My Public IPv4.

SSH into the control node and follow the steps below:
- Copy the **_yml_** file to **_ansible folder_**.
- Update the **_config_** file to include **_remote users and ports_**.
- Run the playbook, and navigate to **_Kibana ((your IP Address):5601)_** to check that the installation worked as expected.

Answer the following questions to fill in the blanks:

- Which file is the playbook? 
  *	For Ansible create [My First Playbook]
  *	For Filebeat create [Filebeat Playbook]
  *	For Metricbeat create [Metricbeat Playbook] 
- Where do you copy it?
  *	**_/etc/ansible/_**
- Which file do you update to make Ansible run the playbook on a specific machine? 
  * **_	/etc/ansible/hosts (IP of the virtual machines)_**

- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
  * **_I have specified two separate groups in the /etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the 3 VMs that I will install filebeat to. The other group is named ELKserver which will have the IP of the VM I will install ELK to._**

- Which URL do you navigate to in order to check that the ELK server is running?
  * http://20.84.136.248:5601//app/kibana


As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

| COMMAND                                         | PURPOSE                                                |
|-------------------------------------------------|--------------------------------------------------------|
| `ssh-keygen`                                      | create a ssh key for setup VM's                        |
| `sudo cat .ssh/id_rsa.pub`                       `| to view the ssh public key                             |
| `ssh azadmin@Jump-Box-Provisioner IP address     `| to log into the Jump-Box-Provisioner                   |
| `sudo docker container list -a                   `| list all docker containers                             |
| `sudo docker start dremy_elbakyan                `| start docker container dremy_elbakyan                  |
| `sudo docker ps -a                               `| list all active/inactive containers                    |
| `sudo docker attach dremy_elbakyan               `| effectively sshing into the dremy_elbakyan container   |
| `cd /etc/ansible                                 `| Change directory to the Ansible directory              |
| `ls -laA                                         `| List all file in directory (including hidden)          |
| `nano /etc/ansible/hosts                         `| to edit the hosts file                                 |
| `nano /etc/ansible/ansible.cfg                   `| to edit the ansible.cfg file                           |
| `nano /etc/ansible/pentest.yml                   `| to edit the My-Playbook                                |
| `ansible-playbook [location][filename]           `| to run the playbook                                    |
| `sudo lsof /var/lib/dpkg/lock-frontend           `| unlocking a locked file                                |
| `ssh ansible@Web-1 IP address                    `| to log into the Web-1 VM                               |
| `ssh ansible@Web-2 IP address                    `| to log into the Web-2 VM                               |
| `ssh ansible@DVWA-VM3 IP address                 `| to log into the DVWA-VM3 VM                            |
| `ssh ansible@ELKserver IP address                `| to log into the ELKserver VM                           |
| `exit                                            `| to exit out of docker containers/Jump-Box-Provisioners |
| `nano /etc/ansible/ansible.cfg                   `| to edit the ansible.cfg file                           |
| `nano /etc/ansible/hosts                         `| to edit the hosts file                                 |
| `nano /etc/ansible/pentest.yml                   `| to edit the My-Playbook                                |
| `ansible-playbook [location][filename]           `| to run the playbook                                    |
| `sudo apt-get update                             `| this will update all packages                          |
| `sudo apt install docker.io                      `| install docker application                             |
| `sudo service docker start                       `| start the docker application                           |
| `sudo systemctl status docker                    `| status of the docker application                       |
| `sudo systemctl start docker                     `| start the docker service                               |
| `sudo docker pull cyberxsecurity/ansible         `| pull the docker container file                         |
| `sudo docker run -ti cyberxsecurity/ansible bash` | run and create a docker container image                |
| `ansible -m ping all                          `   | check the connection of ansible containers             |
| `curl -L -O [location of the file on the web]`    | to download a file from the web                        |
| `dpkg -i [filename]`          | to install the file i.e. (filebeat & metricbeat)       |
| `http://20.84.136.248:5601//app/kibana`           | Open web browser and navigate to Kibana Logs           |
| `nano filebeat-config.yml`                  | create and edit filebeat config file                   |
| `nano filebeat-playbook.yml`                   | write YAML file to install filebeat on webservers      |
| `nano metricbeat-config.yml`                    | create metricbeat config file and edit it              |
| `nano metricbeat-playbook.yml`                    | write YAML file to install metricbeat on webservers    |

[My First Playbook]: https://github.com/maitri3997/ELK-Deployemnt-Project/blob/main/Ansible/Docker/pentest.yml
[Hosts]: https://github.com/maitri3997/ELK-Deployment-Project/blob/main/Ansible/Elk-Stack/hosts
  	[Ansible Configuration]: https://github.com/maitri3997/ELK-Deployemnt-Project/blob/main/Ansible/Elk_Stack/ansible.cfg
  	[Ansible ELK Installation and VM Configuration]: https://github.com/maitri3997/ELK-Deployment-Project/blob/main/Ansible/Elk-Stack/install-elk.yml
  	[Filebeat Config]: https://github.com/maitri3997/ELK-Deployemnt-Project/blob/main/Ansible/Filebeat/filebeat_config.yml
  	[Filebeat Playbook]: https://github.com/maitri3997/ELK-Deployment-Project/blob/main/Ansible/Filebeat/filebeat_playbook.yml
  	[Metricbeat Config]: https://github.com/maitri3997/ELK-Deployment-Project/blob/main/Ansible/Metricbeat/metricbeat-config.yml
  [Metricbeat Playbook]: https://github.com/maitri3997/ELK-Deployment-Project/blob/main/Ansible/Metricbeat/metricbeat-playbook.yml

