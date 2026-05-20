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

## Adding Rulesets
- sudo suricata-update enable-source ptresearch/attackdetection | *repeat with rulesets listed below*
- 






# Issues

**DNS Can't resolve to test suricata**
*When trying to use http://testmynids.org to test suricata rules, I recieve a curl code 6. when testing with nslookup I get SERVFAIL reply from quad100.*
*Tailscale MagicDNS is overriding interface configurations*
- sudo tailscale set --accept-dns=false | *disables magicdns*
- sudo systemctl restart networking


**ET Open rules not triggering when testing**
*When trying to use http://testmynids.org, the rule is not triggering*
- sudo suricata-update | *this does nothing and is showing that the rules are loaded even when using the -v flag*
- grep -r "GPL ATTACK_RESPONSE" /etc/suricata/rules/ | *does not show any output, meaning the rule is not in the /rules directory*
- sudo nano /etc/suricata/suricata.yaml | *I realized my network range was not set correctly, and changed in the config file*
*rule is now triggering and traffic is showing in the tail output for fast.log*