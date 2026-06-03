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

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/bf58794796900e5a957874ae5ccbc2eb9bcf55f6/enterprise-homelab-windows-server-2025-active-directory/screenshots/02-ad-user-created.png)

---

### Initial Connectivity Failure

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/a8eddae94dfe84c08a91db4b99f9f0e867290e3a/enterprise-homelab-windows-server-2025-active-directory/screenshots/03-ping-failed.png)

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/04-ping-failed.png)

---

### Firewall Rules Before / After

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/05-pfsense-rules-before.png)

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/06-pfsense-rules-after.png)

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/06-pfsense-rules-after01.png)

### Result:

- Successful communication
- Gateway reachable

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/07-ping-success.png)

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/08-ping-success.png)

---

### DNS Configuration

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/7e5c4c7235fa2294253741751164a4ad6ab72067/enterprise-homelab-windows-server-2025-active-directory/screenshots/09-dns-config.png)

---

### Domain Join

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/89698cb787bfa932b8baf9451e7002c362f10947/screenshots/10-domain-join.png)

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

![image alt](https://github.com/TheCyberGuardian25/Cybersecurity-homelab/blob/89698cb787bfa932b8baf9451e7002c362f10947/screenshots/12-whoami-success.png)

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


