# Network-SOC-Lab

## Overview

### Scenario
You have been contracted by a firm, Morgan Maxwell Real Estate, to overhaul and harden part of their IT infrastructure and systems. The main concern is creating a secure network that is protected from attacks over the internet. They would also like to have some standard web filtering established that will block employees from accessing things like social media or adult sites. Finally, they would like to have a SOC where they can easily analyze and respond to threats or view network activity without knowing much about system administration or networking.

### Purpose and Skills Learned

- To get hands-on networking skills.
- To get hands-on system administration skills.
- To learn how to configure networks, firewalls, servers, and systems.
- To get virtualization/hypervisor configuration experience.
- To learn how to create a SOC and monitor for potential threats.
- To learn how to harden a network and its (virtual) devices.
- To gain experience using various tools involved in networking and cybersecurity.

### Tools and Services Used
#### Network/General
- nmap
- ifconfig
- ssh
- Filezilla
#### OS
- Kali
- Windows
- Ubuntu
#### Virtualization
- VirtualBox
#### SIEM
- Wazuh
#### Firewall/NG Firewall
- OPNsense
- Zenarmor
#### IDS/IPS
- OPNsense/Suricata
#### IR/XDR
- TheHive
- Cortex
#### Containerization
- Docker Compose
#### Proxy
- OPNsense

