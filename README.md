# Sonicwall Configuration Outline

## First Step Out of the Box
  - Start from Safemode: (Recommended)
    - Enter Safemode by booting up the firewall – then using a paper clip or similar sized item, insert 
      into the small hole either in front or back of the firewall, and hold the “button” down for 10 
      seconds or more. The wrench light will start flashing, and you can release the “button”
    - Load latest firmware and boot to factory defaults*
    - Safemode is not required but is the most cautious method and leaves least room for 
      technology induced issues
  - If you do not start from Safemode, do the following:
    - Skip Setup Guide (Wizard)
    - Register the appliance (you cannot load firmware unless the appliance is registered (if you are 
      not in Safemode))
    - Load latest firmware and boot to factory defaults* 
     - Reason: Issues in configuration created in old/initial release RTM firmware can survive firmware upgrades; this step eliminates this chance, however small it may be.

## Settings and Backup
  - Do these at the start and the end and have them labeled appropriately. 
  - Export Settings
  - Create Backup

## One Touch Configuration
  ### System > Administration
    - Password must be changed every 90 days (not that reccomended, honestly)
    - Bar repeated pssword changes for 4 changes.
    - Enforce password complexity: Require alphabetic, numeric, and symbolic characters.
    - Apply the above password contrainst for all user categories.
    - Enable administrator/user lockout.
    - Failed login attempts per minuite before lockout: 7
    - Enable inter-administrator Messaging polling interval (seconds): 10
  ### Network > Interfaces
    - Any interface allowing HTTP management is replaced with HTTPS Management
    - Any setting to 'Add rule to enable redirect from HTTP to HTTPS' is disabled.
    - Ping Management is disabled on all interfaces.
   
   ### Network > Zone
    - Intrusion Prevention is enabled on all applicable default Zones.
    - Gateway Anti-Virus protection is enabled on all applicable default Zones.
    - Anti-spyware protection is enabled on all applicable default Zones.
  
  ### Network > DNS
    - Enable DNS Rebinding protection.
    - DNS Rebinding Action: Log Attack & Drop DNS Reply
  
  ### Firewall > Access Rules
    - Any Firewall policy with an Action of Deny, the Action is changed Discard.
    - Source IP Address connection limiting with a threshold of 128 connections is enabled for all firewall polices.
  
  ### Firewall Settings > Advanced
    - Turn on 'Enable Stealth Mode'
    - Turn on 'Randomize IP ID'
    - Turn off 'Decrement IP TTL for Forwarded Traffic'
    - Connections are set to: 'Maximum DPI Connections (DPI Services Enabled'
  
  ### Firewall Settings > Flood Proctections
    - Turn on 'Enable TCP Handshake Timeout'
  
  ### VPN > Advanced
    - Turn on 'Enable IKE Dead Peer Detection'
    - Turn on 'Enable Dead Peer Detection for Idle VPN Sessions'
    - Turn on 'Enable Fragmented Packet Handling'
    - Turn on 'Ifnore DF (Don't Fragment) Bit'
    - Turn on 'Enable NAT Traversal'
    - Turn on 'Clean up Active Tunnels when Peer Gateway DNS Name Resolves To A Different Address'
    - Turn on 'Preserve IKE port for Pass Through Connections'
  
  ### Security Services > Gateway Anti-Virus
    - If licensed, Enable Gateway Antivirus
    - Configure Gateway AV Settings: Turn on 'Disable SMTP Responses'
    - Configure Gateway AV Settings: Turn off 'Disable detetion of EICAR test virus'
    - Configure Gateway AV Settings: Turn on 'Enable HTTP Byte-Range requests with Gateway AV'
    - Configure Gateway AV Settings: Turn on 'Enable FTP Rest Request with Gateway AV'
    - Configure Gateway AV Settings: Turn off 'Enable HTTP Clientless Notification Alerts'
  
  ### Security Services > Intrusion Prevention
    - If licensed, Enable IPS
    - Turn on 'Prevent All and Detect All for High Priority Attacks'
    - Turn on 'Prevent All and Detect All for Medium Priority Attacks'
    - Turn on 'Prevent All and Detect All for Low Priority Attacks'
  
  ### Security Services ? Anti-Spyware
    - If licensed, Enable Anti-Spyware
    - Turn on 'Prevent All and Detect All for High Priority Attacks'
    - Turn on 'Prevent All and Detect All for Medium Priority Attacks'
    - Turn on 'Prevent All and Detect All for Low Priority Attacks'
    - Configure Anti-Spyware Settings: Turn on 'Disable SMTP Responses'
    - Configure Anti-Spyware Settings: Turn off 'Enable HTTP Clientless Notification Alerts'
  
  ### Log > Settings 
    - Set Logging Level: Debug
    - Set Alert Level: Warning
  
  ### Log > Name Resolution
    - Set Name Resolution Method to 'DNS the NetBIOS'
  
  ### Internal Settings
    - Turn on 'Protect Against TCP State Manipulation DoS'
    - Turn on 'Apply IPS Signatures Bidirectionally'
    - Allow launching of AppFlow monitor in stand-alone browser frame
    - Enable Visualization UI for Non-Admin/Config Users
