# Weekly IT Security Update — Week Ending 2026-11-18

Coverage: 2026-11-11 to 2026-11-18. Compiled from public news and vendor/agency sources via web search; figures are as first reported, mostly from secondary roundups, and may change. Confirm key facts against primary sources before acting.

## 1. Major security incidents

| Incident | Highlights |
|---|---|
| Optimizely supply-chain attack | Malicious JavaScript planted in customer projects of the website-testing platform; reportedly affected sites of ~178 organizations (incl. CNN, Zoom, BBC, HSBC). Scripts swapped crypto wallet addresses and could capture card data, cookies and screenshots; iOS exploit kits (Coruna for iOS 13–17, DarkSword for iOS 18) also reported. Discovered by Scott Helme. Count still developing. |
| Infoblox | Security vendor confirmed a breach via a third-party vendor (SharePoint data-migration service); one source says sensitive customer data was taken. Infoblox says its products and build/release processes were not involved. |
| TSMC | Breach at an unnamed cloud provider exposed current/former employee data (hacker advertised ~6.9 GB). TSMC says its systems and customer data were unaffected. |
| Osaka city government | Over 1 million email addresses exposed after a third-party email distribution service was compromised. |
| Maersk | Breach of a legacy system confirmed; WhiteArmor ransomware group claims ~100 GB stolen and demands $5M. |

Theme: third-party / supply-chain exposure (Optimizely, Infoblox, TSMC, Osaka) drove most of the week's incidents.

## 2. Major tool and platform security updates

- **Chromium / Chrome (CVE-2026-125350):** use-after-free in the XML component, exploited in the wild; patched Nov 9, added to CISA KEV Nov 10. Reported as the 8th Chrome zero-day of 2026. Update Chrome/Edge and other Chromium browsers.
- **Microsoft Patch Tuesday (Nov 10):** large release (one source cites 1,097 CVEs, 64 critical — low-confidence source). Outlook flaw CVE-2026-124194 exploited; requires opening a crafted message file and can expose authentication info.
- **Check Point Gaia OS (CVE-2026-123039):** local privilege escalation to root, exploited against about two dozen customers; KEV deadline reportedly Nov 16. Patch now.
- **Barracuda Email Security Gateway:** two bugs added to KEV Nov 10 with an unusually short 3-day deadline.
- **Cisco BroadWorks (CVE-2026-20320), SonicWall SonicOS SSLVPN (CVE-2026-19868), UnrealIRCd (CVE-2026-114713), Openwave (CVE-2026-111793):** added to KEV this week or last.
- **OpenSSH (CVE-2026-123158):** authentication bypass reportedly exploited on Alpine and other musl-based Linux systems.
- **GPUStack:** KEV-listed flaw; upgrade to 2.2.2 and rotate worker and cloud credentials.

## 3. Reports and advisories (international, national, large organizations)

- **ENISA State of Cybersecurity in the Union 2026 (EU, Nov 10):** second NIS2 Art. 18 report covering Jul 2024–Jul 2026. Highlights: AI lowers the cost of finding flaws, so mass exploitation can follow disclosure within days, hours or even minutes; Member States should require critical operators to attest zero-trust baselines, resilient incident response and escalation paths for AI-accelerated incidents. Recommendations are non-binding.
- **ENISA technical advisories:** Secure Update Mechanisms (Nov 4); SBOM (Oct 22, unverified).
- **CISA (US):** KEV additions on Nov 6 (1), Nov 10 (6), Nov 13 (2); ICS advisories incl. Siemens RUGGEDCOM APE1808 (Nov 12) and Mitsubishi Electric CC-Link IE TSN (Nov 5).
- **NCSC (UK):** no new November advisory found; the NCSC/CISA OT secure-connectivity principles (earlier in 2026) remain the reference guidance.

## 4. Recommendations from agencies and professionals

1. **Patch KEV items on emergency timelines** — Chromium, Check Point, Barracuda, Outlook first; treat internet-facing gateways as highest priority.
2. **Assume fast weaponization (ENISA):** shorten patch SLAs and keep exposure inventories current.
3. **Manage third-party risk:** inventory third-party scripts (use CSP and Subresource Integrity), review vendor access to SharePoint/email/cloud tenants, and rotate credentials after vendor incidents.
4. **Zero-trust baselines and AI-incident escalation paths** in incident response plans (ENISA).
5. **OT/ICS:** centralize and tightly control remote connections (NCSC/CISA guidance); apply Siemens and Mitsubishi advisories.
6. **Hunt, don't just patch** for exploited edge devices (Check Point, SonicWall, Barracuda) — patching does not remove prior compromise.

## Sources

- [MacRumors — Optimizely/iPhone website breach](https://www.macrumors.com/2026/11/17/iphone-users-targeted-website-security-breach/)
- [TechCrunch — Infoblox incident](https://techcrunch.com/2026/11/16/cyber-giant-infoblox-confirms-security-incident-after-hackers-access-company-information/)
- [Check Point Research — 16 Nov threat intelligence report](https://research.checkpoint.com/2026/16th-november-threat-intelligence-report/)
- [CISA — KEV additions Nov 13](https://www.cisa.gov/news-events/alerts/2026/11/13/cisa-adds-two-known-exploited-vulnerabilities-catalog)
- [CISA — KEV additions Nov 10](https://www.cisa.gov/news-events/alerts/2026/11/10/cisa-adds-six-known-exploited-vulnerabilities-catalog)
- [CISA — KEV addition Nov 6](https://www.cisa.gov/news-events/alerts/2026/11/06/cisa-adds-one-known-exploited-vulnerability-catalog)
- [ENISA — 2026 State of Cybersecurity in the Union](https://www.enisa.europa.eu/sites/default/files/2026-11/2026%20Report%20on%20the%20State%20of%20Cybersecurity%20in%20the%20Union%20-%20FINAL.pdf)
- [CISA ICS advisories](https://www.cisa.gov/news-events/ics-advisories)
