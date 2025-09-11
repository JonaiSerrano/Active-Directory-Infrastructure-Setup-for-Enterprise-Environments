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
  - At least 50gb of allocated space

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213329.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213256.png?raw=true)

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

<b>Once it finishes, close the window</b>

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

<h2>Step 5: Creating an Organizational Unit </h2><br />

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
  - Username = -ajdoe
  - Password = Password1

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20182740.png?raw=true)

<b>🎉 Success! You’ve officially created your domain and admin account.</b>

<br />
<hr>

<h2>Step 6: INSTALL RAS/NAT </h2><br />

Next, we’ll install RAS/NAT (Remote Access Server & Network Address Translation) on the domain controller

This lets the client VM access the internet through the domain controller, even on a private virtual network.

The image below shows the path we are taking from client1 all the way to the Domain Controller then accessing the outside internet.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20184239.png?raw=true)

- Go back to Server Manager
- Add Roles and Features
  - Skip ahead to Server Roles
- Select Remote Access Click Next

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20184439.png?raw=true)

- Keep clicking Next until you reach:
- Role Services 
- Then Select Routing 
- Click Add Features 
- Click Next
- Click Install

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20185514.png?raw=true)

Once installed, go back to Server Manager 
- On the top right click "Tools" 
- Then click "Routing and Remote Access"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20190148.png?raw=true)

<b>This single next step is for those that have a Installation Wizard come up:</b>
- Click "next" through the wizard until you reach Configuration

Now you should see the following:
- Right click on "DomainController"
- Then click "Configure and Enable Routing and Remote Access"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20190319.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20190358.png?raw=true)

- After it opens click Next to go to the next page

- Select Network Address Translation (NAT) 
  - This allows internal clients to connect to the internet using one public IP

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20190520.png?raw=true)

- Select the Internet interface you created earlier

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20190811.png?raw=true)

- Click Next

- Review the settings and click Finish

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20191220.png?raw=true)

<b>As you can see below, everything was configured correctly. We succesfully created RAS/NAT</b>

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20191400.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20191612.png?raw=true)
<br />
<hr>

<h2>Step 7: Configuring DHCP </h2><br />

To allow Windows clients to access the internet, we need to install DHCP. The DHCP server automatically assigns IP addresses to clients so they can connect to the network.


The DHCP server assigns IP addresses to clients so they can navigate the web

<br />

- Open Server Manager
- Add Roles and Features Click Next until you reach the Server Roles section
- Select DHCP Server Click Add Features
- Next 
- Install

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20194338.png?raw=true)

- Once installed, go to the Tools tab in the top-right corner of Server Manager 
- Find and click on DHCP

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20194800.png?raw=true)

Inside the DHCP console, you’ll see both IPv4 and IPv6 listed 

- Right-click on IPv4 and select New Scope
- Click Next 

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20195043.png?raw=true)

As you can see this is the scope we will be following.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20195257.png?raw=true)

For the name, use something descriptive like the IP range shown in the scope:

- Example: 172.16.0.100-200
  - Don't worry about a description
- Click Next

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20195558.png?raw=true)

  - Set the following:
    - Start IP: 172.16.0.100
    - End IP: 172.16.0.200
    - Length: 24
    - Subnet Mask: 255.255.255.0
  - Click Next

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20195638.png?raw=true)

Skip the Exclusions page

On the Lease Duration page, leave the default settings
- If you're setting this up for a café or business with short-term users, you can reduce the lease time later.
- We will ignore and press next

On the DHCP Options page, leave "Yes" selected 
- This lets clients know which servers to use

On the Router page, enter the IP of the internal NIC: 
- 172.16.0.1 
- Click Add
- Click Next

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20200210.png?raw=true)

On the Domain Name and DNS Servers page
- Leave the domain as-is 
- Use the domain controller as your DNS server
- Click Next

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20200405.png?raw=true)

- On the Active Scope page
- Select Yes then Next
- Select “Finish”

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20200444.png?raw=true)

Once you have it added, Right click on your domain inside of the DHCP window and click Authorize
- Right click and press "Refresh" and you will see your scope, leases, reservations, etc

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20200649.png?raw=true)

<br>
<hr>

<h2>Step 8: PowerShell Scripts to Create Users </h2><br />

To save time, we’ll use a PowerShell script to bulk-create users.

Open Internet Explorer and paste the following link:
- https://github.com/joshmadakor1/AD_PS/archive/master.zip
    - This is an open-source PowerShell script by Josh Madakor. It will begin downloading immediately.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20202502.png?raw=true)

Once prompted, click "Save As" and choose Desktop as the location. This makes it easier to find. Then extract the contents to the desktop as well

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20203201.png?raw=true)

<hr>


<h4>If for some reason you cannot access the link through Internet Explorer follow the next step</h4>

<h4>*If you CAN access the link, skip this step*</h4>

