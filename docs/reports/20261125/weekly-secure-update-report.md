# Weekly IT Security Update — Week Ending 2026-11-25

Coverage: 2026-11-18 to 2026-11-25. Compiled from public news and vendor/agency sources via web search; figures are as first reported and may change. Several items come from aggregators/trackers rather than primary sources and are flagged "unverified". Searches for ENISA, UK NCSC and Vietnam-specific advisories returned nothing for this week.

## 1. Major security incidents

| Incident | Highlights |
|---|---|
| VMware vCenter zero-day (CVE-2026-123890) | Authentication bypass disclosed Nov 19–20; reportedly exploited in the wild since September 2026. Added to CISA KEV on Nov 20 (federal deadline Nov 23). |
| AI-agent exploitation of MONAI Label | Amazon threat intel reported autonomous agents on the open-source Cairn framework found and exploited a path-traversal zero-day (CVE-2026-133968, CVSS 9.8) to take over university servers in East Asia (campaign began Sept 8, ~48 hours). Fix in v0.8.6 (Nov 22). |
| ScreenConnect / MSP attacks | BleepingComputer (Nov 11) reports active ScreenConnect exploitation breaching dozens of MSPs and their downstream customers. |
| OPSSHIELD update infrastructure | Attacker used compromised credentials to access OPSSHIELD's update-distribution CDN (Nov 2) — supply-chain risk. |
| Check Point gateways | UNC5221 reportedly exploited the PHP2Shell flaws to compromise Check Point Security Gateways. |
| Breach trackers (unverified) | Tower Records (~11M users), Samsung subsidiary, Colla Health (336K), European Commission, myColorado (taken offline), Swedish exhibition/hotel operator and a Croatian payment provider. Confirm against primary statements. |

## 2. Major tool and platform security updates

- **Broadcom VMSA-2026-0011 (Nov 24):** new flaws in vCenter, ESX, Workstation, Fusion, VMware Tools and Avi Load Balancer — separate from CVE-2026-123890. Critical ESX issues could allow remote code execution on hosts.
- **CISA KEV additions:**
  - Nov 23: N-able MailAssure (SSRF, reported CVE-2026-134624, CVSS 9.3; sources disagree on CVE ID) and MONAI Label; due Nov 26.
  - Nov 20: VMware vCenter, SAP NetWeaver, Cisco Expressway, Apache Roller.
  - Earlier in Nov: SonicWall SonicOS SSLVPN (Nov 6); Cisco BroadWorks XXE, Barracuda, Openwave, Chromium use-after-free (Nov 10); UnrealIRCd, Check Point (Nov 13).
- **Microsoft November Patch Tuesday (Nov 10–11):** roughly 1,000+ CVEs (counts vary by source), 63 critical, including an exploited Outlook spoofing zero-day (CVE-2026-124194).
- **Dorsett Controls InfoScan (SCADA):** CISA ICS advisory Nov 17 for versions 1.45.0 and earlier (water sector); no confirmed compromise.

## 3. Reports and advisories (international, national, large networks)

- **FBI FLASH (Nov 13):** criminals use ARTEX, an open-source AI-driven pentest framework, to automate intrusions (misuse since at least Aug 2026).
- **CERT-In (India) CIAD-2026-0062 (Nov 20):** abuse of business payment workflows and AI-driven offensive frameworks; fraudulent payments were pushed through legitimate approval workflows inside already-compromised environments.
- **Bitdefender Threat Debrief (Nov 2026):** 1,072 claimed ransomware victims in October; N0n Ransomware rising to the top.
- **Law enforcement:** international takedown of servers behind Gootloader, CastleRAT and StealC (timing not confirmed).
- **Trend:** AI-driven exploitation (Cairn, ARTEX) is compressing time from discovery to compromise; MSP/remote-management tools and update infrastructure remain favoured supply-chain entry points.

## 4. Recommendations from agencies and professionals

1. Treat KEV entries as emergency changes; patch vCenter/ESX, SAP NetWeaver, Cisco Expressway, MailAssure and MONAI Label (federal deadlines Nov 23/26).
2. For vCenter, assume possible prior compromise (exploited since Sept): review logs, rotate credentials, restrict management-plane exposure.
3. Per CERT-In: add out-of-band verification and anomaly detection to payment-approval workflows; an approval inside a compromised environment is not proof of legitimacy.
4. Per FBI: assume attackers automate reconnaissance and exploitation; shorten patch windows and monitor for rapid scanning.
5. Lock down MSP/remote-access tooling (ScreenConnect): patch, enforce MFA, audit vendor access and update-channel credentials.
6. Isolate OT/SCADA from the internet and apply CISA ICS mitigations.

## Sources

- [The Hacker News: N-able MailAssure and MONAI Label exploitation](https://thehackernews.com/2026/11/hackers-exploit-n-able-mailassure-flaw.html)
- [CISA KEV: Nov 13](https://www.cisa.gov/news-events/alerts/2026/11/13/cisa-adds-two-known-exploited-vulnerabilities-catalog)
- [CISA KEV: Nov 10](https://www.cisa.gov/news-events/alerts/2026/11/10/cisa-adds-six-known-exploited-vulnerabilities-catalog)
- [CISA KEV: Nov 6](https://www.cisa.gov/news-events/alerts/2026/11/06/cisa-adds-one-known-exploited-vulnerability-catalog)
- [Cryptika: CISA KEV vCenter, SAP, Cisco, Roller](https://www.cryptika.com/cisa-warns-of-vmware-vcenter-sap-netweaver-cisco-expressway-and-apache-roller-flaws-exploited-in-attacks/)
- [Notebookcheck: Cairn AI agents zero-day](https://www.notebookcheck.net/Cairn-AI-agents-took-over-server-via-zero-day-flagged-by-CISA.1458774.0.html)
- [Cybersecurity News: VMware ESX flaws](https://cybersecuritynews.com/vmware-esx-host-vulnerabilities/)
- [Security Boulevard: Daily OT Security News, Nov 18](https://securityboulevard.com/2026/11/daily-ot-security-news-november-18-2026/)
- [Bitdefender Threat Debrief, Nov 2026](https://bitdefender.com/en-us/blog/businessinsights/bitdefender-threat-debrief-november-2026)
- [The Hacker News weekly recap](https://thehackernews.com/2026/11/weekly-recap-sshole-exploited-apt-root.html)
- [Cyber Threat tracker (unverified breaches)](https://cybersecurityventures.com/intrusion-daily-cyber-threat-alert/)
