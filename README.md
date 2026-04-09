<h1>Building and Managing an Active Directory Environment in Microsoft Azure</h1>

<p align="center">
<img width="600" height="335" alt="image" src="https://github.com/user-attachments/assets/f5a3d652-196e-4dcf-814f-113a9cbb6f54" />

<hr>

<h2>Overview</h2>
This project demonstrates the full lifecycle of building, deploying, automating, and managing an Active Directory environment in Microsoft Azure. The implementation follows a structured approach that begins with infrastructure setup, continues through domain deployment, expands into user account automation with PowerShell, and concludes with security enforcement through Group Policy and account management.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Azure Virtual Network (VNet)
- Azure Resource Groups
- Remote Desktop Protocol (RDP)
- Domain Name System (DNS)
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Group Policy Management Console (GPMC)
- Windows PowerShell
- PowerShell ISE

<h2>Operating Systems Used</h2>

- Windows Server 2022 (Datacenter Azure Edition Hotpatch-x64 Gen2)
- Windows 11 Pro (Version 25H2-x64)

<h2>Phase 1: Azure Infrastructure Setup</h2>
In the first phase of the project, the Azure environment is prepared to support the Active Directory deployment. A dedicated resource group and virtual network are created, two virtual machines are deployed, and connectivity is verified to ensure the environment is ready for domain services.<br>
 
<h3>Open Resource Groups in Azure</h3>
<img width="1461" height="896" alt="Lab1" src="https://github.com/user-attachments/assets/c258b933-c00c-4449-a1e8-eecc90e2ed89" />

**Steps:**
- Open the Azure portal.
- Locate **Resource groups** from the Azure services menu.
- Click **Create** to begin building a new resource group for the lab environment.

**Explanation:** A resource group is used to organize related Azure resources in one location. Creating it first keeps all lab components structured and easier to manage.

<h3>Create the Resource Group</h3>
<img width="1512" height="982" alt="Lab2" src="https://github.com/user-attachments/assets/ade46f5a-d784-4bcc-8056-739cbdc273e6" />

**Steps:**
- Select the appropriate Azure subscription.
- Enter the resource group name **Active-Directory-Lab.**
- Choose the region **Canada Central.**
- Review the settings.
- Continue to create the resource group.

**Explanation:** This creates a dedicated container for all resources used in the lab, including virtual machines and networking components.

<h3>Create the Virtual Network</h3>
<img width="1512" height="982" alt="Lab4" src="https://github.com/user-attachments/assets/bfc7ef1a-ff64-4bcf-bcf4-79e8f47abf91" />

**Steps:**
- Open **Virtual networks** in Azure.
- Select **Create.**
- Assign the network to the **Active-Directory-Lab** resource group.
- Enter the virtual network name **Active-Directory-VNet.**
- Verify the region matches the resource group.
- Review the default IP address space and subnet settings.
- Click **Create.**

**Explanation:** The virtual network allows the domain controller and client machine to communicate privately inside Azure. This is required for domain communication and DNS resolution.

<h3>Configure dc-1 Operating System and Credentials</h3>
<img width="1512" height="982" alt="Lab6" src="https://github.com/user-attachments/assets/91454e26-9354-4a28-9929-4393ab68fe12" />

**Steps:**
- Begin creating the **dc-1** virtual machine.
- Select **Windows Server 2022 Datacenter Azure Edition** as the image.
- Confirm the VM architecture is set to **x64.**
- Select the VM size for the lab.
- Enter the administrator username.
- Enter and confirm the administrator password.
- Continue through the setup wizard.

**Explanation:** This step confirms that the server is deployed with a supported Windows Server operating system capable of running Active Directory Domain Services.

<h3>Configure Networking for dc-1</h3>
<img width="1512" height="982" alt="Lab7" src="https://github.com/user-attachments/assets/e719186f-1a6e-4a59-9a3d-c0ef8441b4ef" />

**Steps:**
- Open the **Networking** tab during the **dc-1** VM creation.
- Select **Active-Directory-VNet** as the virtual network.
- Choose the default subnet.
- Assign a public IP address for remote access.
- Set the network security group options.
- Allow **RDP (3389)** inbound access.
- Review the settings before deployment.