## Steps
### 1. OPNsense Firewall Installation
- Installed VirtualBox
- Created a Virtual Machine with OPNsense Firewall installed onto it
![1 1](https://github.com/user-attachments/assets/a114e1a2-6999-48bc-aaf2-559d673379ac)
- Configured OPNsense with the correct settings to enable the WAN and LAN connections as well as the online interface’s IP gateway address.
![1 2](https://github.com/user-attachments/assets/571b1d7a-b3d6-4c09-af59-775c80ff6119)
- Created a Virtual Machine via Kali Linux virtual disk image and connect it to the LAN
![1 3](https://github.com/user-attachments/assets/17e7e443-d05b-4ddc-815d-0bcb21877cca)
### 2. Configure OPNsense Firewall
- Configured OPNsense within the online interface to use the WAN and IPv4 connections.
- Updated OPNsense to the latest version.
- Enabled Services 
### 3. Enable and Configure Suricata-based IDS/IPS Services on OPNsense
- From the Administration tab, enable the IDS, IPS mode, Promiscuous mode, and syslog alerts.
![3 1](https://github.com/user-attachments/assets/078bbd1c-1167-44c8-9fdb-c3df43d1266c)
- Changed interfaces to LAN
- Changed Home networks to the appropriate IP address (LAN Subnet)
![3 2-3 3](https://github.com/user-attachments/assets/043e7037-e437-48d3-bc65-da6404575c6b)
- Created a file named customnmap.rules with a Suricata based rule that would detect a synstealth scan on the LAN network and provide an alert with a message.
![3 4](https://github.com/user-attachments/assets/891a4557-37d3-42a8-9184-f0ca67c4a178)
- Created a file named customnmap.xml that has XML code that will tell the OPNsense firewall to check the above file for the IDS rule.
![3 5](https://github.com/user-attachments/assets/c504de06-c0f6-4cf3-ae5f-dd6e53c20b36)
- Enabled Secure Shell on OPNsense, permitted root user login, and changed the SSH port to 22.
![3 6](https://github.com/user-attachments/assets/4fc53f9d-8f9c-424d-ba0f-8b136e1ddc1c)
- Used Filezilla to transfer the xml file into OPNsense
![3 7](https://github.com/user-attachments/assets/24c0f672-e77e-48bb-8f0a-3a7c670cb4fc)
- Created a basic HTTP server for the directory where the ruleset is located
![3 8](https://github.com/user-attachments/assets/22976b8c-d488-46ee-9776-a84f7cdbd557)
- From OPNsense, downloaded the Suricata ruleset and installed it.
![3 9](https://github.com/user-attachments/assets/eb2a93c5-8072-4707-836e-9f8c30289f34)
- Used nmap to perform a synstealth scan on the LAN network in order to test if the IDS is working.
![3 10](https://github.com/user-attachments/assets/d6c3b352-dcaa-434e-9624-77847b6ff3f1)
### 4. Proxy Configuration for Web Filtering
- Created then installed a Trusted Certificate Authority on the System
- Enabled and configured a transparent web proxy with a firewall port forward rule
- Configured the web filtering features of the proxy including a blacklist imported from a URL
- Downloaded ACLs for the proxy
- Setup and repositioned the firewall and its rules
- Tested the web filtering using a browser
### 5. Firewall High Availability and CARP, pfSync Configuration
- Cloned the OPNsense firewall VM in VirtualBox
![5 1](https://github.com/user-attachments/assets/fef57c4a-008d-471c-a818-45910018aec0)
- Created a NAT network within VirtualBox
![5 2](https://github.com/user-attachments/assets/054ddaa3-587a-4339-a23e-b4d84a0eed75)
- Created an internal network for pfsync
![5 3](https://github.com/user-attachments/assets/597e60a4-6a80-4c75-afc6-d33959375cf6)
- Configured the back-up firewall with a different IP address
![5 3](https://github.com/user-attachments/assets/3f094729-dada-4164-a671-6689bdd7313b)
- Configured both firewalls within their respective web GUIs with new rules to allow intercommunication
![5 5](https://github.com/user-attachments/assets/9edc5661-1c68-4793-bd0c-161c5ce498ba)
- Created virtual IPs for the WAN and LAN networks using the CARP protocol
![5 6](https://github.com/user-attachments/assets/0c33e2de-9f33-4078-9c95-585e86890f66)
- Configured the High Availability settings within the master and back-up firewalls
![5 7](https://github.com/user-attachments/assets/ae1ffe90-1fcb-403a-9e7b-627b4b5b50ae)
- Configured the NAT settings within both firewalls to use the VIPs created earling, including setting the skew for each firewall
![5 8](https://github.com/user-attachments/assets/f71a6215-7dcd-41c1-8a25-a0217b33aca2)
- Tested the setup by simulating a connection failure with the master firewall by virtually disconnecting network cables within VirtualBox.
### 6. Multi-Wan Failover and/or Load Balancing Configuration
- Created two new NAT networks for each WAN ISP interface within VirtualBox’s network settings
![6 1](https://github.com/user-attachments/assets/5a3ba99a-0558-44ef-9724-6078425985cb)
- Configured the OPNsense Firewall VM’s network settings to use the new NAT networks
![6 2](https://github.com/user-attachments/assets/51d62c13-9fff-4e6c-9f32-0cc7dbd7f488)
- Changed OPNsense’s network interface assignments within the GUI to support both NAT networks
![6 3](https://github.com/user-attachments/assets/c193a5ff-d659-4652-8d71-e88d95b9c9c4)
- Configured WAN1 to have priority 1 and WAN2 with priority 254
![6 4](https://github.com/user-attachments/assets/222ac5e0-d514-4240-9592-0554083e25aa)
- Setup a gateway group with tier 1 and tier 2 networks with the tier 2 being a failover network
![6 5](https://github.com/user-attachments/assets/1b538cc9-01e6-4f59-baf3-baede19e91ea)
- Configured DNS settings including DNS servers and allowing gateway switching
![6 6](https://github.com/user-attachments/assets/599a5038-6ddf-4b72-bd08-a4ae97254688)
- Edited firewall rules to allow for the use of the gateway group, and added a new pass rule for the LAN network
![6 7](https://github.com/user-attachments/assets/febea85c-5567-4d95-afb3-b2db5dcfd9ba)
- Used traceroute to test out the Multi-WAN setup after simulating a failure of WAN1
### 7. Enabling Next Generation Firewall Features using Zenarmor
- Installed NG plugins (Zenarmor) on OPNsense
![7 1](https://github.com/user-attachments/assets/e2f5b2e6-cd70-4e97-b0ce-b87eaa9ab6f7)
- Continued through the installation wizard
![7 2](https://github.com/user-attachments/assets/261e5bf3-2855-4c84-a7c2-b51473b4b19c)
- Updated to the latest version
![7 3](https://github.com/user-attachments/assets/0f283dac-e315-4b23-af87-d784dacbcfe1)
### 8. Wazuh SIEM/XDR Installation and Setup
- Created a Wazuh VM with VirtualBox
![8 1](https://github.com/user-attachments/assets/fa3e7c20-0953-4a7c-ad92-cac3a1c2943d)
- Configured the network settings within Wazuh to match my network’s environment
![8 1](https://github.com/user-attachments/assets/bb31e06c-ecaa-4e97-9d3c-b5859a3eacff)
- Created a new Wazuh Windows agent that’s connected to my network
![8 3](https://github.com/user-attachments/assets/472fb9f0-6cf3-4583-8075-f31edf7fae51)
### 9. TheHive, Cortex, MISP Installation, Configuration and Integration
- Created an Ubuntu server with Docker Compose installed on it using VMware.
- Using a Windows OS, connected to the Ubuntu server via SSH to create a docker-compse.yaml file.
![9 2](https://github.com/user-attachments/assets/fa488bf4-f328-40ea-af16-a896dc18f0dd)
- Using Visual Studio Code, defined TheHive, Cortex, and MISP (as well as Cassandra, Elasticsearch, MinIO) tools/services as containerized applications in YAML, then copied it over into the created file.
![9 3](https://github.com/user-attachments/assets/edca889d-da1e-4131-be30-1ce421799c53)
- Deployed and installed the above applications.
![9 4](https://github.com/user-attachments/assets/ba428ff0-1727-4a57-bb4e-0c3b579b39e7)
- Logged into MISP and configured users, auth keys, and settings as needed.
![9 5](https://github.com/user-attachments/assets/7e01331c-c80e-43a0-bf65-24b0060b917f)
- Logged into Cortex and created an API key for users
![9 6](https://github.com/user-attachments/assets/77408247-d770-45f7-9a3b-2053376f20cc)
- Updated the YAML configuration file to work with the new generated auth and API keys, and an application.conf file for MISP to create a whitelist of information it receives
![9 7](https://github.com/user-attachments/assets/86872353-74a8-4edf-8172-10b1338060ed)
- Logged into TheHive and finished configurations for the synchronization of the three applications
![9 8](https://github.com/user-attachments/assets/4c2839e2-0886-47f7-ab0d-7990cb350a7c)
- Retrieved API keys for VirusTotal and MalwareBazaar and setup analyzers within Cortex
![9 9](https://github.com/user-attachments/assets/b1c2a877-b578-4ab5-8728-46c043e14ca2)
### 10. Integrating Wazuh and TheHive
- Installed Python & PIP on the Wazuh server
- Installed TheHive Python module using PIP
- Created a custom integration script
- Set up permissions and ownership
- Enabling integration in the Wazuh manager config file
