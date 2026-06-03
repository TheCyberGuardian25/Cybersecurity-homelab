# 🛡️ Active Directory Domain Join Across Segmented VLANs using pfSense | Cybersecurity Homelab

## Overview

Built an enterprise-style homelab in Proxmox to simulate a segmented network environment and securely join a Windows client to an Active Directory domain.

The objective was to validate domain authentication while maintaining network isolation using pfSense firewall rules.

---

## Technologies Used

- Proxmox VE
- Windows Server 2025
- Active Directory Domain Services (AD DS)
- DNS
- pfSense
- VLAN Segmentation
- Windows Client

---

## Environment

| Component | Role |
|----------|------|
| VLAN50 | User Workstation |
| VLAN70 | Domain Controller |
| pfSense | Firewall |
| Proxmox | Virtualisation |

---

## Objective

- Create Active Directory users
- Configure DNS
- Troubleshoot connectivity
- Configure pfSense firewall rules
- Join workstation to domain
- Validate authentication

---

## Screenshots

### Network Architecture

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/81fa76af59bc80f01931e24c53e8b19bab85584f/enterprise-homelab-windows-server-2025-active-directory/screenshots/01-lab-topology.png)


---

### Created Active Directory User

<img width="1258" height="570" alt="screenshots:02-ad-user-created" src="https://github.com/user-attachments/assets/9e1ea1c3-f2ce-4278-aa7a-0511a61bc79c" />

---

### Initial Connectivity Failure

<img width="1016" height="404" alt="screenshots:03-ping-failed png " src="https://github.com/user-attachments/assets/b653c5b5-8a7b-4c78-9873-081d83ad65a4" />

<img width="1039" height="407" alt="screenshots:04-ping-failed" src="https://github.com/user-attachments/assets/bccb37ec-4ee8-442e-bc6c-2359738c246b" />

---

### Firewall Rules Before / After

<img width="1258" height="570" alt="screenshots:05-pfsense-rules-before" src="https://github.com/user-attachments/assets/6a19215a-67f0-401f-96f1-4f4d298d3106" />

<img width="1095" height="468" alt="screenshots:06-pfsense-rules-after" src="https://github.com/user-attachments/assets/652d3498-d77e-4e1b-b52d-f489680f0f65" />

### Result:

- Successful communication
- Gateway reachable

<img width="1095" height="539" alt="screenshots:07-ping-success" src="https://github.com/user-attachments/assets/6f869e5e-7ba2-4b59-8958-7821ce6d8400" />

<img width="1095" height="539" alt="screenshots:08-ping-success" src="https://github.com/user-attachments/assets/e39bc81e-e21b-4db6-b457-15f678085f80" />

---

### DNS Configuration

<img width="1095" height="648" alt="screenshots:11-dns-config" src="https://github.com/user-attachments/assets/86e8e0e0-2232-4225-969b-19912a75a92d" />

---

### Domain Join

<img width="1095" height="539" alt="screenshots:09-domain-join" src="https://github.com/user-attachments/assets/8eef60ae-4c60-471e-87af-8cc2db0179e8" />

---

### Validation

Command:

```powershell
whoami
```

Output:

```text
lab\jsmith01
```

<img width="1095" height="539" alt="screenshots:10-whoami-success" src="https://github.com/user-attachments/assets/460f0da5-11a6-44e5-ba05-a46d444bcc82" />

---

## 🎥 Full Walkthrough

Watch the complete build and validation process here:

https://youtu.be/KDRAYsE_c1M?si=z-6oQuqbPbBqExKh

---

## Key Skills Demonstrated

- Active Directory Administration
- DNS Configuration
- VLAN Segmentation
- pfSense Firewall Management
- Connectivity Troubleshooting
- Domain Authentication
- Enterprise Network Concepts

---

## Lessons Learned

This project demonstrated how firewall rules and DNS directly impact Active Directory communication and domain authentication across segmented VLAN environments.