**Explanation:** This ensures that **dc-1** is connected to the lab environment and can be remotely accessed and managed through Remote Desktop.

<h3>Configure DC-1 Private IP Address</h3>
<img width="1512" height="982" alt="Lab12" src="https://github.com/user-attachments/assets/57d92583-b2ed-4e55-bf31-d697e40a3946" />

**Steps:**
- Locate the Network Interface settings for the **dc-1** virtual machine.
- Select **IP configurations** to modify the network settings.
- Open the primary IP configuration **(ipconfig1).**
- Change the **Private IP allocation** setting from **Dynamic** to **Static.**
- Confirm the assigned private IP address **(10.0.0.4).**
- Verify the public IP address for remote access.
- Save the configuration changes.

**Explanation:** This step ensures that the domain controller maintains a consistent private IP address, which is critical for reliable DNS services and proper communication within the Active Directory environment.

<h3>Begin Creating the Client VM</h3>
<img width="1512" height="982" alt="Lab8" src="https://github.com/user-attachments/assets/39a82e3d-fc7b-4c2d-9724-9cf36df917ed" />

**Steps:**
- Open another virtual machine.
- Select the **Active-Directory-Lab** resource group.
- Enter the virtual machine name **Client-1.**
- Select the same region as **dc-1.**
- Review the basic configuration settings.

**Explanation:** This machine is created to simulate a workstation that will later connect to and join the domain.

<h3>Configure Networking for Client-1</h3>
<img width="1512" height="982" alt="Lab10" src="https://github.com/user-attachments/assets/8045850c-872e-4674-b0a0-3339562c3f9d" />

**Steps:**
- Open the **Networking** tab during the **Client-1** setup.
- Choose **Active-Directory-VNet.**
- Keep the default subnet.
- Assign a public IP address.
- Configure the network security settings.
- Allow **RDP (3389)** inbound access.
- Continue with the deployment.

**Explanation:** This confirms that **Client-1** is placed on the same virtual network as **dc-1** so both machines can communicate internally.

<h3>Begin Connectivity Testing from Client-1</h3>
<img width="1512" height="982" alt="Lab23" src="https://github.com/user-attachments/assets/da3ce6da-48fc-4b37-998e-5b06f2ca32f3" />

**Steps:**
- Log in to **Client-1.**
- Open **Windows PowerShell.**
- Type the following command: **ping 10.0.0.4**
- Run the command to test connectivity to the domain controller.

**Explanation:** This test verifies network connectivity between **Client-1** and the domain controller **(dc-1).** Successful ping responses confirm that the client machine can reach the server across the private network.

<h3>Verify Network Connectivity and DNS Settings</h3> 
<img width="1512" height="982" alt="Lab24" src="https://github.com/user-attachments/assets/89b9a087-c261-45d1-aa44-a5266df69ddc" />

**Steps:**
- Confirm that the ping to **10.0.0.4** is successful.
- Type the following command: **ipconfig /all**
- Review the network adapter details.
- Verify the client IP configuration.
- Confirm the DNS server setting points to **10.0.0.4.**

**Explanation:** This final verification confirms that **Client-1** can communicate with **dc-1** and that the DNS settings are correctly configured to point to the domain controller. That completes the infrastructure setup required before deploying Active Directory.

<h2>Phase 2: Active Directory Domain Services Deployment</h2>
With the Azure infrastructure in place, the next phase focuses on installing and configuring Active Directory Domain Services, promoting the server to a Domain Controller, creating the domain environment, and joining the client machine to the domain.<br>

<h3>Server Manager Dashboard</h3>
<img width="1512" height="982" alt="LabA1" src="https://github.com/user-attachments/assets/716986ee-7786-455d-8cee-8ab3a56e86fd" />

**Steps:**
- Open the Windows Server virtual machine **dc-1.**
- Sign in to the server using administrative credentials.
- Wait for the desktop environment to finish loading.
- Open **Server Manager.**
- Review the dashboard to confirm the server is available for configuration.
- Prepare to begin installing the Active Directory Domain Services role.

