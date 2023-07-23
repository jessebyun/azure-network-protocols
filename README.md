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
<img src="https://imgur.com/zMqfyvU.png" height="80%" width="80%" alt="create_vm"/>
  <br />
  <br />
  <br />
  <img src="https://imgur.com/oZDTUQg.png" height="80%" width="80%" alt="create_vm"/>
</p>
<p>
First, we will create 2 Virtual Machines named Client-1 and Linux. Client-1 will be our Windows VM (later referred as VM1) and Linux will be our Linux VM (later referred as VM2).
</p>
  <br />
  <br />
  <br />
  <br />

<p>
  <img src="https://imgur.com/eup7Hf7.png" height="80%" width="80%" alt="create_vm"/>
</p>
<p>
 Once we have our two VM's created, we will Remote Desktop into the Windows VM (VM1) and install Wireshark. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/fX8PKa6.png" height="80%" width="80%" alt="icmp"/>
</p>
<p>
Once Wireshark is installed. We will filter traffic by ICMP which is the protocol for ping. Then we will need to find the private address for the Linux VM (VM2) because it is on the same network as the VM1. 
We found that the private IP address for VM2 is 10.0.0.5. In PowerShell, from VM1 we sent ping to 10.0.0.5 (VM2). VM2 responded 4 times, we lost 0 packets and received 4 from VM2. 
In Wireshark, the first line we can see the source IP 10.0.0.4 (VM1) and destination IP is 10.0.0.5 (VM2), protocol ICMP, and can see it is an echo or a ping request. The next line we can see the source 10.0.0.5 (VM2) and destination 10.0.0.4 (VM1) and we got an actual reply from VM2. And this happened 4 times. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/BqMLLUW.png" height="80%" width="80%" alt="icmp_ping_google"/>
</p>
<p>
  Next, we will ping www.google.com using IPv4 and we can see traffic going to Google and traffic coming back from Google. The first ping request we can see the source 10.0.0.4 (VM1) and the destination 142.251.16.99 which is one of Google’s public IP addresses. 
</p>
  <br />
  <br />
  <br />
  <br />

<p>
  <img src="https://imgur.com/6iWCaOV.png" height="80%" width="80%" alt="icmp_ping_google"/>
</p>
<p>
We can see what is actually being sent if we expand Internet Control Message Protocol and click Data. Interesting that it is sending the alphabet abcdefghiklmn... which is essentially just junk data being sent across the network.
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/iwhr2s9.png" height="80%" width="80%" alt="perpetual_ping"/>
  <br />
  <br />
  <img src="https://imgur.com/JGRsMsJ.png" height="80%" width="80%" alt="perpetual_ping"/>
</p>
<p>
Next, we are going to initiate a perpetual ping from VM1 to VM2 which is a non-stop ping from VM1 to VM2. Then in the following step we are going to change the firewall on VM2 to not allow ICMP traffic to come through. Because ping uses ICMP protocol, once we do that, we should stop seeing Echo replies from VM2. We will observe the results to get a better understanding of how firewalls work. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/9gxmMs5.png" height="80%" width="80%" alt="NSG_rule"/>
  <br />
  <br />
  <img src="https://imgur.com/KI3A7mz.png" height="80%" width="80%" alt="NSG_rule"/>
</p>
<p>
We will minimize VM1 and go back to the Azure portal and go to Network Security Groups (NSG) which is essentially a firewall in Azure. We will go into VM2 (Linux) NSG and deny inbound ICMP traffic to block pings coming from VM1. To accomplish this, we will create another rule that says for ICMP traffic for any destination, the action will be denied. The rule will be created within VM2’s Network Security Group. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/eYOEzmc.png" height="80%" width="80%" alt="icmp_blocked_wireshark"/>
</p>
<p>
Switch back to our VM1 Remote Desktop connection and you can see the ping has immediately started timing out. It is getting blocked by VM2's firewall. If we look inside Wireshark, before we got a lot of request/reply, request/reply because VM2 was getting the packets and replying. Now all we are seeing are requests because it is being blocked by the Network Security Group and no chance for VM2 to receive it and send a reply. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/9jXXG5S.png" height="80%" width="80%" alt="allow_icmp"/>
</p>
<p>
We are going to go back into Azure portal and allow VM2’s NSG to allow ICMP traffic again. We can see the replies coming back in reflected in Wireshark and PowerShell. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/cwF2is6.png" height="80%" width="80%" alt="initial_SSH_traffic"/>
</p>
<p>
Next, we are going to explore SSH traffic. SSH is essentially the same thing as Remote Desktop except there is no GUI and you can access another computer's command line. Filter by SSH traffic in Wireshark. Instead of pinging VM2, we are going to SSH into it from VM1. We are going to connect from VM1 to VM2 via Secure Shell. When we created VM1 and VM2, we also created user logins both with identical user names (Labuser). To SSH into VM2 we are going to open PowerShell and type SSH Labuser@10.0.0.5. Immediately when we try to initiate the SSH connection, we can see some SSH traffic in Wireshark.  
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/lGhCek8.png" height="80%" width="80%" alt="SSH"/>
  <br />
  <br />
  <img src="https://imgur.com/bBUm25u.png" height="80%" width="80%" alt="SSH"/>
  <br />
  <br />
  <img src="https://imgur.com/pqAE4xZ.png" height="80%" width="80%" alt="SSH"/>
  <br />
  <br />
</p>
<p>
Once connection is established via SSH, we can see on PowerShell command line: labuser@Linux.  When typing in Linux commands can see Wireshark SSH spam traffic. To close SSH connection, type exit and connection is closed and back into VM1’s command line. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/3X8Pwk3.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Next, we will observe DHCP traffic. DHCP is used to automatically assign your IP address. We can force the renewal of our IP address with command "ipconfig /renew". This VM1 is going to broadcast on our virtual network for the DHCP server in Azure to re-issue a new IP address and we can observe some DHCP traffic. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/3jTfnCo.png" height="80%" width="80%" alt="DNS"/>
</p>
<p>
Next, we will be going to observe DNS traffic. Filter for DNS traffic by typing “DNS” or “udp.port == 53”, because DNS uses port 53. We can ask the DNS server what the IP address is for any given host name. With “nslookup www.google.com” this essentially asks our DNS server: Hey, what is Google’s IP address? We can see we got an answer with some of Google’s public IP addresses. Also in Wireshark we can see we got whole bunch of Source and Destination queries from between us and our DNS server. It essentially looked up all the information about Google and returned the IP address to us. 
</p>
<br />
<br />
<br />
<br />

<p>
<img src="https://imgur.com/Y2oZyzZ.png" height="80%" width="80%" alt="RDP"/>
</p>
<p>
Last thing we will observe is RDP traffic by filtering by “tcp.port == 3389”. As soon as you filter for RDP traffic you can see spamming non-stop because there is a live, active RDP session from our host computer to our physical computer. 
</p>
<br />
<br />
<br />
<br />
