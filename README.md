# Secure-Remote-DNS-Sinkhole-Mesh-VPN-Deployment (Project Aegis)

A production-ready private DNS sinkhole deployment leveraging a **Raspberry Pi** and **Tailscale Mesh VPN** to provide encrypted, ad-filtered, and telemetry-free browsing across heterogeneous network environments.

## 🚀 Overview
This project addresses the challenge of maintaining privacy and network performance on restrictive Tier-1 academic and public networks. By routing DNS traffic through a hardened **Pi-hole** instance via a **Tailscale** overlay, the system achieves significant telemetry mitigation and reduced bandwidth consumption without the need for complex firewall configurations.

## 🛠️ Tech Stack
* **Hardware:** Raspberry Pi (Raspberry Pi OS / Debian)
* **Software:** Pi-hole (DNS Sinkhole), Tailscale (WireGuard-based Mesh VPN)
* **Database:** SQLite3 (Gravity DB management)
* **Protocols:** DNS, WireGuard, UDP (NAT Traversal)

## ✨ Key Features & Performance Metrics
* **Remote Ad-Blocking:** Seamless DNS-level filtering on mobile and desktop devices via global Tailscale exit nodes.
* **Restrictive Network Traversal:** Successfully bypassed Symmetric NAT and aggressive firewalls on Tier-1 academic networks.
* **Efficacy:** Achieved a consistent **40% block rate** on device egress traffic, effectively neutralizing background telemetry from OS and third-party SDKs.
* **Hardened Blocklists:** Integrated high-volume threat intelligence feeds (OISD Pro) via manual SQL injection, managing a database of **347,000+ records**.
* **Low Latency:** Optimized DNS resolution path to maintain sub-70ms RTT even when forced to fallback to DERP relaying.

## 🔧 Architecture & Deployment
### 1. Mesh VPN Integration
The Raspberry Pi is configured as a persistent node on a private Tailnet. **MagicDNS** and **Global Nameserver Override** are utilized to ensure all connected clients utilize the Pi-hole for resolution regardless of local DHCP settings.

### 2. Database Hardening
To enhance the default filtering capabilities, the SQLite3 backend was manually modified to ingest ABP-style wildcard patterns:
```bash
# Manual injection of OISD threat intelligence feed
sudo sqlite3 /etc/pihole/gravity.db "INSERT INTO adlist (address, enabled, comment) VALUES ('[https://big.oisd.nl](https://big.oisd.nl)', 1, 'OISD Pro');"
pihole -g
