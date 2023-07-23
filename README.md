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

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 22.04

<h2>High-Level Steps</h2>

- Create 2 Virtual Machines (Windows & Linux) within Azure environment
- Remote Desktop into Windows VM and install Wireshark to monitor network traffic
- Monitor ICMP traffic
- SSH into Linux VM via PowerShell and monitor SSH traffic
- Also will be monitoring DCHP, DNS, and RDP traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://imgur.com/zMqfyvU.png" height="80%" width="80%" alt="xxxxxx"/>
  <br />
  <br />
  <br />
  <img src="https://imgur.com/oZDTUQg.png" height="80%" width="80%" alt="xxxxxx"/>
  <br />
  <br />
  <br />
  <img src="https://imgur.com/eup7Hf7.png" height="80%" width="80%" alt="xxxxxx"/>
</p>
<p>
First, we will create 2 Virtual Machines named Client-1 and Linux. Client-1 will be our Windows VM (later referred as VM1) and Linux will be our Linux VM (later referred as VM2). We will then remote login to Windows VM (VM1) and install Wireshark. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="xxxxxx"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="xxxxxx"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
