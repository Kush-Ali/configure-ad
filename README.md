<p align="center">
  <img src="https://github.com/user-attachments/assets/a9e62984-6877-41d3-b24c-25d0460b9005" alt="Microsoft Azure logo">
</p>

<h1>Configuring Active Directory within Azure VMs</h1>

<p>In this lab, I will walk you through how I set up Active Directory within Azure Virtual Machines. I’ll configure a domain controller, join a client machine to the domain, and manage user roles and permissions in a simulated IT environment.</p>

<h2>Part 1: Setting Up the Domain Controller and Client in Azure</h2>

<p><strong>Step 1: Create a Resource Group and Virtual Network</strong></p>
<ul>
  <li>First, I created a new resource group in the Azure Portal to organize my resources for the lab.</li>
  <li>Then, I set up a virtual network with a subnet that will host both the Domain Controller (DC-1) and the client machine (Client-1).</li>
</ul>




<p align="center">
  <img src="https://github.com/user-attachments/assets/a28aacb7-6e4a-4694-bcc2-125d9a0fc7e8" alt="Creating Resource Group and Virtual Network" img width="1440"">
</p>


<p align="center">
  <img src="https://github.com/user-attachments/assets/fbff73f8-aad1-4042-8760-ea6aa65d8ac1" alt="Creating Resource Group and Virtual Network" img width="1440"">
</p>



<p><strong>Step 2: Create Domain Controller (DC-1)</strong></p>
<ul>
  <li>Next, I created a Windows Server 2022 VM named <strong>DC-1</strong> in the Azure Portal.</li>
  <li>I used the following credentials: Username: <strong>labuser</strong>, Password: <strong>Cyberlab123!</strong>.</li>
  <li>I set DC-1's NIC private IP address to static in the Azure Portal to ensure consistency.</li>
  <li>Once I logged into the VM, I disabled the Windows Firewall temporarily for testing connectivity.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/fee5f8dd-c5e0-4483-a8b1-cc29d1944198" alt="Creating Domain Controller" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/32ea2a16-14df-4a04-af2b-8bf9a406ad4b" alt="Creating Resource Group and Virtual Network" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/82a0b57c-426e-49a3-9b25-7923407bd262" alt="Creating Resource Group and Virtual Network" img width="1440"">
</p>





<p><strong>Step 3: Create Client VM (Client-1)</strong></p>
<ul>
  <li>Similarly, I created a Windows 10 VM named <strong>Client-1</strong> and attached it to the same virtual network as DC-1.</li>
  <li>I used the same credentials as DC-1: Username: <strong>labuser</strong>, Password: <strong>Cyberlab123!</strong>.</li>
  <li>Then, I set Client-1’s DNS settings to point to DC-1’s private IP address.</li>
  <li>After restarting Client-1 from the Azure Portal, I logged in and pinged DC-1’s private IP address to confirm connectivity.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/776e3ad1-c80b-4f7a-a14b-015fffcd6f6a" alt="Creating Client VM" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/8891c806-d9c7-4275-8f47-d9e784b33288" alt="Creating Client VM" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/07383999-d812-42ce-84ed-81fa317dea0a" alt="Creating Client VM" img width="1440"">
</p>





<h2>Part 2: Installing and Configuring Active Directory</h2>

<p><strong>Step 4: Install Active Directory on DC-1</strong></p>
<ul>
  <li>I logged back into DC-1 and installed Active Directory Domain Services (AD DS).</li>
  <li>Afterward, I promoted DC-1 to a Domain Controller and set up a new forest with the domain name <strong>mydomain.com</strong>.</li>
  <li>I restarted DC-1 and logged back in using <strong>mydomain.com\labuser</strong> to continue configuration.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/6c6e7928-1802-49b4-9aba-2a6bcf82a76b" alt="Installing AD DS on DC-1" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/b5fb3447-e667-4b08-bccc-8312216611f0" alt="Installing AD DS on DC-1" img width="1440"">
</p>





<p><strong>Step 5: Create a Domain Admin User</strong></p>
<ul>
  <li>Using Active Directory, I created an Organizational Unit (OU) called <strong>_ADMINS</strong>.</li>
  <li>I created a new user named <strong>pablo_davis</strong> with the password <strong>Cyberlab123!</strong>, and added her to the <strong>Domain Admins</strong> security group.</li>
  <li>Finally, I logged out and logged back in as <strong>mydomain.com\pablo_davis</strong>, which I will use as my admin account going forward.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/e599e8d2-322e-4446-a455-ee8a1ffceb32" alt="Creating Domain Admin User" img width="1440"">
