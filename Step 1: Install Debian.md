# Installing Debian

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
- sudo apt update && sudo apt upgrade -y


## Issues
### Accidentally installed gnome and debian desktop environment, may have missed installing ssh and standard system utilities.
-**Remove desktop environments**
  - sudo apt remove --purge gnome* gdm3 -y
      - *may need to reboot after this*
  - sudo apt autoremove --purge -y
  - sudo apt autoclean
  - sudo reboot

-**Check for SSH**
  - sudo systemctl status sshd
    - *not found, install using following steps*
-**Install SSH**
  - sudo apt install openssh-server -y
  - sudo systemctl enable ssh
  - sudo systemctl start ssh
  - sudo systemctl status sshd | *this confirms it is running*






### sudo apt update && sudo apt upgrade not working. removing gnome must have messed something up.
-**Check for connectivity**
  - ping 8.8.8.8
  - *Network is unreachable*

-**Bring interface back online**
  - ip a | *shows network interfaces*
  - sudo ip link set eno1 up | *eno1 is the interface name, this command sets it to up*
  - sudo networkctl up eno1 | *configures interface*
  - sudo systemctl restart systemd-networkd
  - ping 8.8.8.8 | *this failed so I had to check the network config*

-**Fix network config file**
  - cat /etc/network/interface | *This is to check what is written in the file*
      - *Missing the ethernet interface*
  - sudo nano /etc/network/interfaces
     - Add these lines:
       .# The primary network interface | dont add the period, i just put it there so it wouldnt turn that into a header in this doc
       auto eno1
       iface eno1 inet dhcp
     - Ctrl +, Ctrl + X
  - sudo systemctl restart networking
