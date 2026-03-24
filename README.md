<p align="center">
<img width="849" height="436" alt="image" src="https://github.com/user-attachments/assets/9b7bb3b5-b0d5-4419-94a5-4f4f5bd0b8f2" />
</p>

<h1>Designing Active Directory Infrastructure in Azure</h1>
This section of the lab involves building the foundational cloud infrastructure required for an Active Directory environment in Microsoft Azure. The process included creating a resource group, deploying a virtual network, provisioning virtual machines for the domain controller and client, and validating network connectivity to ensure the environment was ready for domain services deployment.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Azure Virtual Network
- Azure Resource Groups
- Remote Desktop Protocol (RDP)
- DNS

<h2>Operating Systems Used </h2>

- Windows Server 2022 (Standard_D2s_v3 - 2 vcpus, 8 GiB memory)
- Windows 11 pro (Version 25H2 - x64 Gen2)

<h2>Open Resource Groups in Azure</h2>

<p>
<img width="1461" height="896" alt="Lab1" src="https://github.com/user-attachments/assets/f6d5d6c0-a4d6-45ff-a25a-3a674006834e" />
</p>
<p>
 Step-by-step

1. Sign in to the **Azure portal**
2. Navigate to Resource groups from the Azure services menu
3. Select **Create** to begin building a new resource group for the lab environment

**Explanation:** 
 A resource group is used to organize related Azure resources in one location. Creating it first helps keep all lab components structured and easier to manage.

<h2>Create the Resource Group</h2>

<img width="1512" height="982" alt="Lab2" src="https://github.com/user-attachments/assets/55239797-f727-4e73-998f-55116d0ad19b" />
</p>
<p>
Step-by-step
  
1. Select the appropriate Azure subscription
2. Enter the resource group name **Active-Directory-Lab**
3. Chose the region Canada Central
4. Click **Review + Create** 
5. Continue to create the resource group

**Explanation:**
 This created a dedicated container for all resources used in the lab, including virtual machines and networking components.

<h2>Create the Virtual Network</h2>

<img width="1512" height="982" alt="Lab4" src="https://github.com/user-attachments/assets/a896cf84-a1c0-49f1-b743-ee076abb921c" />
</p>
<p>
Step-by-step

1. Open Virtual networks in Azure
2. Select **Create**
3. Assign the network to the Active-Directory-Lab resource group
4. Enter the virtual network name **Active-Directory-VNet**
5. Confirm that the region is the same as the resource group
6. Review the default IP address space and subnet settings
7. Click **Create**

**Explanation:**
 The virtual network allows the domain controller and client machine to communicate privately inside Azure. This is required for domain communication and DNS resolution.

<h2>Begin Creating the Domain Controller VM</h2>

<img width="1512" height="982" alt="Lab5" src="https://github.com/user-attachments/assets/9a1f8302-0947-4b6f-9eac-ef67c879d614" />
<p>
Step-by-step

1. **Open** Virtual machines in Azure
2. Click **Create virtual machine**
3. Select the **Active-Directory-Lab resource group**
4. Enter the **VM name dc-1**
5. Select the deployment region (Canada Central)
6. Review the basic configuration settings

**Explanation:**
 This virtual machine is created to serve as the main server for the lab. It would later be promoted to a Domain Controller.

<h2>Configure dc-1 Operating System and Credentials</h2>

<img width="1512" height="982" alt="Lab6" src="https://github.com/user-attachments/assets/54c07d12-6741-4d63-8e40-4887fb3ef0ce" />
</p>
<p>
Step-by-step

1. Select **Windows Server 2022 Datacenter Azure Edition** as the image
2. Confirm the VM architecture setting 
3. Select the **VM size** for the lab
4. Enter the **administrator username**
5. Enter and confirm the **administrator password**
6. Continue through the setup wizard

**Explanation:**
 This step ensures that the server is deployed with a supported Windows Server operating system capable of running Active Directory Domain Services.

<h2>Configure Networking for dc-1</h2>

<img width="1512" height="982" alt="Lab7" src="https://github.com/user-attachments/assets/101e4773-24e6-43f7-944d-b857841d7bfe" />
</p>
<p>
Step-by-step

1. Open the Networking tab during the dc-1 VM creation
2. Select **Active-Directory-VNet** as the virtual network
3. Keep the **default subnet**
4. Assign a public IP address for remote access
5. Set the network security group options
6. Allow **RDP (3389)** inbound access
7. Review the settings before deployment                                   

**Explanation:**
 This configuration integrates DC-1 into the lab network infrastructure and enables secure remote management using Remote Desktop Protocol (RDP).

<h2>Configure DC-1 Private IP Address</h2>

<img width="1512" height="982" alt="Lab12" src="https://github.com/user-attachments/assets/7d19e742-f66f-4321-bd6b-2bea5dc48323" />
</p>
<p>
Step-by-step

1. Navigate to the Network Interface settings for the DC-1 virtual machine
2. Selecte **IP configurations** to modify the network settings
3. Open the primary IP configuration **ipconfig1**
4. Change the Private IP allocation setting from **Dynamic to Static**
5. Verify the assigned private IP address **10.0.0.4**
6. Confirm the public IP address association for remote access
7. Than save the configuration changes

**Explanation**
 This step ensures that the DC-1 domain controller maintains a consistent private IP address, which is critical for reliable DNS services and proper communication within the Active Directory environment.

<h2>Begin Creating the Client VM</h2>

<img width="1512" height="982" alt="Lab8" src="https://github.com/user-attachments/assets/3445cc72-d890-4b3d-ba6b-84c7d24071d9" />
</p>
<p>
Step-by-step