**Explanation:** This shows the starting point of the server configuration process. Server Manager is the primary tool used to install roles and features on Windows Server, including Active Directory Domain Services.

<h3>Select Active Directory Domain Services</h3>
<img width="1512" height="982" alt="LabA4" src="https://github.com/user-attachments/assets/4ceccc21-77d9-4a28-a643-2cc3a2137201" />

**Steps:**
- Choose **Add Roles and Features.**
- Proceed through the wizard until reaching the **Server Roles** page.
- Locate **Active Directory Domain Services** in the list of available roles.
- Check the box for **Active Directory Domain Services.**
- Review the description shown on the right side of the wizard.
- Prepare to continue with the required supporting features.

**Explanation:** This step selects the core server role required to build an Active Directory environment. Choosing Active Directory Domain Services allows the server to later be promoted into a Domain Controller.

<h3>Confirm Installation Selections</h3>
<img width="1512" height="982" alt="LabA10" src="https://github.com/user-attachments/assets/71388aa5-de12-4cee-b22a-5b8e0d0af9e3" />

**Steps:**
- Accept the additional required features when prompted.
- Continue through the wizard until reaching **Confirm installation selections.**
- Review the selected roles and features, including: 
- **Active Directory Domain Services**
- **Group Policy Management**
- **Remote Server Administration Tools**
- Check **Restart the destination server automatically if required.**
- Click **Install.**

**Explanation:** This step confirms the final set of components required for Active Directory Domain Services. Installing these features is a necessary step toward configuring the Windows Server as an operational Domain Controller.

<h3>Promote This Server to a Domain Controller</h3>
<img width="1512" height="982" alt="LabA14" src="https://github.com/user-attachments/assets/e15aadec-806f-4d52-a314-69a15ae84a07" />

**Steps:**
- Return to the **Server Manager** dashboard after the AD DS role finishes installing.
- Observe the notification flag in the upper-right corner.
- Click the notification flag.
- Review the post-deployment message indicating that configuration is required for Active Directory Domain Services on **dc-1.**
- Locate the option: **Promote this server to a domain controller**
- Select the promotion link to begin the Active Directory Domain Services Configuration Wizard.

**Explanation:** Promoting the server to a Domain Controller configures it to run Active Directory and manage the domain. This allows the server to control user accounts, computers, and security policies across the network.

<h3>Configure a New Forest</h3>
<img width="1512" height="982" alt="LabA15" src="https://github.com/user-attachments/assets/fa15f91b-3f2f-4a02-a4e6-2a071c61a7a9" />

**Steps:**
- Open the Active Directory Domain Services Configuration Wizard from Server Manager.
- Review the available deployment options.
- Select **Add a new forest.**
- Click into the **Root domain name** field.
- Type the domain name: **mydomain.com**
- Ensure that the new forest option is selected.
- Click **Next** to continue.

**Explanation:** During this step, a new Active Directory forest is created and the root domain is defined. The forest serves as the top-level container of the directory structure, while the root domain acts as the primary administrative control point for the network.

<h3>Configure Domain Controller Options</h3>
<img width="1512" height="982" alt="LabA16" src="https://github.com/user-attachments/assets/b70b6e85-e696-4cb5-b6bb-cdec0a489706" />

**Steps:**
- Proceed to the **Domain Controller Options** page.
- Review the default **Forest functional level.**
- Review the default **Domain functional level.**
- Verify that the **DNS server** option is selected.
- Verify that the **Global Catalog (GC)** option is enabled.
- Leave **Read Only Domain Controller (RODC)** unselected.
- Enter a **Directory Services Restore Mode (DSRM)** password.
- Confirm the DSRM password in the second field.
- Select **Next** to continue.

**Explanation:** This step defines important domain controller settings. It enables DNS integration, confirms the server’s role in the domain, and sets the DSRM password used for recovery and maintenance of Active Directory services.

