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
- sudo passwd -l root | *this locks the root account*
**SSH config Hardening**
- sudo nano /etc/ssh/sshd_config
	*change these settings*
	- PermitRootLogin no
	- X11Forwarding no | *unnecessary*
	- MaxAuthTries 3
	
- ssh-keygen -t ed25519 -C "IDS key" | *this is done on the computer being used to ssh into server*
- ls -l ~/.ssh/id* | *do this on server to verify there are no ssh keys already*
- mkdir -p ~/.ssh && chmod 700 ~/.ssh | *create directory for ssh keys*
- nano ~/.ssh/authorized_keys | *paste the key here* | run type $env:USERPROFILE\.ssh\id_ed25519.pub in powershell to find public key*
- chmod 600 ~/.ssh/authorized_keys
- *disabling password authentication* | sudo nano /etc/ssh/sshd_config -> PasswordAuthenitcation no
- sudo systemctl restart sshd 

**UFW firewall config**
- sudo apt install ufw -y

- sudo ufw allow OpenSSH | *This prevents losing ssh access when turning on UFW*
- sudo ufw allow ssh 
- sudo ufw allow 53/tcp | *this will be used for pi-hole*
- sudo ufw allow 53/udp | *used for pi-hole*
- sudo ufw allow 80/tcp | *pi-hole web dashboard*

- sudo ufw show added | *verify all rules have been added*
- sudo ufw enable

**Fail2Ban Setup**
- sudo apt install fail2ban -y | *install fail2ban*
- sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local | *ensures settings will persist through package updates*
- sudo nano /etc/fail2ban/jail.local
	*set these values:*
	- bantime  = 1h
	- findtime  = 10m
	- maxretry = 3
- sudo systemctl enable fail2ban 
- sudo systemctl start fail2ban 
- sudo fail2ban-client status sshd

**Automatic security updates**
- sudo apt install unattended-upgrades -y | *this package automatically insatalls security updates*
- sudo dpkg-reconfigure unattended-upgrades | *runs the setup wizard*
- sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
	-*uncomment these lines*
		Unattended-Upgrade::Origins-Pattern {
    "origin=Debian,codename=${distro_codename},label=Debian-Security";
};

		Unattended-Upgrade::Remove-Unused-Dependencies "true";
		
- sudo systemctl enable unattended-upgrades
- sudo systemctl start unattended-upgrades

**Static IP config**
- sudo nano /etc/network/interfaces 
	- change dhcp to Static
	- address 192.168.x.x
	- netmask 255.255.255.0
	- gateway 192.168.x.1
	- dns-nameservers 8.8.8.8
	- 

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

    
