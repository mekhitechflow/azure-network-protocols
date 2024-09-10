<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>
<p>In this tutorial, I will be going over Network Security Groups (NSGs) and we will be Inspecting Traffic Between two Azure Virtual Machines (Windows & Linux). First, you will need to go into Azure and create these two VMs for this lab, each VM will need 2vcpus 16Gb, and they will need to both be connected to the same VNET, this is crucial for this lab so after creating your first VM wait around 10 minutes before making the next one and you should be good to go. Next, take the public IP of the VM (windows) and insert it into Remote Desktop so that we can access the VM. Once logged into Windows, we will need to download Wireshark... use https://www.wireshark.org/download.html this program allows us to see the traffic in real time. Once downloaded we will look at ICMP traffic in Wireshark, so filter ICMP traffic only for this first portion. ICMP (Internet Control Message Protocol) is a network layer protocol that allows devices to communicate with each other about network status and connectivity. Ping is associated with this protocol, in order to see the connectivity status you must use ping. In our cmd prompt, as you can see when we use ping 10.0.0.5 (VM-Linux private IP) it shows that they are able to connect to one another. Try ping with another website for example use www.google.com and see the traffic on Wireshark.
</p>

<p>
<img width="1279" alt="Screenshot 2024-09-02 at 10 56 30 PM" src="https://github.com/user-attachments/assets/20e10b18-766d-4e0d-9a42-c9667e1f9dba">
</p>
<br />

<p>
Next, we are going to create a continuous ping between VM1 and VM2 using the command (ping 10.0.0.5 -t). This will continuously ping to each other until we manually stop the ping ourselves. While this is happening we are going to go back into Azure and create a firewall for VM2 and block ICMP traffic... this is done by creating a new Network Security Group for inbound security rules specifically on VM2. This will cause VM1 to lose connection with VM2 and it will start to time out! Once you have had enough of seeing requested time out, go back into Azure and change the Network Security Group from deny to allow and it will start connecting again!
</p>

<p>
<img width="1110" alt="Screenshot 2024-09-02 at 11 06 25 PM" src="https://github.com/user-attachments/assets/e11c2cf0-3c10-494e-945b-e091e5e3ef3c">
</p>

<p>
<img width="1276" alt="Screenshot 2024-09-02 at 11 06 51 PM" src="https://github.com/user-attachments/assets/ed6be711-32e2-4504-9951-6071aae45ae7">
</p>
<br />

<p>
The next protocol we will filter is SSH, SSH or Secure Shell, is a network protocol that allows secure communication between computers over an unsecured network. The difference between RDP and SSH, is the graphic aspects, when you connect using SSH you are simply just connecting a computer's command line. So, in Wireshark set the filter to SSH and in your command line prompt use ssh (your username for vm2)@(vm2 private IP). Once you do this you will see Wireshark start to show SSH traffic.  
</p>

<p>
![ssh](https://github.com/user-attachments/assets/67f7cc4d-023a-4211-b04d-65061fc0a554)
</p>
<br />

<p>
Furthermore, now we are going to dive into DHCP protocol. DHCP stands for Dynamic Host Configuration Protocol, which is a network protocol that automatically assigns IP addresses to devices on a network. So, because when our VMs were made they were automatically assigned an IP that we aren't going to be ale to see the traffic of within Wireshark. So, what we are going to do is us the command ipconfig /renew so that we get a new IP address and this will allow us to see DHCP traffic within Wireshark.
</p>

<p>
<img width="1325" alt="Screenshot 2024-09-02 at 11 27 48 PM" src="https://github.com/user-attachments/assets/eed1aa93-5e86-43e5-ba44-93f2ae0a1a68">
</p>
<br />

<p>
The next protocol we will see traffic for is DNS. Domain Name System, it's a system that translates domain names into IP addresses. This allows users to type domain names into their browsers, like "google.com" or "nytimes.com", instead of having to remember the IP address for each website. For this section of the tutorial we are going to use nslookup and whatever website you want to use, (Ex: nslookup www.google.com) and this will return you the IP address of the website you choose. Once you do this look at the traffic in Wireshark.
</p>

<p>
![dns](https://github.com/user-attachments/assets/fbb0a288-1c3f-4f91-b0f3-5eba349d65bf)
</p>
<br />

<p>
Lastly, we will filter the last protocol of this tutorial which is RDP (Remote Desktop Protocol), to see the traffic you will need to enter (tcp.port == 3389) and you will see Wireshark start going crazy because we are currently running RDP to even access VM1 so the traffic is going to show a lot more than the other protocols which is pretty cool!
</p>

<p>
<img width="1398" alt="Screenshot 2024-09-02 at 11 36 19 PM" src="https://github.com/user-attachments/assets/772b0ea0-52fc-46ce-a14c-a24be52f1e6a">
</p>
<br />
