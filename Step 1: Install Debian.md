### Installing Debian

## 1. Installation
- Downloaded the netinst ISO file from [Debian website](https://www.debian.org/download)  
- Flash ISO to USB drive using BalenaEtcher  
- Plug in PC and entered BIOS to boot the USB
- *Ensure ethernet is plugged in*   
- Install Debian without graphical interface to save resources
- Disable root by leaving password spot blank
- Use full disk space
- Do not use LVM
- Do not separate /home /var /tmp
- choose debian archive mirror deb.debian.org
- leave http proxy blank, *not using a proxy server so not needed*

## 2. Hardening
- 


## Issues
# Accidentally installed gnome and debian desktop environment, may have missed installing ssh and standard system utilities.
-**Remove desktop environments**
  - sudo apt remove --purge gnome* gdm3 -y
  - sudo apt autoremove --purge -y
  - sudo apt autoclean

-**Check for SSH**
  - sudo systemctl status sshd
