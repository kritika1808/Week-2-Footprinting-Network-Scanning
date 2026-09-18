# Week-2-Footprinting-Network-Scanning

>**Author:** Kritika Rai
> | **Program:** Cybersecurity & Ethical Hacking — Networkwalks
>| **Week:** 02
>
>**Date:** 18 September 2026

---

## 📌 Week 2 Project Overview

This repository combines the Week 2 practical modules completed during the Cybersecurity & Ethical Hacking internship at Networkwalks:

* **W2-PM1 — Footprinting & Reconnaissance with Multiple Kali Tools**
* **W2-PM5 — Network Scanning with Zenmap**

The first module covers reconnaissance and information gathering against the assigned course lab target. The second module covers host discovery and network mapping on the author's own local LAN.

---

## Liability Disclaimer
 
I have performed these activities only on systems and networks where I had secured written permission, or that I own myself. All material in this repository is for education and research purposes only. Do not use anything from here to break the law. The instructor, the authors, and Networkwalks are not responsible for what you do with this knowledge. Every action taken is my own responsibility. Misuse of these techniques can lead to criminal charges, heavy fines, loss of employment, and a permanent record — in most countries, unauthorised access is a crime even when nothing is damaged.
 
| | |
|---|---|
| **Client / Target 1** | `networkwalks.com` (assigned lab target, permission granted by the course) [`Permission Letter`](./W2-PM-SamplePermissionLetterv1.pdf)|
| **Client / Target 2** | My own home LAN (`192.168.0.0/24`) |
| **Permission secured?** | Yes, for both targets |

---

# 🔎 W2-PM1 — Footprinting & Reconnaissance

## Objective

The objective of this module was to perform passive reconnaissance and footprinting of the assigned lab target, `networkwalks.com`, using built-in Kali Linux tools.

The report used six reconnaissance tools:

| Tool       | Purpose                                       |
| ---------- | --------------------------------------------- |
| `whois`    | Domain registration and registrar information |
| `whatweb`  | Web technology fingerprinting                 |
| `nslookup` | DNS-to-IP resolution                          |
| `curl -I`  | HTTP response-header analysis                 |
| `wafw00f`  | Web Application Firewall detection            |
| `dnsrecon` | DNS record enumeration                        |

## Commands Used

```bash
whois networkwalks.com
whatweb networkwalks.com
nslookup networkwalks.com
curl -I https://networkwalks.com
wafw00f networkwalks.com
dnsrecon -d networkwalks.com
```

## Key Findings

* **Registrar:** GoDaddy.com, LLC
* **Name Servers:** `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`
* **Server IP:** `192.232.216.135`
* **Web Server:** Apache
* **CMS:** WordPress 7.1
* **Plugin:** WordPress Download Manager 3.3.58
* **WAF:** ModSecurity (SpiderLabs)
* **Mail Server:** `mail.networkwalks.com`
* **DNS Software:** BIND 9.16.23-RH
* **DNSSEC:** Not enabled
* **REST API Endpoint:** `/wp-json/`
* **Public Contact:** `info@networkwalks.com`

## Main Observations

The reconnaissance exercise demonstrated how different tools reveal different layers of a target's public footprint.

* `whois` provided domain registration information.
* `whatweb` identified web technologies and versions.
* `nslookup` resolved the domain to its IP address.
* `curl` exposed HTTP response headers and the WordPress REST API endpoint.
* `wafw00f` identified the Web Application Firewall.
* `dnsrecon` enumerated DNS, mail, SPF, TXT and SRV records.

> These observations are **not confirmed vulnerabilities**. Further authorized testing would be required to validate any actual weakness.

## Recommendations

1. Reduce software-version disclosure.
2. Keep WordPress and plugins updated.
3. Restrict or authenticate unnecessary REST API access.
4. Add security headers such as HSTS, CSP and X-Frame-Options.
5. Enable DNSSEC.
6. Harden SPF and consider DKIM/DMARC.
7. Suppress DNS software version disclosure.
8. Reduce publicly exposed contact details.
9. Keep the WAF tuned and monitored.
10. Perform reconnaissance only with authorization.

## 📸 Screenshots & Evidence

### WHOIS
![WHOIS](/01_whois.PNG)

### WhatWeb
![WhatWeb](/02_whatweb.PNG)

### Nslookup
![Nslookup](/03_nslookup.PNG)

### Curl
![Curl](/04_curl_headers.PNG)

### Wafw00f
![Wafw00f](/05_wafw00f.PNG)

### DNSRecon
![DNSRecon](/06_dnsrecon.PNG)


---

# 🖥️ W2-PM5 — Network Scanning with Zenmap

## Objective

This module focused on network discovery and mapping of the author's own local LAN using Zenmap, the GUI front-end for Nmap.

## Network Details

