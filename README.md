# Active Directory Enterprise Network Deployment

<h2>Description</h2>
This project showcases a full-scale internal network setup for an enterprise environment using Active Directory Domain Services, all within a virtual lab. It walks through the detailed process of creating departments and user accounts tailored to organizational needs, all managed centrally through a domain controller.

In this configuration, devices within the internal network access the public internet exclusively through the main domain controller. This ensures that network activity is tightly governed by administrators, with permissions and controls defined by their roles and overseen by a superadmin. <br />


<h2>Languages and Utilities Used</h2>

- <b>Windows Server Manager</b>
- <b>Active Directory Domain Services</b> 
- <b>PowerShell</b>
- <b>Python</b> 


<h2>Environments Used </h2>

- <b>Windows Server (2019)</b>
- <b>Windows 10 (Pro)</b>
- <b>Active Directory Domain Services (AD DS)</b>
- <b>Internal Network (via VirtualBox NAT or Host-Only Adapter)</b>
- <b>DNS & DHCP Services</b>
- <b>Remote Desktop Services (RDP)</b>

<p align="left">

<h2>Major objectives in this project: </h2>

1. Establish a Virtualized Enterprise Network Environment
    - Set up a fully functional internal network using VirtualBox, simulating an enterprise infrastructure with multiple virtual machines.
3. Deploy and Configure Active Directory Domain Services (AD DS)
    - Install Windows Server 2019 and configure it as a domain controller to manage users, devices, and organizational units centrally.
5. Design Organizational Structure with Departments and Users
    - Create a realistic enterprise hierarchy by setting up departments and user accounts based on business roles and access needs.
7. Implement Network Access Control via Domain Controller
    - Ensure all internal devices route internet access through the domain controller, allowing administrators to monitor and govern traffic.
9. Utilize Administrative Tools for System Management and Automation
    - Leverage PowerShell, Server Manager, and Python scripts to automate tasks, enforce policies, and maintain system integrity across the network.

<hr>


<h1>Initial Setup</h1><br />

<p align="left">
<h2>Step 1: VirtualBox setup</h2><br />
	
For the required environment and number of devices and systems, we need a virtual environment where we will be able to set up our devices in the network. For that, we are going to use VirtualBox VMware. 
<a href="https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html">Oracle VirtualBox</a><br/>
</p>


![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20151900.png?raw=true)



<h3>Disclaimer</h3>
- This content is intended solely for educational purposes. Any replication, in whole or in part, for malicious or unethical use may constitute a legal offense.
- “Screenshots are for demonstration only. Please do not reuse without permission.”

