# Installing Pi-Hole

## Installation:
- wget the installer | wget -O basic-install.sh https://install.pi-hole.net
- sudo bash basic-install.sh
- select eno1 for interface
- choose Quad9 for DNS
- keep unifified hosts list when asked
- choose anonymous for privacy mode

## Configuration
- sudo usermod -aG pihole www-data | *run this so that lists can be added from the web interface*
- *in web interface add the blocklists listed below* 

**Blocklists**
- https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/multi.txt
- https://big.oisd.nl
- https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/fake.txt
- https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/tif.txt