<p align="center">
  
![Image](https://github.com/user-attachments/assets/ad8da1a3-4fff-4af2-bd17-cdd14d39da67)


<h1> Setting up Active Directory infrastructure in Azure </h1>
This tutorial outlines preparing the Azure infrastructure before deploying our Active Directory Environment. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop/Bastion
- PowerShell (to test DNS server) 

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022 

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Resource Group 
- Create 2 VMs within the Resource Group (Domain Controller and client) 
- Set Domain Controller's NIC Private IP to be static
- Change the client's DNS settings so it goes through our Domain Controller. 

<h2>Setup and Configuration</h2>

First, we must create a Resource Group; within this Group, we will create 2 Virtual Machines. If you don't know how to do this you can refer to our previous guide [here](https://github.com/JosueVazquezTech/Azure-VM-setup-?tab=readme-ov-file). The first virtual machine will be a Windows 10 machine like the one in the guide and we will name it "client". The second virtual machine will be different, here we will choose Windows Server 2022 as our OS instead of Windows 10. This machine will be our domain controller and we will name it "dc1." 


![Image](https://github.com/user-attachments/assets/ac313bef-fcf0-4d0f-ba56-eae734aed644)

After you are done creating both virtual machines now we have to change the IP address on our domain controller to be static ( by default Azure will assign you a dynamic IP address) so we can use it as our DNS server. You can do this by navigating to the virtual machines and clicking on "dc1", then go to network settings and click on "network interface/IP configuration."

![Image](https://github.com/user-attachments/assets/18f4a7d0-84d1-439b-aaeb-f1ed55d08716)

![Image](https://github.com/user-attachments/assets/21afa43f-08a7-42c7-b873-6d42c4687fb7)


On the left side you see the settings tab, open it and go to IP configurations. Here you can see at the bottom that your IP is currently Dynamic. Just click on the IP address name and a window will open where you can change the allocation to Static and save it. 

Now we have to log into our domain controller and disable the firewall so we can test the connection later. To do this just go to your Virtual Machines, click dc1, and under "connect" you can click "Download RDP file." This is the remote desktop protocol file used to connect remotely to other systems, most computers come with software capable of opening this kind of file. Go to your downloads folder and open the RDP file, it will prompt you to use the username and password you created earlier for the domain control account. Fill in the information and click ok. After you connect to the VM you should see the Windows Server Manager Dashboard, if you don't see this Dashboard and you can't find it on the search bar it probably means you installed the wrong OS and you have to delete the VM and make a new one with Windows Server. If you see the Dashboard go to the next step. 

![Image](https://github.com/user-attachments/assets/51cde626-8698-4126-99d5-d1c14600271d)

![Image](https://github.com/user-attachments/assets/18794700-c12e-4847-a2be-c7b7cb5064e1)

![Image](https://github.com/user-attachments/assets/850848fe-2a30-40ee-8987-81ab994ad8a8)


Now we will turn off the Dc1 Firewall to test the connection. You can type Windows key+R or type run in the search bar and you should get the Run pop-up window. Here you will type **wf.msc** and this should open the Firewall window for you.
Click on Windows Defender Firewall Properties and set the Firewall State drop-down to "off". Repeat this process in the Private Profile and Public Profile tabs. After doing this click Apply and Ok then you can logout of Dc1. 

![Image](https://github.com/user-attachments/assets/c041619f-b9a6-40a0-90f3-771e12dfe6a1)

![Image](https://github.com/user-attachments/assets/292b8339-2486-467d-8cc9-d8c6ff54280c)

![Image](https://github.com/user-attachments/assets/932ed5f3-4414-40c5-9a2b-337ef2060e65)

The next step is to change the Client's DNS settings to Dc1's private IP address. This will make it so that anytime the Client's VM needs to look up any domain it will look to our Domain Controller. To do this first, go back to your virtual machines in Azure and go to Dc1. Then go to Network>Network Settings and you will be able to see  Dc1's private IP address and copy it. Now go to the Client VM and again go to >Network>Network Settings and click Network Interface/IP Configuration. Here you can find the DNS server under settings and paste the Dc1 IP address in the DNS server box. For me, this is **10.0.0.5**. After doing this you have to restart the Client VM so the changes can take place. Go to virtual machines, right-click the Client VM, and click restart. Alternatively, you can check the box next to the VM and click Restart at the top. After doing this you can finally log into Client's VM to test the connection. Do this by going to Virtual Machines>Client>Connect and downloading the RDP file so you can open it and connect remotely. 


![Image](https://github.com/user-attachments/assets/06e102dd-c0f3-4dc5-a133-ef63476ae1d9)

![Image](https://github.com/user-attachments/assets/c7b56d89-e1ea-4b92-824e-d0044f9442d3)

![Image](https://github.com/user-attachments/assets/4345717f-af75-406e-97c9-6b91069caf11)

![Image](https://github.com/user-attachments/assets/ec589820-97e0-43c4-8063-266fa0137747)

The last step of the setup is to test the connection between our Client VM and our Domain Controller and make sure our DNS changes are working. To do this open Windows PowerShell (you can find it in the search bar). Here you will type the command **ping** followed by a space and your Dc1's Private IP and press enter. For me this will look like this: **ping 10.0.0.5**. You should see multiple replies from the Domain Controller's IP. If you get an error here it could be because of multiple reasons like the firewall not being turned off or not changing the Client DNS settings or not making the Dc1 IP static. You can further test the connection by typing the command **ipconfig /all** to see if the DNS server is pointing to the correct IP which in my case is **10.0.0.5**. 

































































