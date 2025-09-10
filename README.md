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
<h2>Step 1: Downloads (Tools and Programs)</h2><br />
	
For the required environment and number of devices and systems, we need a virtual environment where we will be able to set up our devices in the network. For that, we are going to use VirtualBox VMware.
<a href="https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html">Download Oracle VirtualBox</a><br/>
</p>

<p>
After that, we will need to download both 
<a href="https://www.microsoft.com/en-us/software-download/windows10">Windows 10</a> 
and 
<a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019">Windows Server 2019</a>.
</p>

  - Make sure to choose "ISO" files to download.
  - Ideally have them downloaded to your desktop to find them easier in the following steps.

Once you have them both donwnloaded we can proceed.

<br />
<hr>


<h2>Step 2: Creating the VMs inside VirtualBox</h2><br />

Open up VirtualBox
  - Select "NEW"
  - Name each of them something easy to remember.
    - For example "*Domain Controller*" and "*Client1*"
  - Select the correct ISO image from wherever they are on your disk 
    - (You should have downloaded to your Desktop)
  - Click ""Next""

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20151900.png?raw=true)

Now we will setup the Username(s) and Password(s)

  - Set up a basic username and password, something easy for testing. 
  - Allocate between 2GB to 4GB of RAM depending on your system. 
  - You can leave the rest of the settings as they are.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20152044.png?raw=true)

Let’s tweak a few general settings to make working with the VMs smoother.
  - Enable Bidirectional Clipboard and Drag-and-Drop so you can easily move files and text between your host and the virtual machines.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20152314.png?raw=true)

<br />
<hr>

<h2>! The next steps apply only to the Domain Controller VM: !</h2>

  - Add a second network adapter. 
  - Set it to Internal Network 
    - This lets your virtual machines talk to each other privately.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20154018.png?raw=true)

Double click on the VM to boot and wait for it to go through its boot-up process. 

Proceed till you are given to choose an “Operating System”
  - Choose Windows Server 2019 (DESKTOP EXPERIENCE)
    - If you do not select Desktop you will be left with the CMD and no UI
  - Click "Next"  
  - Accept the license terms


![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20155834.png?raw=true)

Choose Custom Install when prompted.
  - Select the default disk that appears and click Install.
  - Now sit back and let the installation run.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20160051.png?raw=true)

Once it’s done, you’ll be asked to set a password. 
- Pick something simple and easy to remember for now.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20161027.png?raw=true)

Success! Your server is up and running.

<h5> Tip (Optional)</h5>

Once inside the machine find the top menu that says "Devices" → Insert Guest Additions CD. This lets the VM resize automatically and improves the user interface.
- Navigate to File Explorer and locate the VirtualBox Additions file. 
- Open it and choose the AMD64 installer.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20161504.png?raw=true)

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20161600.png?raw=true)

Run the installer. Now we manually shut down and restart the VM.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20161854.png?raw=true)

<br />
<hr>

<h2>Step 3: Configuring the Internal NIC </h2><br />

The external NIC will be automatically assigned by your home router so we don't need to configure it.

We need to configure the internal NIC so the machines can communicate with each other.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20165551.png?raw=true)

Go to Ethernet Settings → Change Adapter Options

- Find the adapter with the auto-assigned DHCP IP and rename it to "__Internal__" 

- Rename the other one to "__Internet__"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20165909.png?raw=true)

You can see that the one below has an assigned IP from your home/office network.

That will be our "__External__" network - No need to change anything

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20170119.png?raw=true)

Select the “__Internal__” network and open the Properties menu. 
- Choose TCP/IPv4 and enter the following:
  - IP Address: 172.16.0.1
  - Subnet Mask: 255.255.255.0
  - Default Gateway: (leave blank)
  - Preferred DNS: 127.0.0.1 (loopback address)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20173717.png?raw=true)

At this point, both NICs are configured.

<br />
<hr>

<h2>Step 4: Creating the Domain and Users </h2>

Now we need to open Server Manager and select Add Roles and Features

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20174309.png?raw=true)

- Click "Next" three times until you reach the "Server Roles" tab 
 - Select Active Directory Domain Services 
 - Click Add Features

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20174614.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20174705.png?raw=true)

- Click "Next" through each section until you reach the Install button .
- Click Install and wait, it may take a few minutes.

Once it finishes, close the window

<hr>


Back in Server Manager, you’ll see a yellow flag 

- Click "Promote this server to a domain controller"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20174934.png?raw=true)

- Choose "Add a new forest" 
- Name it however you like, for example: mydomain.com
- Click "Next"
  - Set a password

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20175332.png?raw=true)

- Click Next all the way through until you’re prompted to installon the "Prerequisites Check"
- Click Install

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20175539.png?raw=true)

The server will sign out and reboot automatically

Once it reboots, log back in 

You’ll notice the login now says "MYDOMAIN\Administrator"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20180317.png?raw=true)

Go ahead and log back in
- Click the Start menu and go to Windows Administrative Tools. 
- From there, open Active Directory Users and Computers.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20180722.png?raw=true)


<hr>

<h2>Step 4: Creating an Organizational Unit </h2><br />

Let’s create an organizational unit to store our admin account. You can think of it like a folder inside Active Directory.

Make sure you have Active Directory open for the next steps

- Right-click on mydomain.com 

- Select New Organizational Unit 
 
- Name it "_Admins"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-10%20174545.png?raw=true)

- Once the folder is created right-click on it 
- Select New 
- Select User

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20181526.png?raw=true)

- Type in the desired name.
  - For the logon name, it’s common practice to prefix with a- to indicate an admin role (e.g., a-jonai)
- Click Next

In our case we went with "<u>-aJdoe</u>" for "John Doe"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20181628.png?raw=true)

- Choose a simple password like "Password1" 
- Uncheck User must change password at next logon 
- Check Password never expires

Click Next → Finish

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20181757.png?raw=true)

Now we’ve created the user, and even though it has the "-a"  it’s not an admin yet

Let’s fix that.

- Right-click the user 
- Then click Properties

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20181933.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20182134.png?raw=true)

- Go to the "Member Of" tab 
- Click Add 
- Type "Domain Admins" 
- Click OK 
- Then Apply

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20182232.png?raw=true)

You now have a Domain Admin account!

To confirm, let's sign out of the server.

You should see an option to sign in as another user
- Use the credentials you just created

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20182740.png?raw=true)

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<h2>Step 3: Configuring the Internal NIC </h2><br />

<br />
<h3>Disclaimer</h3>
- This content is intended solely for educational purposes. Any replication, in whole or in part, for malicious or unethical use may constitute a legal offense.
- “Screenshots are for demonstration only. Please do not reuse without permission.”