| Item            | Result                      |
| --------------- | --------------------------- |
| Local IPv4      | `192.168.0.105`             |
| Subnet Mask     | `255.255.255.0`             |
| CIDR Subnet     | `192.168.0.0/24`            |
| Default Gateway | `192.168.0.1`               |
| Live Hosts      | 6                           |
| Scan Type       | Host Discovery              |
| Scan Duration   | Approximately 11.06 seconds |

## Tools Used

* Windows 11
* Nmap 7.991
* Zenmap
* Npcap
* Windows Command Prompt

## Commands Used

### Find Local IP Address

```cmd
ipconfig
```

### Host Discovery Scan

```bash
nmap -sn 192.168.0.0/24
```

The `-sn` option performs host discovery without performing a port scan.

## Discovered Hosts

| IP Address      | MAC Address         | Identification                   |
| --------------- | ------------------- | -------------------------------- |
| `192.168.0.1`   | `D8:47:32:B1:43:91` | TP-Link router / default gateway |
| `192.168.0.100` | `86:ED:4A:88:99:6A` | Unknown / likely mobile device   |
| `192.168.0.101` | `3A:FC:04:DC:88:04` | Unknown / likely mobile device   |
| `192.168.0.102` | `D0:9C:AE:3A:7A:FB` | vivo smartphone                  |
| `192.168.0.104` | `FC:A6:67:98:46:6C` | Amazon smart/IoT device          |
| `192.168.0.105` | Not shown by Nmap   | Author's Windows PC              |

The report notes that `192.168.0.100` and `192.168.0.101` use randomized privacy MAC addresses, so their vendors could not be identified from the OUI.

## Zenmap Topology

After the host-discovery scan, the Zenmap **Topology** tab was reviewed and the network topology was exported as:

[`topology.pdf`](./topology.pdf)

The topology showed the discovered hosts around the local machine.

## Main Observations

The scan demonstrated that a host connected to the LAN could discover six live hosts using a single ping scan.

The exercise also demonstrated:

* Identifying the local subnet.
* Discovering live hosts.
* Identifying MAC addresses.
* Identifying device manufacturers using MAC OUI information.
* Visualizing network topology using Zenmap.
* Exporting topology information as a PDF.

> These are **network-discovery observations, not confirmed vulnerabilities**. No port scanning, service enumeration, exploitation or vulnerability validation was performed.

## Recommendations

1. Maintain an inventory of all network devices.
2. Investigate unidentified hosts.
3. Secure the router and change default administrative credentials.
4. Disable unnecessary remote management.
5. Keep router firmware updated.
6. Use strong WPA2/WPA3 Wi-Fi security.
7. Separate IoT and guest devices using guest networks or VLANs.
8. Keep host firewalls enabled.
9. Perform regular internal discovery scans.
10. Keep network topology documentation updated.
11. Scan only networks for which authorization exists.

---

# 🔗 Combined Week 2 Learning

These two modules demonstrate two related stages of a penetration-testing workflow:

```text
        Reconnaissance
              ↓
         Footprinting
              ↓
  Identify domains, IPs,
 technologies & infrastructure
              ↓
      Network Discovery
              ↓
 Identify live hosts &
    network topology
              ↓
 Further Authorized
    Security Testing
```

---

## 🧠 Skills Demonstrated

### Reconnaissance & Footprinting

* Domain reconnaissance
* WHOIS analysis
* DNS enumeration
* Web technology fingerprinting
* HTTP header analysis
* WAF identification
* DNS record analysis
* Security risk identification

### Network Scanning

* IP address identification
* Subnet identification
* Host discovery
* Nmap usage
* Zenmap usage
* MAC/OUI analysis
* Network topology visualization
* Network documentation

### Reporting

* Security findings documentation
* Risk analysis
* Security recommendations
* Technical report preparation
* Evidence collection
* Authorized security testing practices

---

# 📚 Project Reports

The complete reports for the individual modules are included in this repository:


1. [`W2-PM1_Footprinting_Reconnaissance_Report.pdf`](./W2-PM1_Footprinting_Reconnaissance_Report.pdf)

2. [`W2-PM5_Zenmap_Network_Scanning_Report.pdf`](./W2-PM5_Zenmap_Network_Scanning_Report.pdf)


---

# 🎯 Conclusion

Week 2 provided practical experience with both **reconnaissance** and **network discovery**.

The footprinting module demonstrated how publicly available information can reveal domain, DNS, web technology, server and security-control details.

The Zenmap module demonstrated how network discovery can identify live hosts, device information and network topology within an authorized local environment.

Together, these exercises provided practical exposure to important cybersecurity concepts including:

* Reconnaissance
* Footprinting
* DNS Enumeration
* Web Fingerprinting
* WAF Detection
* Host Discovery
* Network Mapping
* Risk Analysis
* Security Documentation

---

## 👩‍💻 Author

**Kritika Rai**

Cybersecurity & Ethical Hacking Internship
**Networkwalks**

---

⭐ *This repository documents educational cybersecurity work performed in authorized environments.*
