# PENETRATION-TESTING-REPORT-FOOTPRINTING-NETWORK-SCANNING-PHASES
This report covers footprinting networkwalks.com with Kali Linux tools  and scanning my own network with Zenmap (W2-PM5). Networkwalks gave me permission for the footprinting, so it was done legally. Together they show how an attacker goes from gathering information to mapping hosts.
# Penetration Testing Report: Footprinting and Network Scanning Phases

W2-PM-FINAL | Cybersecurity | Networkwalks

## Report Details

| Item | Details |
|---|---|
| Pentester Name (Cybersecurity Professional) | Samuel Donkor |
| Program/Batch | B084 Networkwalks |
| Date | 12 November 2026 |
| Modules completed | W2-PM1 (Multiple Kali Tools), W2-PM5 (Zenmap Scanning) |
| Client/Target | 1. Networkwalks (secured written permission already) 2. My own local LAN network |
| Permission secured from client? | Yes |
| Phases covered | Phase 1: Reconnaissance and Footprinting. Phase 2: Scanning and Network Discovery. Phase 3 to 5: In progress |

## 1. Liability Disclaimer

I have performed these activities only on the systems and devices where I had secured written permission, or on devices and systems that I own myself. All these materials are for education and research purposes only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 2. Introduction

This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

Networkwalks gave written permission for the footprinting of its domain, so this work was legal and authorised. The scanning was done only on my own network.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

## 3. Tools Used

The table below lists each tool used in this report and its purpose.

| Tool | Purpose |
|---|---|
| Kali Linux and Windows | Operating systems used for reconnaissance activities |
| WHOIS | Find domain registration details (owner, dates, name servers) |
| whatweb | Fingerprint web technologies (server, CMS, plugins, IP) |
| nslookup | Resolve the domain name to its IP address using DNS |
| curl -I | Read the HTTP response headers of the website |
| wafw00f | Detect whether a Web Application Firewall protects the site |
| dnsrecon | Enumerate all DNS records (NS, MX, SPF, TXT, SRV) |
| Zenmap (Nmap GUI) | Scan the local subnet to find live hosts and their IP addresses |
| Windows CMD | Local IP address and subnet identification |

## 4. Activities Performed

### 4.1 Footprinting and Reconnaissance

I performed reconnaissance against the networkwalks.com domain using six Kali Linux tools: WHOIS, WhatWeb, Nslookup, Curl, Wafw00f and DNSRecon. Each tool was used to collect a different type of information about the target.

**WHOIS** (`whois networkwalks.com`)

First, I used WHOIS to obtain publicly available domain registration information and identify the domain's name servers. The results provided information about the domain registration and hosting infrastructure.


**WhatWeb** (`whatweb networkwalks.com`)

I then used WhatWeb to identify technologies used by the website. The results identified WordPress 7.0.4 and WP Download Manager 3.3.58, along with other information exposed by the website.


**Nslookup** (`nslookup networkwalks.com`)

Using Nslookup, I resolved the domain name to its IP address. The result identified 192.232.216.135.


**Curl** (`curl -I https://networkwalks.com`)

I used Curl with the -I option to inspect the HTTP response headers. This provided additional information about the web application and exposed the WordPress REST API endpoint `/wp-json/`.


**Wafw00f** (`wafw00f https://networkwalks.com`)

Next, I used Wafw00f to determine whether a Web Application Firewall was protecting the website. The result identified ModSecurity (SpiderLabs).


**DNSRecon** (`dnsrecon -d networkwalks.com`)

Finally, I used DNSRecon to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF and TXT records, service records and DNS software information.


### 4.2 Network Scanning with Zenmap

For the second activity, I used Zenmap to perform network discovery on my local network. The practical required me to identify my local IP address and subnet, discover live hosts and their IP addresses, and generate a network topology.

I first used the Windows `ipconfig` command to identify my local IP address and LAN subnet. I then entered the subnet into Zenmap and selected Ping Scan to identify active hosts.



The scan identified ten live hosts:

- 192.168.0.45
- 192.168.0.21
- 192.168.0.2
- 192.168.0.58
- 192.168.0.78
- 192.168.0.1
- 192.168.0.8
- 192.168.0.7
- 192.168.0.4
- 127.0.0.1/8 (my own machine, loopback)