<h3>Prerequisites Check</h3>
<img width="1512" height="982" alt="LabA19" src="https://github.com/user-attachments/assets/cc5f3349-c404-4b78-bcb3-aadba1a3b5e4" />

**Steps:**
- Proceed through the configuration wizard after reviewing the deployment options.
- Reach the **Prerequisites Check** page.
- Wait while Windows validates the configuration.
- Review the results shown in the wizard.
- Verify that the message states: **All prerequisite checks passed successfully**
- Review any warning messages listed in the results panel.
- Confirm that no blocking errors prevent the promotion process.
- Click **Install.**

**Explanation:** This step checks the server’s readiness to become a Domain Controller. The wizard validates required settings, confirms prerequisites are met, and alerts you to any warnings or errors before the promotion process starts.

<h3>Domain Controller Promotion Complete</h3>
<img width="1512" height="982" alt="LabA21" src="https://github.com/user-attachments/assets/6f0f84ce-a880-4d00-b532-b0a63741a0c2" />

**Steps:**
- Wait while Active Directory is configured.
- Review the **Results** page after the promotion process completes.
- Confirm the message showing: **This server was successfully configured as a domain controller**
- Observe the notification indicating that the computer is about to be signed out.
- Read the restart message explaining that: **The server is going to be restarted because Active Directory Domain Services has been installed or removed.**
- Allow the restart process to continue.

**Explanation:** This step indicates that the Domain Controller promotion process has finished successfully. Restarting the server finalizes the configuration and allows Active Directory Domain Services to begin operating.

<h3>Verify Domain Presence in ADUC</h3>
<img width="1512" height="982" alt="LabA26" src="https://github.com/user-attachments/assets/33027344-0e53-47ab-8f6f-aa5c8cc9d3a6" />

**Steps:**
- Click the **Start Menu.**
- Open **Windows Administrative Tools.**
- Select **Active Directory Users and Computers.**
- Verify that **mydomain.com** appears under Active Directory Users and Computers.
- Confirm that the domain is listed and accessible in the console.

**Explanation:** This step confirms that the Active Directory domain is successfully created and visible within the Active Directory Users and Computers console. Seeing the domain listed indicates that the Domain Controller is properly managing the directory environment and is ready for administrative tasks.

<h3>Create Organizational Unit</h3>
<img width="1512" height="982" alt="LabA27" src="https://github.com/user-attachments/assets/99ab9f96-d401-4e76-a012-36d3633779c7" />

**Steps:**
- Open **Active Directory Users and Computers.**
- Select **mydomain.com.**
- Right-click the domain name.
- Choose **New.**
- Select **Organizational Unit.**
- Click into the **Name** field.
- Type the name: **_EMPLOYEES**
- Leave **Protect container from accidental deletion** checked.
- Click **OK** to create the Organizational Unit.

**Explanation:** An Organizational Unit is created to organize and manage user accounts within the domain. OUs allow administrators to structure resources logically and make it easier to delegate administrative tasks and apply group policies.

<h3>Join Client-1 to the Domain</h3>
<img width="1512" height="982" alt="LabA31" src="https://github.com/user-attachments/assets/cdb43d0e-a449-43ec-b2b5-73e868af82bd" />

**Steps:**
- Log in to the **Client-1** virtual machine.
- Open the **Start Menu** and select **System.**
- Click **Rename this PC (advanced).**
- Under the **Computer Name** tab, click **Change.**
- Observe the **Computer Name/Domain Changes window.**
- Select the Domain option under **Member of.**
- Click into the domain field.
- Type: **mydomain.com**
- Click **OK.**

**Explanation:** By joining the workstation to the domain, it becomes part of the Active Directory environment. This integration allows administrators to manage the computer, apply group policies, and provide users with domain-based sign-in access.

<h2>Phase 3: Automating User Creation with PowerShell</h2>
After establishing the domain environment, the next phase focuses on automating user account creation using PowerShell. This demonstrates how repetitive administrative tasks can be completed more efficiently and consistently through scripting.<br>

<h3>Open and Review the PowerShell Script</h3>
<img width="1512" height="982" alt="LabB5" src="https://github.com/user-attachments/assets/85faaac7-8051-49f3-bc56-32166523fdfc" />

