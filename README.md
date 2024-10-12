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
- Configured OPNsense with the correct settings to enable the WAN and LAN connections as well as the online interface’s IP gateway address.
- Created a Virtual Machine via Kali Linux virtual disk image and connect it to the LAN
### 2. Configure OPNsense Firewall
- Configured OPNsense within the online interface to use the WAN and IPv4 connections.
- Updated OPNsense to the latest version.
- Enabled Services 
### 3. Enable and Configure Suricata-based IDS/IPS Services on OPNsense
- From the Administration tab, enable the IDS, IPS mode, Promiscuous mode, and syslog alerts.
- Changed interfaces to LAN
- Changed Home networks to the appropriate IP address (LAN Subnet)
- Created a file named customnmap.rules with a Suricata based rule that would detect a synstealth scan on the LAN network and provide an alert with a message.
- Created a file named customnmap.xml that has XML code that will tell the OPNsense firewall to check the above file for the IDS rule.
- Enabled Secure Shell on OPNsense, permitted root user login, and changed the SSH port to 22.
- Used Filezilla to transfer the xml file into OPNsense
- Created a basic HTTP server for the directory where the ruleset is located
- From OPNsense, downloaded the Suricata ruleset and installed it.
- Used nmap to perform a synstealth scan on the LAN network in order to test if the IDS is working.
### 4. Proxy Configuration for Web Filtering
- Created then installed a Trusted Certificate Authority on the System
- Enabled and configured a transparent web proxy with a firewall port forward rule
- Configured the web filtering features of the proxy including a blacklist imported from a URL
- Downloaded ACLs for the proxy
- Setup and repositioned the firewall and its rules
- Tested the web filtering using a browser
### 5. Firewall High Availability and CARP, pfSync Configuration
- Cloned the OPNsense firewall VM in VirtualBox
- Created a NAT network within VirtualBox
- Created an internal network for pfsync
- Configured the back-up firewall with a different IP address
- Configured both firewalls within their respective web GUIs with new rules to allow intercommunication
- Created virtual IPs for the WAN and LAN networks using the CARP protocol
- Configured the High Availability settings within the master and back-up firewalls
- Configured the NAT settings within both firewalls to use the VIPs created earling, including setting the skew for each firewall
- Tested the setup by simulating a connection failure with the master firewall by virtually disconnecting network cables within VirtualBox.
### 6. Multi-Wan Failover and/or Load Balancing Configuration
- Created two new NAT networks for each WAN ISP interface within VirtualBox’s network settings
- Configured the OPNsense Firewall VM’s network settings to use the new NAT networks
- Changed OPNsense’s network interface assignments within the GUI to support both NAT networks
- Configured WAN1 to have priority 1 and WAN2 with priority 254
- Setup a gateway group with tier 1 and tier 2 networks with the tier 2 being a failover network
- Configured DNS settings including DNS servers and allowing gateway switching
- Edited firewall rules to allow for the use of the gateway group, and added a new pass rule for the LAN network
- Used traceroute to test out the Multi-WAN setup after simulating a failure of WAN1
### 7. Enabling Next Generation Firewall Features using Zenarmor
- Installed NG plugins (Zenarmor) on OPNsense
- Continued through the installation wizard
- Updated to the latest version
### 8. Wazuh SIEM/XDR Installation and Setup
- Created a Wazuh VM with VirtualBox
- Configured the network settings within Wazuh to match my network’s environment
- Created a new Wazuh Windows agent that’s connected to my network
### 9. TheHive, Cortex, MISP Installation, Configuration and Integration
- Created an Ubuntu server with Docker Compose installed on it using VMware.
- Using a Windows OS, connected to the Ubuntu server via SSH to create a docker-compse.yaml file.
- Using Visual Studio Code, defined TheHive, Cortex, and MISP (as well as Cassandra, Elasticsearch, MinIO) tools/services as containerized applications in YAML, then copied it over into the created file.
- Deployed and installed the above applications.
- Logged into MISP and configured users, auth keys, and settings as needed.
- Logged into Cortex and created an API key for users
- Updated the YAML configuration file to work with the new generated auth and API keys, and an application.conf file for MISP to create a whitelist of information it receives
- Logged into TheHive and finished configurations for the synchronization of the three applications
- Retrieved API keys for VirusTotal and MalwareBazaar and setup analyzers within Cortex
### 10. Integrating Wazuh and TheHive
- Installed Python & PIP on the Wazuh server
- Installed TheHive Python module using PIP
- Created a custom integration script
- Set up permissions and ownership
- Enabling integration in the Wazuh manager config file