After completing the scan, I opened the Topology section in Zenmap, enabled the legend and saved the network topology in PDF format as required by the practical task.



## 5. Risk Analysis / Impact

Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Web technology information exposed | WhatWeb identified WordPress and WP Download Manager | Attackers may use exposed technology and version information to identify software requiring further security review | 🟡 Medium |
| 2 | Server IP address identifiable | Nslookup resolved the domain to 192.232.216.135 | Provides information about the network location of the web service | 🟢 Low |
| 3 | HTTP technical information exposed | Curl returned HTTP response headers and exposed `/wp-json/` | May assist technology fingerprinting and further enumeration | 🟢 Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application's security architecture | 🟢 Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified DNS, mail and service-related records | DNS information can help build a broader infrastructure profile | 🟡 Medium |
| 6 | Multiple live hosts visible on local network | Zenmap identified ten live hosts on my network | Unknown or unauthorised devices may potentially be present on a network | 🟡 Medium |

Risk level key: 🔴 Critical, 🟡 Medium, 🟢 Low

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules.

Therefore, the presence of information such as a software version, IP address or DNS record does not by itself mean that the system is vulnerable. Further authorised security testing would be required to confirm any actual vulnerability.

## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

1. **Review publicly exposed technology information.** Organizations should regularly review what information about their web technologies, CMS and plugins is publicly visible.
2. **Keep software updated.** CMS platforms, plugins and other web technologies should be regularly updated and reviewed against current security advisories.
3. **Review HTTP headers.** HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.
4. **Review DNS records regularly.** DNS records should be checked periodically to ensure that only required information and services are publicly exposed.
5. **Properly configure and monitor the WAF.** Keep the WAF (ModSecurity) enabled and tuned, as it adds a useful layer of defence.
6. **Perform regular internal network discovery.** Organizations should periodically scan their own networks to identify active devices.
7. **Investigate unknown devices.** Any unexpected device discovered during network scanning should be investigated and verified.
8. **Maintain network documentation.** Network topology and device information should be documented and updated regularly.
9. **Perform security testing with authorization.** Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.

## 7. Conclusion

During Week 2 of my Cybersecurity and Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

In the footprinting activity, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS can provide domain information, WhatWeb can identify web technologies, Nslookup can resolve domain names, Curl can inspect HTTP headers, Wafw00f can identify a WAF, and DNSRecon can provide additional DNS information.

In the network scanning activity, I used Zenmap to identify my local network configuration and discover active hosts. I also collected IP address information and created a network topology.

The exercises showed me that information gathering is an important part of cybersecurity. Even before attempting to exploit a system, a security professional can learn a significant amount about an environment by carefully analyzing publicly available information and network responses.

I also learned that technical findings should be documented clearly. A good cybersecurity report should explain what was performed, what was discovered, what the observation means, what risk it may create, and what can be done to reduce that risk.

Finally, I learned that reconnaissance and scanning must always be performed within an authorised scope. These activities were completed as part of the assigned educational cybersecurity lab, with permission from Networkwalks for the domain and on my own network for the scan.

## 8. Evidence collected 
<img width="1535" height="853" alt="who is command on  project 2" src="https://github.com/user-attachments/assets/61949b90-8349-45ea-a174-67e100eeccd7" />
<img width="1518" height="766" alt="2" src="https://github.com/user-attachments/assets/9cf07bd7-044a-4c0c-a6d6-d0d7b87581fa" />
<img width="1535" height="863" alt="3" src="https://github.com/user-attachments/assets/3615eb50-d8da-4954-995d-210201ec5773" />
<img width="1535" height="863" alt="7" src="https://github.com/user-attachments/assets/72d8177f-f701-4e7c-9818-8cc4f9974470" />
<img width="1534" height="861" alt="6" src="https://github.com/user-attachments/assets/2d22e5bf-955e-41bd-8460-bbf9900963a4" />
<img width="1535" height="863" alt="5" src="https://github.com/user-attachments/assets/723f0e2d-48c5-48c0-a118-e04835a72a33" />
<img width="1534" height="860" alt="4" src="https://github.com/user-attachments/assets/29638c7a-61ba-4ef4-a07c-2b68dd8b5183" />

