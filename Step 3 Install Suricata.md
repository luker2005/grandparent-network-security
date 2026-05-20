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






# Issues

**DNS Can't resolve to test suricata**
*When trying to use http://testmynids.org to test suricata rules, I recieve a curl code 6. when testing with nslookup I get SERVFAIL reply from quad100.*
*Tailscale MagicDNS is overriding interface configurations*
- sudo tailscale set --accept-dns=false | *disables magicdns*