**Steps:**
- Open **Windows PowerShell ISE** with administrative privileges.
- Import or open the user creation script within the script editor.
- Examine the script variables and configuration details carefully.
- Verify the password that will be applied to the generated user accounts.
- Check the total number of users the script is configured to create.
- Review the overall script layout to ensure everything is correct before running it.

**Explanation:** This step represents the review stage of the automation process. Examining the script before execution helps confirm that the configuration is correct and that the user creation process will run as intended without errors.

<h3>Review New-ADUser Automation Logic</h3>
<img width="1512" height="982" alt="LabB7" src="https://github.com/user-attachments/assets/f5326449-81c6-4521-a886-56611ba4677b" />

**Steps:**
- Examine the portion of the script that handles the creation of user accounts.
- Ensure that a loop is implemented to generate multiple users automatically.
- Check that first and last names are being generated programmatically.
- Verify that these names are combined properly to form a username.
- Review the method used to convert the password into a secure string format.
- Confirm that the **New-ADUser** cmdlet is used to create each user account.
- Ensure the **Path** parameter is set to the **_EMPLOYEES** Organizational Unit.
- Verify that all newly created accounts are set to active **(enabled).**

**Explanation:** This step demonstrates the practical use of scripting for scalability. Automating user creation allows administrators to quickly deploy large numbers of accounts while maintaining standardized configurations and minimizing human error.

<h3>Execute the Script and Create Users</h3>
<img width="1512" height="982" alt="LabB8" src="https://github.com/user-attachments/assets/1aef3f6e-c591-4444-a647-3529d7d8f774" />

**Steps:**
- Select **Run Script** within Windows PowerShell ISE to execute the script.
- Observe the console output displayed in the lower pane.
- Check for messages such as **Creating user** to confirm activity.
- Ensure that usernames are being generated one after another without interruption.
- Allow the script to run until the entire user creation process is complete.

**Explanation:** This step validates that the automation is functioning correctly. Monitoring the console output shows that each user account is being generated as intended, confirming accurate script execution.

<h3>Verify Accounts in ADUC</h3>
<img width="1512" height="982" alt="LabB11" src="https://github.com/user-attachments/assets/8a0eef91-f25d-4f06-a9e6-0e753ed74bc7" />

**Steps:**
- Open **Active Directory Users and Computers** from the administrative tools.
- Expand the domain in the left-hand pane to view available Organizational Units.
- Select the **_EMPLOYEES** Organizational Unit.
- Review the list of users shown in the main pane.
- Choose a user account that will be used for sign-in testing.

**Explanation:** This is the proof stage of the automation workflow. It confirms that the accounts are successfully written into Active Directory and stored in the correct Organizational Unit.

<h3>Confirm Successful Login</h3>
<img width="1512" height="982" alt="LabB16" src="https://github.com/user-attachments/assets/4ddd7768-3f56-446b-897c-226a39f49c1c" />

**Steps:**
- Log in to the client computer or connect through Remote Desktop.
- Enter the username of one of the generated accounts.
- Type the password specified in the script.
- Start the sign-in process using the domain user credentials.
- Wait for the sign-in process to finish completely.
- Confirm that the user profile begins initializing and that the account gains access to the system.

**Explanation:** This step serves as the final validation of the automation process. Successful sign-in confirms that the generated accounts are functional, properly authenticated through Active Directory, and able to access system resources as expected.

<h2>Phase 4: Group Policy Configuration and Account Management</h2>
With user accounts successfully created, the final phase focuses on enforcing security policies and performing administrative account management tasks. This phase demonstrates how Group Policy and Active Directory tools are used to manage authentication behavior and user access.<br>

<h3>Launch Group Policy Management Console</h3>
<img width="1512" height="982" alt="LabC5" src="https://github.com/user-attachments/assets/c276e911-7f1c-4d45-8381-34ffeb7dd8f5" />

**Steps:**
- Wait until the **Group Policy Management Console** finishes opening.
- Confirm that the console shows **Forest: mydomain.com.**

