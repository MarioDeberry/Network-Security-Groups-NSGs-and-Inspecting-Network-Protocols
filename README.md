<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)
- Windows Powershell

## Operating Systems Used
- Windows 10 (21H2)
- Ubuntu Server 20.04

## High-Level Steps
- Create VMs and Install Wireshark
- Observe ICMP Traffic 
- Observe SSH Traffic  
- Observe RDP Traffic

## Actions and Observations

### 1. Setting Up Virtual Machines and Installing Wireshark
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/VM2%20Private%20IP.png" height="80%" width="80%" alt="VM Private IP"/>
</p>
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Ping%20VM2.png" />
</p>
Set up two virtual machines in Microsoft Azure. Reference https://github.com/MarioDeberry/Configuring-Active-Directory-within-Azure-VMs to learn how to create virtual machines. 
VM1 will use Windows 10 OS and VM2 will use Ubuntu.  
On VM1, install Wireshark to capture traffic.  
Open Powershell and run it as administrator. Test connectivity between the two VMs by pinging VM2's private IP.  
In Wireshark, select "Ethernet" and type ICMP in the search bar to capture traffic between both VMs.

### 2. Applying Network Security Groups (NSG) Rules
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Network%20Security%20Groups.png" height="80%" width="80%" alt="Network Security Group"/>
</p>
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/VM2%20Inbound%20Security.png" height="80%" width="80%" alt="Inbound Security"/>
</p>
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Ping%20to%20VM2%20Timed%20Out.png" height="80%" width="80%" alt="Ping Timeout"/>
</p>
In the Azure portal, search for Network Security Group and select it.  
You might see two network groups created for VM2. Apply changes to both groups.  
Select VM2nsg, go to Inbound Rules, and click Add.  
Set the protocol to ICMP and change the action to Deny.  
Set the priority to 200 so that it is recognized before other rules. Name it DENY_PING_FROM_ANYWHERE.  
After adding the rule, go back to VM1 and observe that the ICMP traffic has timed out in Wireshark.  
Edit the rule back to Allow in Azure and observe that the requests are received again in Wireshark.

### 3. Observing SSH Traffic
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/SSH%20Traffic.png" height="80%" width="80%" alt="SSH Traffic"/>
</p>
Stop the Wireshark capture.  
Type ssh in the search bar and press enter.  
In Powershell, type ssh and connect to VM2 by entering the following: `ssh user@VM2`.  
Enter the password when prompted.  
You will see the large amount of traffic generated between VM1 and VM2 in Wireshark.  
After observing, type exit in Powershell to return to VM1.

### 4. Inspecting DHCP Traffic
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/DHCP%20Traffic.png" height="80%" width="80%" alt="DHCP Traffic"/>
</p>
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/DHCP%20Renew%20Traffic.png" height="80%" width="80%" alt="DHCP Renew Traffic"/>
</p>
In Wireshark, filter the traffic by typing DHCP in the search bar.  
Continue without saving the current capture.  
Use the nslookup command in Powershell followed by a website name (e.g., `nslookup www.example.com`) to generate DHCP traffic. Observe the traffic in Wireshark.

### 5. Monitoring Remote Desktop Protocol (RDP) Traffic
<p>
  <img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Desktop%20Traffic.png" height="80%" width="80%" alt="RDP Traffic"/>
</p>
To monitor RDP traffic, type tcp.port==3389 into the Wireshark search bar.  
This will display the real-time traffic occurring between VM1 and VM2 via RDP.

---

With these steps, you can effectively monitor and analyze various network traffic types between two Azure VMs while experimenting with Network Security Groups to control access.
