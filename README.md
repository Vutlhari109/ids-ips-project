# Suricata IDS/IPS Project

A hands-on cybersecurity lab deploying **Suricata** as a network Intrusion Detection and Prevention System (IDS/IPS) on a Linux VPS. This project demonstrates real-time traffic analysis, custom rule authoring, and live threat blocking using only the command line.

---

## What This Project Covers

| Skill | How I Practice It |
|---|---|
| YAML Infrastructure as Code | Fine-tuning `suricata.yaml` — memory allocators, thread affinity, and protocol parsers — to optimise detection performance. |
| Sensor Placement & Architecture | Strategically positioning Suricata in AF-PACKET mode at network bottlenecks to achieve full traffic visibility. |
| Rule Writing Syntax | Authoring custom detection signatures using protocol fields, traffic direction keywords, and content modifiers. |
| CLI Incident Triage | Using `jq`, `grep`, `awk`, and `sed` to parse `eve.json` logs and conduct rapid triage from the terminal. |

---

## Environment

| Component | Detail |
|---|---|
| OS | Ubuntu 22.04 LTS (VPS) |
| Tool | Suricata (CLI) |
| Interface | `eth0` |
| Log Format | EVE JSON (`/var/log/suricata/eve.json`) |

---

## Setup & Configuration

### 1. Install Suricata

```bash
sudo apt update && sudo apt install suricata -y
```

### 2. Configure the Network Interface

Open the Suricata configuration file:

```bash
sudo nano /etc/suricata/suricata.yaml
```

Make the following changes:

- Set `af-packet` interface to `eth0`
- Update `HOME_NET` to your VPS IP or subnet

```yaml
# Example changes in suricata.yaml
af-packet:
  - interface: eth0

vars:
  address-groups:
    HOME_NET: "[YOUR.VPS.IP.HERE/32]"
```

### 3. Update Rulesets

Pull the latest Emerging Threats ruleset:

```bash
sudo suricata-update
```

### 4. Validate Configuration

Test the config before starting the service:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

### 5. Enable & Start the Service

```bash
sudo systemctl enable --now suricata
sudo systemctl status suricata
```

---

## Testing

### Tool Used: Nmap (Network Mapper)

Nmap was run from an attacker machine to simulate a reconnaissance scan against the VPS, triggering Suricata's built-in and custom scan-detection rules.

```bash
# Run from attacker machine
nmap -sS -A <VPS_IP>
```

---

## Results

### Intrusion Detection

Suricata successfully identified the Nmap scan in real time. Alerts were written to `eve.json` and can be queried with `jq`:

```bash
sudo cat /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'
```

Screenshots showing the attacker scan alongside Suricata alerts:
**[View Screenshots →](https://github.com/Vutlhari109/ids-ips-project/tree/d17cbb0b9cdc3fe6d83ef08bea096f9c5ba4b50d/screenshots)**

### Intrusion Prevention

With IPS mode enabled, Suricata dropped the attacker's TCP connections in real time — blocking the scan before reconnaissance data could be gathered.

> **Result:** Attacker machine received no response. TCP connections were silently dropped at the packet level.

---

## Key Takeaways

- Suricata in AF-PACKET mode provides low-overhead, wire-speed packet inspection
- EVE JSON logging enables structured, queryable alert data without a SIEM
- Custom rules can be tuned per protocol, direction, and content pattern to reduce false positives
- IPS mode (inline/NFQ) enforces active blocking with no additional tooling required
