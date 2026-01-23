# NAS Homelab
This repository documents the configuration and operational decisions for a simple, local NAS file server. It is intended for learning, reproducibility, and homelab reference purposes.





## Table of Contents 
1. [Key features and specifications](#key-features-and-specifications-anchor-point)
2. [Initial Setup](#initial-setup-anchor-point)
3. [Setting configurations](#setting-configurations-anchor-point)
4. [Mapping NAS drive](#mapping-nas-drive-anchor-point)
5. [Detailed technical information](#detailed-technical-information-anchor-point)










<a name="key-features-and-specifications-anchor-point"></a>
## Key Features and Specifications

NAS model: &emsp;Buffalo TS3410D NAS <br>
Processor: &nbsp;&nbsp;&nbsp;&emsp;Annapurna Labs Alpine AL-212 Dual-Core @ 1.4 GHz <br>
RAM: &emsp;&emsp;&emsp;&emsp;1GB DDR3 <br>
Drives: &nbsp;&nbsp;&emsp;&emsp;&emsp;4 3.5-inch SATA hard drives (1TB each) <br>
Connectivity: &nbsp;&nbsp;&nbsp;Dual Gigabit Ethernet ports <br>
Security: &emsp;&emsp;&nbsp;&nbsp;&nbsp;256-bit AES encryption, Boot Authentication, Kensington lock slot, drive bay locks <br>
Backup: &emsp;&emsp;&emsp;Supports NovaBACKUP (1 server/10 workstation licenses), Cloud Backup (Dropbox, S3, Azure, OneDrive), Private Cloud Replication, iSCSI, Rsync <br>










<a name="initial-setup-anchor-point"></a>
## Initial Setup

Place the NAS on a flat, stable surface in a well-ventilated area near router or network switch. <br>

Connect one end of the supplied Ethernet cable to one of the LAN ports on the back of the NAS and the other end to an available port on the router or network switch.<br>

Plug the AC adapter into the power port on the back of the device and connect the other end to a surge-protected power outlet. <br>

Turn on the NAS. <br>

The power LED on the front should start blinking. Wait for the LED to turn solid, indicating the system is ready for configuration. <br>

Download and install the <a href="https://buffaloamericas.com/knowledge-base/KB1068" target="_blank" rel="noopener noreferrer">Buffalo NAS Navigator</a> utility from the official Buffalo support website. <br>

Open NAS Navigator. It should automatically detect the NAS on the network. (If it does not detect the NAS, go into router admin page and manually search for device via device name or MAC address to find the NAS IP address) <br>

Right-click the device in NAS Navigator and select "Properties" or enter the IP address of the NAS into a web browser to access the configuration interface. <br>

The default username is admin and the password is password. NOTE: For newer firmware (v3.00+), you must change this password immediately upon first login.

Change default password.










<a name="setting-configurations-anchor-point"></a>
## Setting Configurations

### RAID
<img width="241" height="179" alt="image" src="https://github.com/user-attachments/assets/0947575b-6af9-4a37-9f5a-c61b6d8f77fd" />

RAID 0 doesn’t provide fault tolerance or redundancy. There’s no data redundancy and an increased risk of data loss. However, as the purpose of this file server is for temporary storage of unimportant files and RAID 0 is the easiest to setup, this option was chosen.

NOTE: The above image shows around 3.6TB but really means *gibibytes* not gigabytes. The drives are 1TB each which correctly results in a total storage of around 3.6 TiB as shown.

[More information on RAID](#detailed-technical-information-anchor-point)

### File sharing
<img width="1424" height="354" alt="image" src="https://github.com/user-attachments/assets/e25a1521-0e16-466f-983e-4b2dfe7a97dd" />

All protocols and features which are not used/needed are disabled for security. This file server will only be used by Windows and iOS devices. SMB is the native file sharing protocol for Windows and has universal compatibility with other platforms including iOS, and thus SMB is enabled. 

<img width="301" height="301" alt="image" src="https://github.com/user-attachments/assets/ac6b9bf4-6b94-49ca-b5a2-438a0c018e1f" />

SMB3 provides the most secure form of protection and thus is the upper limit. The lower limit is only between the options SMB1 (which is insecure) and SMB2 and thus SMB2 is chosen.

[More information on SMB](#detailed-technical-information-anchor-point)

### IP Assignment
Set a static IP address for reliable access, external access (although not pertinent in this case), easier management, simplified configuration; basically for ease of use. This can be done either through a DHCP reservation or by statically assigning an IP address.

NOTE: <br>
DHCP reservation - Done through the router to always assign the same IP address for the NAS. <br>
Manual assignment - Done through the NAS network settings. Make sure to have it assigned outside of the DHCP range to avoid conflicts.

### Update
<img width="881" height="262" alt="image" src="https://github.com/user-attachments/assets/840608bb-9918-4513-93a7-62d1c5d79c0c" />

Enable automatic updates.

### Email Notifications
<img width="703" height="583" alt="image" src="https://github.com/user-attachments/assets/23269c73-876c-4cb8-b733-6fdd44cca8bf" />

Enable email notifications and fill out following information: <br> <br>
smtp.gmail.com or smtp.mail.yahoo.com or etc... google "smtp server address for ___ mail" <br>
Port 465 (SSL/TLS) or Port 587 (STARTTLS) <br>
LOGIN (SMTP-AUTHENTICATION) <br>
Sender address. <br>
SSL/TLS or STARTTLS <br>
The SMTP login username. NOTE: Same email as the sender address <br>
The SMTP account password. NOTE: Gmail / Outlook → use an App Password, NOT your normal password. Local Postfix → mailbox password or SASL credentials <br>
Email subject name. <br>
Recipient emails to be added. <br>

[More information on SMTP authentication](#detailed-technical-information-anchor-point)

[More information on SSL/TLS and STARTTLS](#detailed-technical-information-anchor-point)










<a name="mapping-nas-drive-anchor-point"></a>
## Mapping NAS Drive

Open File Explorer and right-click on "This PC" in the left pane. <br>

Press "Map network drive..." <br>

Optional: Change drive letter to your desired letter. <br>

Input NAS drive in the format "\\\server\share" where "server" is the IP address of the NAS and "share" is the name of the folder share you created in the NAS. <br>
Ex:

<img width="607" height="448" alt="image" src="https://github.com/user-attachments/assets/c66335fe-5511-42bb-b668-21f7d47e0f90" />











<a name="detailed-technical-information-anchor-point"></a>
## Detailed Technical Information

<a href=https://www.backblaze.com/blog/nas-raid-levels-explained-choosing-the-right-level-to-protect-your-nas-data/
target="_blank" rel="noopener noreferrer">More information on RAID</a>

<a href="https://visualitynq.com/resources/articles/what-is-smb-what-it-decision-makers-need-to-know/"
target="_blank" rel="noopener noreferrer">More information on SMB</a>

<a href="https://www.mailslurp.com/blog/smtp-authentication/"
target="_blank" rel="noopener noreferrer">More information on SMTP authentication</a>

<a name="ssl-tls-starttls-anchor-point"></a>
<a href="https://mailtrap.io/blog/starttls-ssl-tls/"
target="_blank" rel="noopener noreferrer">More information on SSL/TLS and STARTTLS</a>
 