**Explanation:** This confirms that the Group Policy tools are available and properly connected to the Active Directory environment. It also shows that the domain is ready for policy administration.

<h3>Configure Account Lockout Policy</h3>
<img width="1512" height="982" alt="LabC14" src="https://github.com/user-attachments/assets/36f20eb0-ecda-453c-8d8a-6e87b33172ed" />

**Steps:**
- Open the **Default Domain Policy** in the Group Policy Management Editor.
- Navigate to:<br>
**↳ Computer Configuration**<br>
**↳ Policies**<br> 
**↳ Windows Settings**<br> 
**↳ Security Settings**<br> 
**↳ Account Policies**<br> 
**↳ Account Lockout Policy**
- Set the following options: 
- **Account lockout duration: 30 minutes** 
- **Account lockout threshold: 5 invalid logon attempts** 
- **Allow Administrator account lockout: Enabled** 
- **Reset account lockout counter after: 10 minutes**

**Explanation:** This step configures the account lockout settings to strengthen security for domain accounts. It helps prevent unauthorized access by locking accounts after multiple failed sign-in attempts.

<h3>Verify Account Lockout from Client Side</h3>
<img width="1512" height="982" alt="LabC17" src="https://github.com/user-attachments/assets/caf0a0cf-8df7-43ed-b581-e66a5f2d25f5" />

**Steps:**
- On the client machine, attempt to sign in multiple times using incorrect credentials for the target user.
- Repeat the attempts until the account reaches the lockout threshold.
- Observe and note the error message displayed by Remote Desktop.

**Explanation:** This step verifies proper enforcement of the account lockout settings. The error message shows that the account is temporarily restricted after repeated invalid sign-in attempts.

<h3>Search for User Account</h3>
<img width="1512" height="982" alt="LabC24" src="https://github.com/user-attachments/assets/4d7ce024-d230-4233-bc0f-012abdb6e6f6" />

**Steps:**
- Open **Active Directory Users and Computers.**
- Use the **Find Users, Contacts, and Groups tool.**
- Search for **bab.pok** in the **Name** box.
- Verify that the user is listed in the search results.

**Explanation:** This step is used to locate the specific user account before performing administrative tasks. It reflects a standard help desk procedure for quickly accessing user information in Active Directory.

<h3>Disable User Account</h3>
<img width="1512" height="982" alt="LabC30" src="https://github.com/user-attachments/assets/75babc6b-6e26-4ac3-8419-ac5f16083169" />

**Steps:**
- Right-click the account to open the context menu.
- Click **Disable Account.**
- Acknowledge any confirmation prompt if it appears.

**Explanation:** Disabling a user account is a standard administrative action that immediately blocks access to the domain. This is commonly performed when an account should no longer be used temporarily, such as during security incidents, offboarding, or account troubleshooting, while still preserving the account and its associated data.

<h3>Refresh Group Policy on Client</h3>
<img width="1512" height="982" alt="LabC32" src="https://github.com/user-attachments/assets/908d6910-84db-45f2-ba75-5edc3bfde3bd" />

**Steps:**
- Sign in to the client PC with an administrative account.
- Open the **Command Prompt** application.
- Type and run: **gpupdate /force**
- Verify that the update completes for both **Computer** and **User** policies.

**Explanation:** This forces the client machine to retrieve and apply the latest Group Policy settings immediately. It is an important validation step because it ensures policy changes are active without waiting for the normal background refresh cycle.

<h2>Summary</h2>
This project demonstrates an end-to-end implementation of Active Directory in a cloud-based environment. Beginning with Azure infrastructure setup, the project progresses through domain deployment, automation of user creation, and enforcement of security policies.

By completing this project, the following IT skills are demonstrated:<br>
- **Azure infrastructure deployment**
- **Active Directory configuration**
- **PowerShell automation**
- **Group Policy administration**
- **User and access management**
- **Network and DNS validation**

This project reflects real-world system administration tasks and showcases the ability to design, implement, automate, and manage an enterprise-style Active Directory environment.


