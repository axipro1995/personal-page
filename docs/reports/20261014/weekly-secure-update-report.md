# Weekly IT Security Update — Week Ending 2026-10-14

Coverage: 2026-10-07 to 2026-10-14. Compiled from public news and vendor/agency sources via web search; figures are as first reported and may change. Items marked "(unverified)" came from a single or aggregator source.

## 1. Major security incidents

| Incident | Highlights |
|---|---|
| Oracle healthcare unit | Breach reported to affect nearly 20M people in the US; SSNs, home addresses and medical details compromised. |
| Pentagon / Defense Manpower Data Center | 3M+ individuals affected; names, SSNs, birth dates, contact and military occupational data exposed. |
| Times Car (Japan) | 6.6M accounts affected; identity-verification documents leaked for 1.6M users. |
| ASOS (UK retailer) | Customer names, addresses, phone numbers, emails and dates of birth exposed after attackers impersonated a trusted contact to reach an employee account. |
| iRhythm | At least 360K patients affected (names, addresses, emails, dates of birth, phone numbers). |
| "FortiBleed" campaign | FBI and Secret Service warned of credential harvesting that compromised tens of thousands of FortiGate devices; attackers sometimes deleted or changed original admin accounts, and access is linked to ransomware affiliates. |

Ransomware trend: one tracker reported global leak-site publications up 33.5% week over week (271 confirmed this week) (unverified).

## 2. Major tool and platform security updates

- **Microsoft October Patch Tuesday (Oct 13)**: reported as the largest release ever (aggregator figure of roughly 1,400 CVEs; verify against Microsoft's MSRC guide). CVE-2026-96951, a Windows Shell security feature bypass, is exploited in the wild. Prioritise it.
- **Citrix NetScaler ADC/Gateway**: critical zero-days CVE-2026-88771, CVE-2026-88772 and CVE-2026-88779 are in CISA's KEV catalog (added Sep 27 and Oct 4). Patch if not already done.
- **Flax Typhoon-exploited flaws**: five were added to KEV; the federal remediation deadline was Oct 11, 2026. Treat as overdue if still unpatched.
- **CISA KEV**: 8 CVEs added in the last 7 days (as of Oct 9). Check the catalog against your asset inventory.
- **Fortinet FortiGate**: given FortiBleed, audit admin accounts for unexpected changes or deletions and rotate credentials.

## 3. Reports and advisories (international, national, large organisations)

- **CISA, FBI, NSA and international partners (Oct 8)**: advisory on Chinese state-linked activity using large-scale botnets, VPN infrastructure and living-off-the-land techniques against US and global networks.
- **FBI / Secret Service**: joint advisory on the FortiGate credential-harvesting campaign above.
- **ENISA Threat Landscape 2026** (covers Jan–Dec 2025): ransomware remains the most impactful incident type in the short term; hacktivist-led DDoS targets essential entities; public administration is the most targeted sector; AI is expected to be used more in malicious operations.
- **CISA ICS/OT**: earlier 2026 advisory on Iranian-affiliated actors exploiting PLCs across US critical infrastructure remains relevant to OT operators.

## 4. Recommendations from agencies and professionals

1. Patch internet-facing edge devices first (Citrix, Fortinet), and use KEV as the priority list.
2. Audit and rotate admin credentials on firewalls and VPNs; enforce phishing-resistant MFA.
3. Hunt for living-off-the-land activity; baseline admin tool use and monitor VPN and botnet-like traffic.
4. Defend against help-desk and trusted-contact impersonation (the ASOS pattern) with out-of-band verification.
5. Minimise sensitive data retention (SSNs, ID documents) to shrink breach impact.
6. Keep offline, tested backups and rehearse ransomware response.

## Sources

- [Cybersecurity Week in Review: Oct 5–11, 2026](https://blog.senthorus.ch/posts/weekly_reviews/12_10_2026)
- [FortiBleed, AI bank cyberattacks, ASOS breach](https://dmarcreport.com/blog/fortibleed-firewall-attacks-ai-bank-cyberattacks-asos-data-breach)
- [Data breaches in October 2026 (Breachsense)](https://www.breachsense.com/breaches/2026/october/)
- [Ransom-DB weekly surge](https://ransom-db.com/blog/ransomware-weekly-surge-october-2026-threat-intelligence)
- [Microsoft October 2026 Patch Tuesday](https://www.ntcompatible.com/story/microsoft-october-2026-patch-tuesday-largest-release-ever-ships-live-windows-shell-zeroday)
- [Flax Typhoon exploits five flaws (The Hacker News)](https://thehackernews.com/2026/10/flax-typhoon-exploits-five-flaws-as.html)
- [CISA: Citrix NetScaler zero-days](https://www.cisa.gov/news-events/alerts/2026/09/27/critical-zero-day-vulnerabilities-exploited-citrix-netscaler-adc-gateway)
- [CISA KEV additions this week](https://senserva.com/exploited-this-week.html)
- [CISA advisories](https://www.cisa.gov/news-events/cybersecurity-advisories)
- [ENISA Threat Landscape](https://www.enisa.europa.eu/topics/cyber-threats/threat-landscape)
