<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- <a href="#section1">Step 1: Creating Virtual Machines in Azure</a>
- <a href="#section2">Step 2: Observing Network Traffic with Wireshark</a>
- <a href="#section3">Step 3: Configuring a Firewall (Network Security Group)</a>
- <a href="#section4">tep 4: Use Wireshark to Observe Various Protocols</a>
 <section id="section1">
<h2>Part 1: Creating Virtual Machines in Azure</h2>
 </section>
       
    
**Step 1: Access Azure Portal**

- Open a web browser and navigate to the Azure portal: https://portal.azure.com/

**Step 2: Create a Resource Group**

![image](https://github.com/user-attachments/assets/3610849b-9e11-48ed-aab4-a087746bdcd8)

- Resource groups help organize Azure resources.
- Follow the Azure portal prompts to create a new resource group.

**Step 3: Create a Windows 10 Virtual Machine (VM)**

![image](https://github.com/user-attachments/assets/9651db63-6e5d-4876-91ee-21280ceade03)

- In the Azure portal, search for "Virtual Machines" and click on it.
- Click "Add" to create a new virtual machine.
- Select the previously created resource group.
- Choose a name and size for your Windows 10 VM.
- Configure other settings like username/password and storage.
- **Important:** During VM creation, enable the option to "Create a new virtual network" and subnet.

![image](https://github.com/user-attachments/assets/21bae671-f09d-42dc-ad77-df68d5a20587)

**Step 4: Create a Linux (Ubuntu) VM**

- Repeat steps similar to creating the Windows 10 VM.
- Select the same resource group and virtual network created earlier.
- Choose a name and size for your Ubuntu VM.
- Configure other settings like username/password and storage.
  
![image](https://github.com/user-attachments/assets/6abed0e8-209a-4693-a5d6-970a483955f8)

- Ensure authentication type is set to "Username/Password".
- Use the same Virtual network as the Win10 Machine

**Step 5: Verify VM Creation**

- After creating both VMs, verify they are listed in the Azure portal under the chosen resource group and are running in the same virtual network/subnet.

![image](https://github.com/user-attachments/assets/e5a099d7-7d70-4ff9-8d7a-6b095740dee5)

<section id="section2">
<h2>Part 2: Observing Network Traffic with Wireshark</h2>
</section>
**Step 1: Connect to Windows 10 VM (Mac Users)**

- If using a Mac, download and install Microsoft Remote Desktop for Mac.

**Step 2: Remote Desktop Connection**

- Use Remote Desktop to connect to your Windows 10 VM using its public IP address found in the Azure portal.

**Step 3: Install Wireshark on Windows 10 VM**

- Download and install Wireshark on your Windows 10 VM.

**Step 4: Start Packet Capture in Wireshark**

- Open Wireshark and start capturing network traffic.

**Step 5: Filter for ICMP Traffic**

- In Wireshark, apply a filter to only capture ICMP traffic (e.g., icmp).

**Step 6: Ping Ubuntu VM and Observe Traffic**

- Retrieve the private IP address of your Ubuntu VM (linux-vm) from the Azure portal.
- Open a command prompt or PowerShell on your Windows 10 VM.
- Ping the Ubuntu VM's private IP address (e.g., ping <private_ip_address>).
- Observe the ICMP ping requests and replies in Wireshark.

![image](https://github.com/user-attachments/assets/6b68123d-523e-4c19-ba32-2e5aabcef69c)

**Step 7: Ping a Public Website and Observe Traffic**

- From your Windows 10 VM, ping a public website like www.google.com.
- Observe the network traffic associated with the ping in Wireshark.
<section id="section3">
<h2>Part 3: Configuring a Firewall (Network Security Group)</h2>
</section>
**Step 1: Initiate Ping to Ubuntu VM**

- Start a continuous ping from your Windows 10 VM to your Ubuntu VM.

**Step 2: Block ICMP Traffic on Ubuntu VM's NSG**

- In the Azure portal, navigate to the Network Security Group (NSG) associated with your Ubuntu VM.
- Create a new inbound security rule that **denies** ICMP traffic.

![image](https://github.com/user-attachments/assets/65da54f8-2d5f-43bc-9adf-019b9bd14656)

**Step 3: Observe Blocked Ping Traffic**

- Observe the changes in Wireshark and the ping command on your Windows 10 VM. The ping should fail due to the blocked ICMP traffic.

![image](https://github.com/user-attachments/assets/1f657006-a769-40d6-962b-d5f06a1f0d8c)


**Step 4: Allow ICMP Traffic Again**

- In the Ubuntu VM's NSG, edit the security rule to **allow** ICMP traffic.

**Step 5: Observe Allowed Ping Traffic**

![image](https://github.com/user-attachments/assets/4a6f5742-60e8-4f32-b10e-1be02cbb5e1c)

<section id="section4">
<h2>Part 4: Use Wireshark to Observe Various Protocols:</h2>
</section>

**Step 1: Observe SSH Traffic**

- Log back into the windows-vm
- Back in Wireshark, start a packet capture up
- Filter for SSH traffic only
- From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address)
- Open PowerShell, and type: ssh labuser@<private IP address>
- Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark

![image](https://github.com/user-attachments/assets/0394b0a1-f510-47d5-bd65-4bd3b026e92c)

**Step 2: Observe DHCP Traffic**

- Back in Wireshark, filter for DHCP traffic only
- From your Windows 10 VM, attempt to issue your VM a new IP address from the command line
- Open PowerShell as admin and run: ipconfig /renew
- Observe the DHCP traffic appearing in WireShark

![image](https://github.com/user-attachments/assets/82e98acb-1601-4e54-8b86-46b217b43abb)

**Step 3: Observe DNS Traffic**

- Back in Wireshark, filter for DNS traffic only
- From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are
- Observe the DNS traffic being show in WireShark

![image](https://github.com/user-attachments/assets/f414d54a-195b-4f8f-8f24-57a577ec55b7)

**Step 4: Observe RDP Traffic**

- Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
- Observe the immediate non-stop spam of traffic.
- This is due to the constant feed that shows you (end user) a live stream from one computer to the other.

![image](https://github.com/user-attachments/assets/f95532f6-5321-42c8-979f-313ded8018a3)