</p>



<h2>Part 3: Joining Client-1 to the Domain</h2>

<p><strong>Step 6: Configure Client-1 and Join the Domain</strong></p>
<ul>
  <li>With Client-1’s DNS settings pointing to DC-1’s private IP address, I restarted the VM and logged in as <strong>labuser</strong>.</li>
  <li>I joined Client-1 to the domain <strong>mydomain.com</strong>, restarted the machine, and verified the successful domain join by logging in.</li>
  <li>Within AD, I created a new OU named <strong>_CLIENTS</strong> and moved Client-1 into it for proper organization.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/a9887bf2-7648-4ce3-a93e-ac5d4ad8fb6a" alt="Joining Client-1 to Domain" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/3cbec604-1e87-4ca8-9838-f5fa1855b922"alt="Joining Client-1 to Domain" img width="1440"">
</p>

<h2>Part 4: Setting Up Remote Desktop and Creating Additional Users</h2>

<p><strong>Step 7: Enable Remote Desktop for Non-Admin Users</strong></p>
<ul>
  <li>Logging into Client-1 as <strong>mydomain.com\pablo_davis</strong>, I enabled Remote Desktop access for domain users under System Properties.</li>
  <li>This allows non-administrative users to remotely access Client-1.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/fe991c74-9002-49dc-878f-1bdc54ea3af6" alt="Setting Up Remote Desktop" img width="1440"">
</p>




<p><strong>Step 8: Create Additional Users in Active Directory</strong></p>
<ul>
  <li>Returning to DC-1, I logged in as <strong>pablo_davis</strong> and opened PowerShell ISE as an administrator to run a script that created multiple user accounts.</li>
  <li>Once the accounts were created, I observed them in the <strong>_EMPLOYEES</strong> OU within AD.</li>
  <li>I tested one of the newly created accounts (bam.wed) by logging into Client-1.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/014ece23-2210-4afc-8988-4415eb2a3c5a" alt="Creating Additional Users" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/5aac9eab-c3e2-4d0c-b4e0-85587098be37" alt="Creating Additional Users" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/4c46b7c8-828e-465f-9fc1-a03f4dab2aeb" alt="Creating Additional Users" img width="1440"">
</p>







<h2>Part 5: Managing Account Lockouts and Monitoring Logs</h2>

<p><strong>Step 9: Configure Account Lockout Policy</strong></p>
<ul>
  <li>Back on DC-1, I configured Group Policy to lock accounts after 5 failed login attempts.</li>
  <li>I tested this by attempting to log in with an incorrect password multiple times, triggering the lockout, and then unlocked the account in Active Directory.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/f30d69dd-157f-4026-9f57-f3d774b3e95b" alt="Configuring Account Lockout Policy" img width="1440"">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/dc243cf4-ed3b-4987-ab73-69ecd60d53da" alt="Configuring Account Lockout Policy" img width="1440"">

</p><p align="center">
  <img src="https://github.com/user-attachments/assets/79ef41cb-583e-4471-a6a4-b840dc4eef9a" alt="Configuring Account Lockout Policy" img width="1440"">
</p>








<p><strong>Step 10: Monitor Logs</strong></p>
<ul>
  <li>To conclude, I used <strong>Event Viewer</strong> on both DC-1 and Client-1 to monitor login attempts and other key activities.</li>
  <li>In the <strong>Security</strong> logs, I filtered for failed login attempts (Event ID 4625) and successful logoffs (Event ID 4634) related to the test account "bam.wed".</li>
  <li>This allowed me to view important details such as the time, user, and event specifics.</li>
  <li>By monitoring these logs, I reinforced the importance of log tracking in security operations and network management.</li>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/830cec9d-440f-4b69-80e5-a02bb204f27a" alt="Monitoring Logs" img width="1440"">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/2c48cdc1-7a62-4a57-9238-f0cdd83d50a4" alt="Monitoring Logs" img width="1440"">
</p>





<h2>Conclusion</h2>
<p>I successfully set up Active Directory within Azure Virtual Machines. This lab allowed me to practice configuring a Domain Controller, joining a client machine to the domain, managing user accounts, and enabling Remote Desktop access. These configurations are essential in real-world IT environments for ensuring secure and efficient user management.</p>
