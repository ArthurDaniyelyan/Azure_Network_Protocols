<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

# Azure Network Security Groups & Protocol Inspection

## Overview
This project demonstrates the setup of Azure Network Security Groups (NSGs) to control inbound/outbound traffic and inspect **network protocols using packet capture and monitoring tools.

- NSG Implementation
- Custom Security Rules
- Packet Inspection using Azure Network Watcher
- Traffic Analysis with Wireshark (optional)

---

## 1. Create an Azure Virtual Network (VNet)
1. Log in to [Azure Portal](https://portal.azure.com).
2. Go to Virtual Networks → Click Create.
3. Set the following:
   - Resource Group: `NSG-Project-RG`
   - VNet Name: `NSG-VNet`
   - Region: Select your desired region
   - Address Space: `10.0.0.0/16`
4. Create two subnets:
   - Web-Subnet: `10.0.1.0/24`
   - DB-Subnet: `10.0.2.0/24`
5. Click Review + Create → Create**.

---

## 2. Deploy Virtual Machines (VMs)
### Create Web & Database VMs
1. Go to Virtual Machines → Create a VM.
2. Configure:
   - Web Server VM (`Web-VM`) in Web-Subnet
   - Database Server VM (`DB-VM`) in DB-Subnet
   - Disable Public IP for `DB-VM` (internal access only).
4. Choose Standard B2s (2 vCPUs, 4GB RAM) for both.
5. Set OS: Windows Server 2022 or Ubuntu (your choice).
6. Click Review + Create → Create.

---

## 3. Configure Network Security Groups (NSGs)
1. Navigate to Network Security Groups in Azure.
2. Click Create NSG → Name it `Web-NSG`.
3. Associate Web-NSG with `Web-Subnet`.
4. Create inbound rules:

| Rule Name       | Priority | Protocol | Source | Destination | Port | Action |
|-----------------|----------|----------|--------|-------------|------|--------|
| Allow-HTTP      | 100      | TCP      | Any    | Web-VM      | 80   | Allow  |
| Allow-SSH       | 110      | TCP      | Any    | Web-VM      | 22   | Allow  |
| Deny-All        | 4000     | Any      | Any    | Any         | Any  | Deny   |

5. Create DB-NSG for `DB-Subnet`, allowing only Web-VM to connect.
6. Associate DB-NSG with `DB-Subnet`.

---

## 4. Monitor & Inspect Network Traffic
### Enable Azure Network Watcher
1. Go to Azure Network Watcher → Enable in your region.

### Capture Packets Using Packet Capture
1. Under Network Watcher, select Packet Capture.
2. Choose Web-VM → Click Start Capture.
3. Generate traffic by accessing the web server (`curl http://<Web-VM-IP>`).
4. Stop capture and download the `.cap` file.

### Analyze with Wireshark (Optional)
1. Open Wireshark → Load the `.cap` file.
2. Apply filters like `tcp.port == 80` to inspect web traffic.

---

## 5. Test & Validate Security Rules
1. Attempt SSH (Port 22) from an unauthorized source → Should be blocked.
2. Try accessing DB-VM from an external source → Should be denied.
3. Verify HTTP requests to Web-VM → Should be allowed.
4. Use `nslookup`, `ping`, and `tracert` to analyze connectivity.

---


---

## Key Skills Demonstrated
- Azure Networking & NSGs
- Security Rule Configuration
- Packet Capture & Protocol Inspection
- Basic Traffic Analysis with Wireshark