1. Create another virtual machine process
2. Select the **Active-Directory-Lab resource group**
3. Enter the virtual machine name **Client-1**
4. Choose the same region as dc-1 **Canada Central**
5. Review the basic configuration settings

**Explanation:**
 This machine was created to simulate a workstation that would later connect to and join the domain.

<h2>Configure Client-1 Operating System and Credentials</h2>

<img width="1512" height="982" alt="Lab9" src="https://github.com/user-attachments/assets/8fda5b9d-1710-4884-9f1d-6f48963d5086" />
</p>
<p>
Step-by-step

1. Select **Windows 11 Pro** as the virtual machine image.
2. Confirm the VM architecture for compatibility.
3. Select the appropriate **VM size** for the lab environment.
4. Enter the **administrator username** for the client machine.
5. Enter and confirmed the **administrator password.**
6. Continue through the Azure VM creation wizard to proceed with deployment.

**Explanation:**
 This step ensured that the client computer was deployed with a supported Windows operating system that will later be connected to the DC-1 domain controller as part of the Active Directory environment.

<h2>Configure Networking for Client-1</h2>

<img width="1512" height="982" alt="Lab10" src="https://github.com/user-attachments/assets/d4ea39a7-1c08-45fe-bf5a-1b9e13b2503f" />
</p>
<p>
Step-by-step

1. Open the Networking tab during the Client-1 setup
2. Select **Active-Directory-VNet**
3. Keep the **default subnet**
4. Assign a public IP address
5. Configure the network security settings
6. Allow **RDP (3389)** inbound access
7. Continue with the deployment

**Explanation:**
 This Comfirms that Client-1 was on the same virtual network as dc-1 so both machines could communicate internally.

<h2>Verify Both Virtual Machines Are Running</h2>

<img width="1512" height="982" alt="Lab13" src="https://github.com/user-attachments/assets/0506e0a0-f236-40a7-b858-6e5191fedd34" />
</p>
<p>
Step-by-step

1. Return to the Virtual machines page
2. Verify that dc-1 and Client-1 appeared in the list
3. **Confirm** both virtual machines shows a Running status

**Explanation:**
 This confirms that both systems were deployed successfully and were ready for configuration and testing.

<h2>Connect to dc-1 with Remote Desktop</h2>

<img width="1512" height="982" alt="Lab14" src="https://github.com/user-attachments/assets/67686dfb-668a-4d11-b359-9a2881ba7472" />
</p>
<p>
Step-by-step

1. Open the **dc-1 virtual machine details in Azure**
2. Locate the public IP address for dc-1   
3. Open **Remote Desktop** Connection on the local computer
4. Enter the **public IP address**
5. Enter the **administrator credentials**
6. Now start the remote session

**Explanation:**
 Remote Desktop provides administrative access to the server so configuration tasks can be completed directly inside the VM.

<h2>Verify Server Manager on dc-1</h2>

<img width="1512" height="982" alt="Lab16" src="https://github.com/user-attachments/assets/1c6b44d7-a7f5-42e2-af14-626bd5b40532" />
</p>
<p>
Step-by-step
 
1. Connect to dc-1 using Remote Desktop Protocol.
2. Allow the Windows Server desktop to fully load.
3. **Confirm** that Server Manager opened successfully, indicating the server was ready for configuration.

**Explanation:**
 Server Manager is the main administrative tool used to install server roles and manage Windows Server services.

<h2>Adjust Windows Firewall Settings for Lab Testing</h2>

<img width="1512" height="941" alt="Lab19" src="https://github.com/user-attachments/assets/87a8f4ac-8441-4e6f-8ee4-7428634825cd" />
</p>
<p>
Step-by-step

1. Open Windows Defender Firewall with Advanced Security on dc-1.
2. Review the firewall profile settings.
3. Adjust the firewall configuration for the lab environment.
4. **Confirm** the changes were applied.

**Explanation:**
 In a lab environment, firewall settings are sometimes adjusted to make connectivity testing easier between systems.This helps avoid blocked communication during setup and troubleshooting.

<h2>Begin Connectivity Testing from Client-1</h2>

<img width="1512" height="982" alt="Lab23" src="https://github.com/user-attachments/assets/b606bba2-f30e-4fc5-a8ec-01499e9e183d" />
</p>
<p>
Step-by-step

1. Log into **Client-1**
2. Open Windows PowerShell
3. Enter the following command: **ping 10.0.0.4**
4. Run the command to test connectivity to the domain controller
 
**Explanation:**
 This test verifies network connectivity between Client-1 and the domain controller (DC-1). Successful ping responses confirm that the client machine can reach the server across the private network.

<h2>Verify Network Connectivity and DNS Settings</h2>

<img width="1512" height="982" alt="Lab24" src="https://github.com/user-attachments/assets/078966a3-eeb8-4d37-bd2e-d3f1c57f41ae" />
</p>
<p>
Step-by-step

1. Confirm that the ping to **10.0.0.4** was successful
2. Enter the following command: **ipconfig /all**
3. Review the network adapter details
4. Verify the client IP configuration
5. Confirm the DNS server setting points to **10.0.0.4**

**Explanation:**
 This final verification confirmed that Client-1 could communicate with dc-1 and that the DNS settings were correctly configured to point to the domain controller. That completes the infrastructure setup required before deploying Active Directory.
 
 <h2>Summary</h2>
At the conclusion of this phase, the Azure environment was fully prepared for the Active Directory deployment. The resource group and virtual network were created, the domain controller and client virtual machines were deployed, remote access was configured, and connectivity between systems was successfully verified. This completed the foundational infrastructure required for the next phase of the lab.
