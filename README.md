# Sonicwall Configuration Outline

## FIRST STEP OUT OF THE BOX
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
