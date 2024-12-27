<div align="center">
  <h2>Monitor Active Directory Server Using Splunk</h2>
</div>

# Project Overview
This project demonstrates the setup of a virtualized environment with Active Directory on a Windows Server, log collection using Splunk, and the simulation of a Man-In-The-Middle (MITM) attack to analyze security events. It highlights network configuration, user authentication, and log monitoring, showcasing practical skills in cybersecurity operations and incident response.
# Pre-requisites 
- Hypervisor( virtual box, vmware)
- Windows 10 (Target machine
- Windows server 2022 (Active Directory)
- Ubuntu server (for splunk)
- Kali Linux (Attack machine)
- Splunk(server and universal forwarder)
# NAT connection
For connecting all the vms, I have created a custom NAT network in virtual box, also allocated static ip addresses to each virtual machines.
NAT network subnet "192.168.10.0/24".
<div align="center">
  <img src="https://github.com/user-attachments/assets/3377617b-d9b1-4b7c-a6df-d981e43cf4bf">
</div>

# Ubuntu server
Ubuntu Server was chosen for hosting Splunk due to its lightweight and efficient performance, making it ideal for server environments. Its compatibility with Splunk, open-source nature, and cost-effectiveness make it a practical choice for projects. Known for its stability, security, and ease of use, Ubuntu simplifies the setup and management of server applications. Additionally, its scalability and seamless integration into virtualized environments like VirtualBox enhance its suitability for log monitoring. The vast community support and documentation further ensure quick troubleshooting, while providing hands-on Linux experience crucial for cybersecurity professionals. 
For configuration, I have allocated 100gb storage and 8gb ram to ubuntu server due to splunk's high demand, while the static ip address of ubuntu server is 192.168.10.10/24 and the default gateway is 192.168.10.1/24.
<div align="center">
  <img src="https://github.com/user-attachments/assets/9434d6d6-aa39-4da1-a784-d026ba374212">
</div>
For allocating a static ip address to ubuntu server, we have to change the existing netplan.
<div align="center">
  <img src="https://github.com/user-attachments/assets/8d1a9a92-622a-4add-9331-6066cc438efd">
</div>
For installing splunk enterprise version on the ubuntu server, a shared folder feature of virtualbox is used. Before that few dependencies like Virtualbox-guest-additions-iso and Virtualbox-guest-Utils were installed. 
<div align="center">
  <img src="https://github.com/user-attachments/assets/d4c6f10e-543b-4f40-b5e6-02f02a282193">
</div>
Further, while the installing the splunk on the server, an administrator user: uper and password was created. This username and password can be used on a browser to connect to splunk server for monitoring the logs. 

# Windows Server

Windows server 2022 edition was used. For forwarding the logs to splunk server, a splunk universal forwarder was installed on the server and configured to transfer logs to splunk server's ip address which is 192.168.10.10/24 on default port for splunk 9997. Further, a custom inputs.conf was created. 

<div align="center">
  <img src="https://github.com/user-attachments/assets/cf2b3f3a-0bda-4195-bf8d-25f14bd9470e">
</div>
Further installed sysmon with sysmon olaf configuration file.
<div align="center">
  <img src="https://github.com/user-attachments/assets/d5efc126-3f86-448c-9222-b7dfa8fd013c"
</div>
  
While sysmon's services runs on local system account by defualt, for splunk universal forwarder log on as was reconfigured, further to implement the changes the service was restarted. 

<div align="center">
  <img src="https://github.com/user-attachments/assets/42945f56-f11a-47f1-8e41-0377f164d565">
</div>
The static ip for windows server
<div align="center">
  <img src="https://github.com/user-attachments/assets/ed583654-8317-4a10-ac34-54b1899f25fd">
</div>
  
Until now everything is same for installation part and network configuration part in windows target machine.
Installing Active Directory for this project is crucial because it enables centralized management of users, groups, and security policies, which is essential for simulating a realistic enterprise network environment. It also allows for the creation of organizational units and user accounts, providing a practical scenario for monitoring login activities and detecting potential security breaches. Additionally, AD integrates seamlessly with tools like Splunk for collecting logs related to authentication attempts, successful logins, and potential attack indicators, enhancing the overall log analysis and incident response workflow. 

<div align="center">
  <img src="https://github.com/user-attachments/assets/08b80a64-0990-43e8-b5b4-e783f8fd17b3">
</div>

Creating two organisation units "IT team" and "HR team", each with two users, "namanvora@splunk.local" and "jennyshah@splunk.local". 

# Windows Target machine

While the installation steps are same on both windows server and target machine, new indexes and receiving port on the splunk server has to be created. To access the splunk server in browser just enter the ip address of the server along with port 8000. Once the page is loaded, we need to enter the username and password which was set while installing the server

<div align="center">
  <img src="https://github.com/user-attachments/assets/f1571cfa-8f08-4d05-9073-31cd2bf72086">
</div>
Creating new index named "endpoints".
<div align="center">
  <img src="https://github.com/user-attachments/assets/91a30c96-a0f6-4f7f-a02d-90ba84f105fd">
</div>
<div align="center">
  <img src="https://github.com/user-attachments/assets/60d72299-cc2d-4cc8-a209-1d62351546d8"
</div>
The 2 hosts are target machine and active directory server.
Once, we can connect to the splunk server successfully, in order to get user login, the target machine has to be in direct connection with our active directory server. Further, reconfigured the previous dns 8.8.8.8 to 192.168.10.8/24 of windows active directory server.
<div align="center">
  <img src="https://github.com/user-attachments/assets/2cd147b1-8058-4f4b-939c-c692a2f3e63e">
</div>
  
Also, if a user wants to connect to our server using our domain we have to add our splunk.local domain to the target machine. 

<div align="center">
  <img src="https://github.com/user-attachments/assets/00580217-2432-4d66-ba27-2ab02232762d">
</div>
  
For our project, we have to enable remote access for bruteforce attack demonstration.

<div align="center">
  <img src="https://github.com/user-attachments/assets/10e0d4a3-d7f8-4f61-9847-87e72e7f6444">
</div>
<div align="center">
  <img src="https://github.com/user-attachments/assets/4a864847-e1d5-4156-ab90-36283bac8d1b">
</div>

# kali linux
Installing crowbar is very simple and straight forward. Configuring the network properties of the machine. 
<div alig="center">
  <img src="https://github.com/user-attachments/assets/77c93027-e7ce-4640-b805-b56bb2963765">
</div>
For this project, we are using a custom password list in crowbar for connecting to our target machine remotely and logging in successfully with the brute-force attack. For this attack, we are attacking "jennyshah"'s account and have already added a correct password in the password lists simulating successful attack. For connecting the target machine remotely, we are using remote desktop protocol a standard windows protocol for connecting machines remotely.
<div align="center">
  <img src="https://github.com/user-attachments/assets/eddd29c2-e876-4941-bd90-cf5ec356d4b7">
</div>

Once, the attack has been successfully executed, and as we know that the attack was done on "jennyshah"'s account. We can further write the name in the search bar and find it.
<div align="center">
  <img src="https://github.com/user-attachments/assets/3a2171ef-ce3e-45b9-9f7f-26fc66f45531">
</div>

We can further see some event codes and event types flagging up. Further expanding them by using them in our filter can help us to examine the attack's traverse. We can see there are particularly two event code flagging up which can help us to do our research on deciding the type of attack.

<div align="center">
  <img src="https://github.com/user-attachments/assets/1356297a-8082-4fbb-8314-5c2b03404487">
</div>

Some information regarding event code: 4625, simple google search can help for our research.

<div align="center">
  <img src="https://github.com/user-attachments/assets/57693258-fbe7-421a-9d90-a222eedadde1">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/0ab59c52-4914-4687-82f6-e9fbb1f6aaa0">
</div>

Failed login attempts indicating brute-force attack signs.

<div align="center">
  <img src="https://github.com/user-attachments/assets/51972b40-090a-416a-add6-173f42ab4633">
</div>
<div align="center">
  <img src="https://github.com/user-attachments/assets/2c599fbc-6597-4262-860a-18d7a72bf3c8">
</div>
A successful login attempt made by a machine namely: kali linux along with its ip address:192.168.10.250 
