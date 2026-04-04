# NAS File Server

This repository documents the configuration, setup, and operational decisions for a local NAS file server.

---

## Table of Contents
1. [Key Features and Specifications](#key-features-and-specifications)
2. [Initial Setup](#initial-setup)
3. [Configuration Settings](#configuration-settings)
4. [Mapping the NAS Drive](#mapping-the-nas-drive)
5. [Detailed Technical Information](#detailed-technical-information)

---

## Key Features and Specifications

| Feature       | Specification |
|---------------|---------------|
| NAS Model     | Buffalo TS3410D NAS |
| Processor     | Annapurna Labs Alpine AL-212 Dual-Core @ 1.4 GHz |
| RAM           | 1GB DDR3 |
| Drives        | 4 × 3.5-inch SATA hard drives (1TB each) |
| Connectivity  | Dual Gigabit Ethernet ports |
| Security      | 256-bit AES encryption, Boot Authentication, Kensington lock slot, drive bay locks |
| Backup        | NovaBACKUP (1 server/10 workstation licenses), Cloud Backup (Dropbox, S3, Azure, OneDrive), Private Cloud Replication, iSCSI, Rsync |

---

## Initial Setup

1. Place the NAS on a flat, stable surface in a well-ventilated area near your router or network switch.
2. Connect the NAS to your network using the supplied Ethernet cable.
3. Plug in the power adapter and connect it to a surge-protected outlet.
4. Power on the NAS. Wait for the power LED to turn solid, indicating readiness.
5. Download and install the [Buffalo NAS Navigator](https://buffaloamericas.com/knowledge-base/KB1068) utility.
6. Open NAS Navigator; it should detect your NAS automatically. If not, locate the NAS IP via your router using its device name or MAC address.
7. Access the NAS configuration interface either by right-clicking the device in NAS Navigator and selecting **Properties**, or by entering the NAS IP in a web browser.
8. Change the default password.

**Default credentials:**  
- Username: `admin`  
- Password: `password`  

> **Note:** For firmware v3.00+, changing the default password on first login is mandatory.

---

## Configuration Settings

### RAID

![RAID Setup](https://github.com/user-attachments/assets/0947575b-6af9-4a37-9f5a-c61b6d8f77fd)

- RAID 0 was chosen for simplicity; it offers no redundancy or fault tolerance.
- Total storage is approximately 3.6 TiB (1TB × 4 drives).

[More on RAID](#detailed-technical-information)

### File Sharing

![File Sharing](https://github.com/user-attachments/assets/e25a1521-0e16-466f-983e-4b2dfe7a97dd)

- Only required protocols are enabled; SMB is used for Windows and iOS compatibility.
- SMB2 is the minimum supported version; SMB3 is the most secure.

![SMB Version](https://github.com/user-attachments/assets/ac6b9bf4-6b94-49ca-b5a2-438a0c018e1f)

[More on SMB](#detailed-technical-information)

### IP Assignment

- Assign a static IP for consistent access and simplified management.
- Can be set via **DHCP reservation** in the router or **manual assignment** in NAS network settings.
- Ensure the IP is outside the DHCP range to prevent conflicts.

### Updates

![Automatic Updates](https://github.com/user-attachments/assets/840608bb-9918-4513-93a7-62d1c5d79c0c)

- Enable automatic updates for firmware and security patches.

### Email Notifications

![Email Notifications](https://github.com/user-attachments/assets/ee98fe56-f764-414a-9b72-880b18d398e8)

- Configure SMTP settings:

  | Parameter | Example / Description |
  |-----------|---------------------|
  | SMTP Server | `smtp.gmail.com`, `smtp.mail.yahoo.com`, etc. |
  | SMTP Port | 465 (SSL/TLS) or 587 (STARTTLS) |
  | Authentication Type | LOGIN (SMTP-AUTH) |
  | Sender Address | Your email |
  | SSL/TLS | SSL/TLS or STARTTLS |
  | Username | Your email |
  | Password | App password for Gmail/Outlook, or mailbox password for local SMTP |
  | Subject | Email subject name |
  | Recipients | List of recipient emails |

[More on SMTP Authentication](#detailed-technical-information)  
[More on SSL/TLS and STARTTLS](#detailed-technical-information)

---

## Mapping the NAS Drive

1. Open **File Explorer** and right-click **This PC**.
2. Select **Map network drive…**
3. (Optional) Choose your preferred drive letter.
4. Enter the NAS path: `\\<NAS_IP>\<share_name>` where `<NAS_IP>` is the NAS IP and `<share_name>` is the folder share name.

![Mapping NAS Drive](https://github.com/user-attachments/assets/c66335fe-5511-42bb-b668-21f7d47e0f90)

---

## Detailed Technical Information

- [RAID Levels Explained](https://www.backblaze.com/blog/nas-raid-levels-explained-choosing-the-right-level-to-protect-your-nas-data/)
- [SMB Protocol Overview](https://visualitynq.com/resources/articles/what-is-smb-what-it-decision-makers-need-to-know/)
- [SMTP Authentication Guide](https://www.mailslurp.com/blog/smtp-authentication/)
- [SSL/TLS and STARTTLS Explained](https://mailtrap.io/blog/starttls-ssl-tls/)
