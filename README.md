<p align="center">
  
![Image](https://github.com/user-attachments/assets/ad8da1a3-4fff-4af2-bd17-cdd14d39da67)


<h1> Setting up Active Directory infrastructure using Azure. </h1>
This tutorial outlines preparing our infrastructure before deploying our Active Directory Environment. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop/Bastion
- PowerShell (to test connection) 

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- 

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Resource Group
- Create a VM within the Resource Group 
- Enable Remote Desktop Protocol on our VM
- Connect to our VM using RDP or Bastion

<h2>Setup and Configuration</h2>

<p> Before you get started you have to go to https://azure.microsoft.com/ and create a free account (note that you do need a credit or debit card to complete this process)
After creating your account now you can go to Resource Groups (you can use the search bar at the top or find it under Azure Services)
then click "Create"


![001](https://github.com/user-attachments/assets/6dda2e94-82ff-4db6-9660-a0d75206cbe3)





![002](https://github.com/user-attachments/assets/9bd5e2e1-deaa-4de8-b5cc-8ca7ac33bd03)


<p>Under subscriptions the default option should be "Azure subscription 1," if it's something different make sure to change it. This will make sure you don't get billed while using the free trial. 

<p>Then we have to name our Resource Group, I called mine "OsTicket1" but you can use any name of your choosing. Then we go to Region and select the closest one to your location. Then click "create." Later when we choose the region for our VM we might find that no VM can be created on the region we selected for our Resource Group, if this happens I recommend you go back to your Resource Group and change the region to one that matches the VM. 




![003](https://github.com/user-attachments/assets/218abedc-eb1e-4046-8ede-248c2b3bc800) 





![004](https://github.com/user-attachments/assets/6ccabd16-6c79-420d-b3c3-a8e6d6348756)

Now we go to "virtual machines". You can use the search bar like I did here or you can find it under "Azure Services". Then we click "Create." 

![005](https://github.com/user-attachments/assets/fd42beb4-34c7-44be-998d-2986bdca93df)



![006](https://github.com/user-attachments/assets/1260ce6d-17ef-4761-835a-5a433920003d)

<p>The default subscription should be "Azure subscription 1" so just leave that as it is. For Resource Group we are gonna select the one we already created in the last step. In my case, I selected "OsTicket1."
<p>Now we need to name our VM, I named mine "HelpDesk."
Next is the region, here you have to make sure you select the same region we selected for our Resource Group. For Availability options just leave the default option as Availability zone and make sure "self-selected zone" is enabled.  </p>

![007](https://github.com/user-attachments/assets/d654ba9a-2613-4f51-a4bc-5a31220f3d41)

When selecting the size for our VM make sure you pick one that has at least 2vcpus and 8GB of memory (think of this as the CPUs and RAM in your computer). I picked the Standard_D4s_v3 with 4vcpus and 16GB memory. Note that the monthly price for our VM is only if we were to leave it running 24/7 which we wont need to do since we can turn them off after we are done. The free account we have comes with 200$ credit so this is nothing to worry about. 

![008](https://github.com/user-attachments/assets/ae1b271f-7500-42f5-90f6-196fb2485d41)

<p>Now we are going to create the admin user were are gonna use to log into our VM. Just like you would do on a new computer select a new username and password. For username I choose labuser. 
</p>
<p>Under select inbound ports you need to select RDP (3389) this is really important since without this we can't connect to our VM. RDP stands for Remote Desktop Protocol, it operates over TCP and UDP port 3389 which allows us to remotely connect to our VM. Usually, you wouldn't allow this port to be exposed to the internet without other security measures but since this is just a lab it's ok. </p>

![009](https://github.com/user-attachments/assets/e0892674-9758-4979-bfd9-961173bc30cc)

At the bottom of the screen click "next" until you land on the "network" page. Here you just have to make sure our network was automatically created. If you did everything correct until this point you should have a new Virtual Network, a new Subnet, and a new public IP address. After that click Review + Create. 

![010](https://github.com/user-attachments/assets/eba1d847-5a20-4659-a922-c6539d1e220d)

Congratulations! You have created your first Virtual Machine on Azure! 