Navigate to your Server Manager and click on
- 1 Configure this local server
- Find the "IE Enhanced Security Configuration"
  - We are turning them both off
- You will now allow you to open the link shown above

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20200921.png?raw=true)

<hr>

Open the Names.txt file 
- You’ll see a pre-generated list of names for the user accounts

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20205732.png?raw=true)

Next, run PowerShell ISE as Administrator

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20205914.png?raw=true)

Click the folder icon to open the Create Users PowerShell script

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20210031.png?raw=true)

Before running it, we need to allow script execution 

Run the following command:
- Set-ExecutionPolicy Unrestricted
- Then when prompted. Select "Yes to All"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20210401.png?raw=true)

This PowerShell script generates a large number of random user accounts with synthetic first and last names. 

Each account is created in Active Directory with a default password and placed in a specific organizational unit. 

It uses a loop and a custom name generator to automate bulk provisioning.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20210147.png?raw=true)

Last thing before we execute 
- Navigate to the correct directory

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20210811.png?raw=true)

Once you’re in the right folder, press Play at the top of PowerShell
- If prompted, click Run Once

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20211006.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20211119.png?raw=true)

The script is fully customizable—you can tweak it to add or remove specific attributes when creating accounts.

As we can see, all the users have been successfully added inside the _USERS folder within Active Directory.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20212548.png?raw=true)

<br>
<hr>

<h2>Step 9: Creating the Windows 10 Virtual Machine </h2><br />

Start by booting up your Windows 10 VM.

Open up VirtualBox
  - Select "NEW"
  - Name the VM "*Client1*"
  - Select the correct ISO image from wherever they are on your disk 
    - (You should have downloaded to your Desktop)
      - Don't choose the Server ISO!
  - Click ""Next""
  - Ideally 2-4 cores
  - Allocate between 2GB to 4GB of RAM depending on your system.
  - Same as before 50gb of allocated storage 
  - You can leave the rest of the settings as they are.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213329.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213256.png?raw=true)

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20152044.png?raw=true)

Remember to also enable Bidirectional Clipboard and Drag-and-Drop so you can easily move files and text between your host and the virtual machine.

![Screenshot](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20152314.png?raw=true)

In the Network tab make sure you have the "Internal Network" selected

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213525.png?raw=true)

<br>
<hr>
<br>


Follow the installation instructions like normal to install Windows 10 on the VM.

When prompted choose the following:
- Select “I don’t have a product key” 
- <b>CHOOSE WINDOWS 10 PRO</b>
  - Windows 10 __HOME__ won’t allow you to join the company domain

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20213729.png?raw=true)

- Select setup for __Personal Use__

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20215112.png?raw=true)

- Go ahead and choose "Offline Account" or "Local Account"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20215235.png?raw=true)

- You can choose to name it "User" for now

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20215340.png?raw=true)

Once it’s running, open Network Settings and make sure it’s connected to the Internal Network, the same one your domain controller is using.

You should see it automatically be connected to "mydomain.com"

That means that we succesfully have bridged the computer's internal NIC to the NIC on the Domain Controller.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20220328.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-08%20184239.png?raw=true)

Now, Navigate to Settings and choose
- Rename this PC (Advanced)
- Click on CHANGE 
- Name the pc
   - CLIENT1
- Member of:
  - mydomain.com (or the name of your domain)
- Press Okay

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20133656.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20134245.png?raw=true)

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20134405.png?raw=true)

After that you need to login with the Admin account and password we created originally

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20134541.png?raw=true)

Success! We are now officially a part of the Domain!

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20134615.png?raw=true)

Go ahead and restart the VM for the changes to take effect.

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20135824.png?raw=true)

We will now attempt to login using the names we imported using Powershell

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20141554.png?raw=true)

Remember the Password that you created (Password1) ? 

You need to use it to sign in with any of the Usernames we populated.

Here I am using "kbutcher"

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20151828.png?raw=true)

Once you see windows setting up for the first time you'll know everything went correctly!

https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20151850.png?raw=true

To finalize and make sure you are logged in to the correct profile:
- Go to CMD and type
   - Whoami

![alt text](https://github.com/JonaiSerrano/project-screenshots-private/blob/main/Screenshot%202025-09-09%20151954.png?raw=true)

We have successfully logged in with one of the usernames!

To double check DHCP run
- ipconfig /release
- ipconfig /renew

Then type ipconfig, you should see a new IP come up


To check your DNS settings try running:
- ping mydomain.com

That will let you know if the server is responsive

Congratulations! The ADE is now fully functional:

Domain Controller with AD, DNS, DHCP, RAS/NAT

Windows 10 client joined to the domain

Connectivity and services verified



<br />
<h3>Disclaimer</h3>
- This content is intended solely for educational purposes. Any replication, in whole or in part, for malicious or unethical use may constitute a legal offense.

“Screenshots are for demonstration only. Please do not reuse without permission.”

JNS
<hr>
