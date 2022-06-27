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

## SYSTEM
  - System >>> Administration
  - For PCI/Better Security: Change super user administration account from ‘admin’
  - Enable Enhanced Audit Logging (new in 6.2.5 for UC APL certification)
  - Change HTTPS management port from 443 to other (i.e. 8443)
  - Change the default Admin Timeout from 5 minutes, to your preferred amount
  - System >>> Time
  - Set time automatically using NTP
  - Don’t configure additional NTP Servers unless needed for internal synchronization
  - System >>> Diagnostics
  - Check Network Settings
  - Note that “Content Filtering” will fail until licensed or if UDP/2257 is blocked from SonicWALL Firewall to SonicWALL GRID CFS Servers. https://support.software.dell.com/kb/sw9405
  - MTU Discovery: Paste determined value into WAN/Internet interface used in test
  - Note: Some ISP’s values change each time you test. In that case review ISP’s KB for optimum MTU

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
    - Source IP Address connection limiting with a threshold of 128 connections is enabled for all firewall police
