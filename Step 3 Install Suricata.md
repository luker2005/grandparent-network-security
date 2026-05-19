# Installing Suricata

**Packages**
- jq | *JSON Processor*
- suricata
- software-properties-common


## Installation
- sudo apt-get install software-properties-common
- sudo add-apt-repository ppa:oisf/suricata-stable
- sudo apt update
- sudo apt install suricata jq
- sudo nano /etc/suricata/suricata.yaml | *change interface to eno1 under af-packet, uncomment tpacket-v3*

- sudo suricata-update | *installs rules*
- sudo systemctl restart suricata

