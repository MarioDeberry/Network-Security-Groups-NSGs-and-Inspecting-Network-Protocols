<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)
- Windows Powershell

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create VMs and Install Wireshark
- Observe ICMP Traffic 
- Observe SSH Traffic  
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/VM2%20Private%20IP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Ping%20VM2.png"/>
</p>
<p>
Set up two virtual machines within Microsoft Azure. Reference https://github.com/chrisrraP/configure-ad to learn how to create virtual machines. One will use Windows 10 OS (VM1) and the other will use Ubuntu (VM2). Using remote desktop, log into VM1. Browse the internet for the protocol analyzer called Wireshark and download the original version. Install using it's default settings. You will not have to check any boxes during installation. After installation, type "Powershell" into the search bar on the lower left area of the screen. Right-click on Windows Powershell and run as administrator. Verify connectivity by perpetually pinging VM2's private IP address. Run Wireshark. Select "ethernet"; type "ICMP" in the search bar at the top of the window and press enter. Traffic between both VMs will post on Wireshark.
</p>
<br />

<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Network%20Security%20Groups.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/VM2%20Inbound%20Security.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Ping%20to%20VM2%20Timed%20Out.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Head back into the Azure portal, type "Network Security Group" into the search bar and select that option. There may be an instance where two groups were created for VM2. You will need to apply these steps to both groups. Select VM2nsg, select "inbound rules" and select "add". Toggle the protocol to "ICMP" and the "allow" action to "deny". The priority will be changed to "200" in order for it to be recognized before other inbound rules. We'll name it "DENY_PING_FROM_ANYWHERE". Click "add" and go back to VM1. Observe the ICMP traffic on wireshark and Powershell. It will have timed out. Switch back to Azure and edit the rule back to "allow". Go to VM1 to observe the requests being received again.
</p>
<br />

<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/SSH%20Traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Wireshark window, press the stop button. Type "ssh" in the search bar and hit enter. Continue without saving. In Powershell, type in "ssh" and press enter. Next, type in your VM2 username @ VM2 (ex: user@VM2). You will be prompted to enter your password. Enter your VM2 password and you will see a large amount of traffic in Wireshark. After observing, type "exit" in powershell to revert back to VM1.
</p>
<br />

<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/DHCP%20Traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/DHCP%20Renew%20Traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Wireshark, filter DHCP traffic by typing in "DHCP" in the search bar and press enter. Continue without saving. In Powershell, use command "nslookup" followed by a space and any website of your choice. Observe all the traffic created.
</p>
<br />

<p>
<img src="https://github.com/chrisrraP/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/blob/main/Desktop%20Traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, to observe the Remote Desktop Protocol (RDP), you will type "tcp.port==3389" into the Wireshark search bar. This will display the real-time traffic that occurs between both computers.
</p>
<br />