## SYSTEM
  ### System >>> Administration
    - For PCI/Better Security: Change super user administration account from ‘admin’
    - Enable Enhanced Audit Logging (new in 6.2.5 for UC APL certification)
    - Change HTTPS management port from 443 to other (i.e. 8443)
    - Change the default Admin Timeout from 5 minutes, to your preferred amount
  ### System >>> Time
    - Set time automatically using NTP
    - Don’t configure additional NTP Servers unless needed for internal synchronization
  ### System >>> Diagnostics
    - Check Network Settings
    - Note that “Content Filtering” will fail until licensed or if UDP/2257 is blocked from SonicWALL Firewall to SonicWALL GRID CFS Servers. https://support.software.dell.com/kb/sw9405
    - MTU Discovery: Paste determined value into WAN/Internet interface used in test
    - Note: Some ISP’s values change each time you test. In that case review ISP’s KB for optimum MTU

## Network
  ### Network >>> Interfaces
    - Link Speed: Set to Auto or, if possible, hard code on both sides to best possible (i.e. 100/full duplex, 1 Gb)
    - WAN Interfaces and enabling management (don’t open to whole world):
    - Create Address Objects for external IP addresses that can manage device
    - Add those to new Address Object Group created for this purpose
    - Edit WAN:WAN HTTPS and HTTP Management auto rules and change Source to newly created group (Note: Alternatively VPN first to manage firewall)
### Network >>> Zones
  - Make sure all desired security services are enforced on proper zones
  - Network >>> Services:
  - “Any” in firewall policy does not mean ‘any’ unless firewall knows about the service. Therefore, add any Services required but not present.
  - Log Event ID 41: Network Access >>> Unknown Protocol Dropped will expose these
### Network Address Translation
  - On outbound policy, select specific outbound Interface (Not recommended to use “Any”)
  - On inbound policy, select specific inbound Interface (Public Server Wizard sets this to Any)
  - If you use Public Server Wizard:
  - Only use once to show/use as a reference then manually create additional Firewall/NAT policies
  - While the Wizard is good for creating a quick policy, it’s recommended to build policies manually for learning purposes (not to mention the Wizard creates 3 policies each time).
  - Rename & change PSW inbound NAT policy’s inbound Interface from Any to specific interface
  - Don’t create unnecessary Loopback policies
  - Outbound Many-to-Few : Use Address Object type Range for Source Translated. Range preferred because of predictable outcome. Example of why to use this: YouTube can blacklist a university due to too many connections from one WAN IP. (Note: This is if you own a block of IP addresses)
  - NEVER set Service Translated to same Service as Service Original, always use ORIGINAL or PAT Port. This could occur due to replicating a config from another manufacturer's firewall
  - Good idea to NOT use firewall's Primary WAN IP (or other high priority IP in Subnet, such as your outbound email SMTP IP) for Guest network’s Outbound NAT Policy. This protects against blacklisting.
## Firewall Settings
  ### Firewall Settings > Advanced
    - Enable Stealth Mode, Enable Randomize IP ID & Enable Decrement IP TTL for forwarded traffic.
    - KB Article Reference: https://support.sonicwall.com/kb/sw12547
    - Choose best “Connections” setting. There is a Help icon you can mouse-over to view these values. If connections in ‘DPI Connections’ mode is enough, use that as it has best DPI performance. (Allocates more memory to DPI/Requires a reboot)
