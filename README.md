# Penetration Testing Report

**Footprinting & Network Scanning Phases**
W2-PM-FINAL | Cybersecurity | Networkwalks

| Field | Details |
| --- | --- |
| **Pentester Name (Cybersecurity Professional)** | Baria HitendraSinh |
| **Program/Batch** | B084-Networkwalks |
| **Date** | 12 November 2026 |
| **Modules completed** | W2-PM1 (Multiple Kali Tools), W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1. Networkwalks (secured written permission already) 2. My own local LAN network |
| **Permission secured from client?** | Yes |
| **Phases covered** | **Phase 1:** Reconnaissance & Footprinting<br>**Phase 2:** Scanning & Network Discovery<br>**Phase 3-5:** In Progress |

---

## 1. Liability Disclaimer

I have performed these activities only on the systems and devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 2. Introduction

This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

## 3. Tools Used

| Tool | Purpose |
| --- | --- |
| Kali Linux & Windows | Operating systems used for reconnaissance activities |
| WHOIS | Find domain registration details (owner, dates, name servers). |
| whatweb | Fingerprint web technologies (server, CMS, plugins, IP). |
| nslookup | Resolve the domain name to its IP address using DNS. |
| curl -I | Read the HTTP response headers of the website. |
| wafw00f | Detect whether a Web Application Firewall protects the site. |
| dnsrecon | Enumerate all DNS records (NS, MX, SPF, TXT, SRV). |
| Zenmap (Nmap GUI) | Scan the local subnet to find live hosts and IP addresses. |
| Windows CMD | Local IP and MAC address identification |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed reconnaissance against the networkwalks.com domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f and DNSRecon**. Each tool was used to collect a different type of information about the target.

First, I used **WHOIS** to obtain publicly available domain registration information and identify the domain's name servers (NS6135.HOSTGATOR.COM and NS6136.HOSTGATOR.COM). The results provided information about the domain registration and hosting infrastructure.

I then used **WhatWeb** to identify technologies used by the website. The results identified **WordPress 7.1.4** and **WP Download Manager 3.3.76**, along with other information exposed by the website.

Using **Nslookup**, I resolved the domain name to its IP address. The provided result identified **192.232.216.135**.

I used **Curl** with the `-I` option to inspect the HTTP response headers. This provided additional information about the web application and exposed the WordPress REST API endpoint `/wp-json/`.

Next, I used **Wafw00f** to determine whether a Web Application Firewall was protecting the website. The result identified **ModSecurity (SpiderLabs)**.

Finally, I used **DNSRecon** to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF/TXT records, service records and DNS software information.

### 4.2 Network Scanning with Zenmap

For the second activity, I used **Zenmap** to perform network discovery on my local network. The practical required me to identify my local IP address and subnet, discover live hosts, identify IP and MAC addresses, and generate a network topology.

I first used the Windows `ipconfig` command to identify my local IP address and LAN subnet. I then entered the subnet `192.168.56.0/24` into Zenmap and selected **Ping Scan** (`nmap -sn 192.168.56.0/24`) to identify active hosts.

The scan covered 256 IP addresses in 11.63 seconds and identified **one live host: 192.168.56.1**.

After completing the scan, I ran a ping scan against `192.168.56.1`, opened the **Topology** section in Zenmap, enabled the legend and saved the network topology as required by the practical task.

## 5. Risk Analysis / Impact

Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| --- | --- | --- | --- | --- |
| 1 | Web technology information exposed | WhatWeb identified WordPress 7.1.4 and WP Download Manager 3.3.76 | Attackers may use exposed technology/version information to identify software requiring further security review | 🟡 Medium |
| 2 | Server IP address identifiable | Nslookup resolved the domain to 192.232.216.135 | Provides information about the network location of the web service | 🟢 Low |
| 3 | HTTP technical information exposed | Curl returned HTTP response headers and exposed /wp-json/ | May assist technology fingerprinting and further enumeration | 🟢 Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application's security architecture | 🟢 Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified DNS, mail and service-related records | DNS information can help build a broader infrastructure profile | 🟡 Medium |
| 6 | Live host visible on local network | Zenmap identified one live host (192.168.56.1) on 192.168.56.0/24 | Unknown or unauthorized devices may potentially be present on a network | 🟢 Low |

