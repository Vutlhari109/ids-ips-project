 # Intrusion Detection System and Intrusion Prevention System ( IDS/IPS ) Project

 ## What Is This Project?

 A hands-on IDS/IPS Project using Suricata CLI version to detect and prevent intrusions on a Linux VPS Server, this projects demonstrate my skills in Detecting and Preventing intrusions in real time using rulesets configured on an IDS/IPS tool

---

## What This Lab Teaches Me

| Skill | How I Practice It |
|-------|-------------------|
| YAML Infrastructure as Code | anaging complex system deployments by fine-tuning variables, memory allocators, and protocol parsers inside suricata.yaml. |
| Sensor Placement & Architecture | Designing network visibility by strategically positioning Suricata mirrors at critical network bottlenecks. |
| Rule Writing Syntax:|  Drafting customized detection signatures using exact protocol fields, traffic directions, and content modifiers. |
| Command-Line Data Triaging | Mastering text processing utilities like jq, grep, awk, and sed to run rapid incident triage directly from the CLI.|


---

 ## Installations

 ```bash
sudo apt install suricata -y
```

## Configure Network Interface
### Before starting Suricata, I configuired Suricata to listen eth0 network interface 

 ```bash
sudo nano /etc/suricata/suricata.yaml
```

- Updated af-packet to eth0
- Updated HOME_NET to VPS Ip

### Updated rulesets

 ```bash
sudo suricata-update
```

### Start & Verify the Service

 ```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
sudo systemctl enable --now suricata
sudo systemctl status suricata
```
## Testing IDS/IPS 
### Used NMAP (Nertwok Mapper) to test ruleset

## Intrusion Detection

### Attached [Screenshorts]([https://google.com)](https://github.com/Vutlhari109/ids-ips-project/tree/d17cbb0b9cdc3fe6d83ef08bea096f9c5ba4b50d/screenshots)
 indicates attcker machine running nmap and Suricata detecting the intrusion on VPS server


## Intrusion Prevention
### Attacker was blocked, network connection (tcp) were dropped




