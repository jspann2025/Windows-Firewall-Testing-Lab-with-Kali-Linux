# Windows-Firewall-Testing-Lab-with-Kali-Linux

## Overview
In this lab, I tested how the Windows Firewall responds to various Nmap scans performed from a Kali Linux virtual machine. The goal was to understand how firewall settings affect port visibility and to practice enabling, disabling, and customizing firewall rules safely in a Host-Only lab environment.

---

## Objectives
- Test port accessibility from Kali to a Windows VM
- Observe Nmap scan results with the firewall enabled and disabled
- Create and remove custom firewall rules to allow specific traffic

---

## Lab Environment
- **MacBook Air M1** running **UTM**
- **Windows 10 VM** (Host-Only mode)
- **Kali Linux VM** (Host-Only mode)
- **Nmap** pre-installed on Kali
- **Admin access** to Windows for configuring firewall

---

## Steps Performed

### Step 1: Verified VM Connectivity
I started both VMs in UTM using Host-Only networking and verified they could communicate using:
- `ping` from Kali to Windows
- `ping` from Windows to Kali

### Step 2: Disabled Windows Firewall
Using an elevated Command Prompt on Windows, I ran:
```cmd
netsh advfirewall set allprofiles state off
```
I then used Nmap on Kali to scan the Windows VM:
```bash
nmap -sS -Pn <Windows_IP>
```
As expected, I was able to see open and closed ports, including RDP (3389) and SMB (445).

### Step 3: Re-enabled Windows Firewall
I restored firewall protections with:
```cmd
netsh advfirewall set allprofiles state on
```
Running the same Nmap scan again showed all ports as "filtered", confirming the firewall was blocking access.

### Step 4: Allowed RDP Port
I created a rule to allow RDP using:
```cmd
netsh advfirewall firewall add rule name="Allow RDP" dir=in action=allow protocol=TCP localport=3389
```
I confirmed the port was open using:
```bash
nmap -p 3389 <Windows_IP>
```

### Step 5: Cleaned Up
To return to a secure state, I deleted the rule:
```cmd
netsh advfirewall firewall delete rule name="Allow RDP"
```

---

## Key Takeaways
- The Windows Firewall blocks unsolicited traffic by default.
- Nmap is a powerful tool for identifying open ports and services.
- Custom firewall rules allow granular control of inbound traffic.

---

## Reflection
This lab helped me understand firewall behavior from both the offensive (attacker) and defensive (system admin) perspectives. I reinforced my knowledge of TCP/IP, port filtering, and host-based security.

---

## Tags
`Kali Linux` `Windows Firewall` `Nmap` `UTM` `Cybersecurity Lab` `Home Lab`