**Risk level key:** 🔴 Critical · 🟡 Medium · 🟢 Low

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities.

The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules.

Therefore, the presence of information such as a software version, IP address or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.

## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

- **Review publicly exposed technology information**: Organizations should regularly review what information about their web technologies, CMS and plugins is publicly visible.
- **Keep software updated**: CMS platforms, plugins and other web technologies should be regularly updated and reviewed against current security advisories.
- **Review HTTP headers**: HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.
- **Review DNS records regularly**: DNS records should be checked periodically to ensure that only required information and services are publicly exposed.
- **Properly configure and monitor the WAF**: Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.
- **Perform regular internal network discovery**: Organizations should periodically scan their own networks to identify active devices.
- **Investigate unknown devices**: Any unexpected device discovered during network scanning should be investigated and verified.
- **Maintain network documentation**: Network topology and device information should be documented and updated regularly.
- **Perform security testing with authorization**: Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.

## 7. Conclusion

During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

In the footprinting activity, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS can provide domain information, WhatWeb can identify web technologies, Nslookup can resolve domain names, Curl can inspect HTTP headers, Wafw00f can identify a WAF, and DNSRecon can provide additional DNS information.

In the network scanning activity, I used Zenmap to identify my local network configuration and discover active hosts, collect IP and MAC address information, and create a network topology.

The exercises showed me that information gathering is an important part of cybersecurity. Even before attempting to exploit a system, a security professional can learn a significant amount about an environment by carefully analyzing publicly available information and network responses.

I also learned that technical findings should be documented clearly. A good cybersecurity report should explain what was performed, what was discovered, what the observation means, what risk it may create, and what can be done to reduce that risk.

Finally, I learned that reconnaissance and scanning must always be performed within an authorized scope. These activities were completed as part of the assigned educational cybersecurity lab.

## 8. Evidences Collected

### WHOIS
```
whois networkwalks.com
```
![WHOIS output](Footprinting%20and%20Reconnaissance/task1_whois/task1_whois_screenshot.png)


### WhatWeb
```
whatweb networkwalks.com
```
![WhatWeb output showing WordPress 7.1.4 and WP Download Manager 3.3.76](Footprinting%20and%20Reconnaissance/task2_whatweb/task2_whatweb_screenshot.png)

### Nslookup
```
nslookup networkwalks.com
```
![Nslookup output](Footprinting%20and%20Reconnaissance/task3_nslookup/task3_nslookup_screenshot.png)

### Curl (HTTP headers)
```
curl -I https://networkwalks.com
```
![Curl header output](Footprinting%20and%20Reconnaissance/task4_curl/task4_curl_screenshot.png)

### Wafw00f
```
wafw00f networkwalks.com
```
![Wafw00f output](Footprinting%20and%20Reconnaissance/task5_wafw00f/task5_wafw00f_screenshot.png)

### DNSRecon
```
dnsrecon -d networkwalks.com
```
![DNSRecon output](Footprinting%20and%20Reconnaissance/task6_dnsrecon/task6_dnsrecon_screenshot.png)

### Zenmap installation
![Zenmap installed](Network%20Scanning%20with%20Zenmap/task1_install_zenmap.png)

### Local IP address (ipconfig)
```
ipconfig
```
![ipconfig output](Network%20Scanning%20with%20Zenmap/task2_ipconfig_screenshot.png)

### Zenmap ping scan
```
nmap -sn 192.168.56.0/24
```
![Zenmap ping scan](Network%20Scanning%20with%20Zenmap/task3_ping_scan_screenshot.png)

### MAC address
![MAC address](Network%20Scanning%20with%20Zenmap/task6_mac_address_screenshot.png)

### Zenmap topology
![Zenmap topology with legend](Network%20Scanning%20with%20Zenmap/task7_topology_screenshot.png)

Topology saved as PDF: [task7_topology.pdf](Network%20Scanning%20with%20Zenmap/task7_topology.pdf)

---
