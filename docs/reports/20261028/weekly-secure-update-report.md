# Weekly IT Security Update — Week Ending 2026-10-28

Coverage: 2026-10-21 to 2026-10-28. Compiled from public news and vendor/agency sources via web search; figures are as first reported and may change. Some counts differ between sources (noted inline).

## 1. Major security incidents

| Incident | Highlights |
|---|---|
| PHP2Shell (PHP) | Two critical PHP bugs allow unauthenticated remote code execution on a very large share of internet-facing servers (reported as ~70%). Found by Calif, coordinated by the PHP Foundation; Amazon saw exploitation about a week before disclosure and working exploits appeared within days. Both CVEs were added to CISA KEV on Oct 26. |
| ConnectWise cloud backup | A China-aligned actor stole credentials via the cloud backup service; Microsoft documented roughly 2,200 compromised Microsoft 365 tenants, with attack rates reported up to 3,000 attempts per minute. ConnectWise shut the backup system down. |
| eMudhra (India CA) | Intermediate CA hacked, private keys exposed (one reportedly trivially weak). Browsers blocked the certificates; eMudhra exited the TLS certificate business. |
| Oracle healthcare unit | Breach reported affecting nearly 20M people (SSNs, addresses, medical details). |
| Capcom | ID service breach tied to 11.7M+ accounts. |
| Japan National Police Agency | About 125,000 officers/staff at 20 prefectural forces possibly exposed via an attack on cloud provider Arcadia. |
| PwC (third-party vendor) | Incident confirmed Oct 23; Everest extortion group claims ~590 GB (claim, unverified). |
| Others | Railway (secrets decrypted), TiDB/Pingcap (FulcrumSec extortion), UpdraftCentral WordPress compromise, Staples support-tool fraud (~$5M gift cards). |

## 2. Major tool and platform security updates

- **Ivanti Connect Secure (CVE-2026-104935):** auth-bypass to command execution, exploited since at least Oct 1 by UTA0576, chained with two other flaws for root access. Fixed in 22.8R2.5, 25.1.3.0, 25.1.3.1, 25.1.1.2. Over 5,600 exposed appliances. Patch, hunt for compromise, reset credentials, revoke certificates, regenerate API keys.
- **Microsoft October Patch Tuesday:** record release (~1,413–1,414 CVEs, 78 critical). One exploited zero-day: Windows Shell Mark-of-the-Web bypass (CVE-2026-96951, CVSS 8.8; CISA due date Oct 16). Also a .NET/WDAC bypass and a MessagePack-CSharp DoS disclosed.
- **CISA KEV additions:** Arista VeloCloud Orchestrator SQL injection (Oct 23); two PHP flaws (Oct 26); earlier Plex Media Server "Plex2Shell" set, Siemens and SiteGuard flaws.
- **Certificate ecosystem:** browser vendors distrusting eMudhra-issued certificates; check legacy and embedded tooling that does not receive CA revocation updates.

## 3. Reports and advisories (international, national, large networks)

- **CISA / FBI / USSS + international partners, #StopRansomware: SafePay (Oct 20):** one of 2026's most active groups, ~600 victims across the Americas, Europe and Australia. Initial access via stolen or guessed credentials, exploited edge/security devices and phishing.
- **Weekly threat reporting (CYFIRMA, others):** continued activity from Qilin, The Gentlemen and SAMURAI-SEC ransomware.
- **Trend:** third-party and supply-chain access (ConnectWise, PwC vendor, Arcadia, Staples support tool) drove many of this week's largest incidents; AI-assisted exploitation speed is increasingly cited.

## 4. Recommendations from agencies and professionals

1. Patch internet-facing systems first: PHP, Ivanti Connect Secure, Arista VeloCloud, Windows. Treat KEV entries as emergency changes.
2. Assume compromise for Ivanti appliances exploited before the patch: forensics, credential and key rotation.
3. Enforce phishing-resistant MFA, unique passwords and credential monitoring (CISA SafePay guidance); keep offline, tested backups.
4. Review MSP and SaaS vendor access, including backup tooling; rotate stored tenant credentials and audit Microsoft 365 sign-ins.
5. Keep secrets out of long-lived environment variables; rotate any exposed API keys (Railway-type incidents).
6. Track CA distrust events and ensure systems can receive trust-store updates.

## Sources

- [This Week in Security, Oct 25 2026](https://this.weekinsecurity.com/this-week-in-security-october-25-2026-edition)
- [CISA: #StopRansomware SafePay (AA26-293A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-293a)
- [CISA KEV additions, Oct 26](https://www.cisa.gov/news-events/alerts/2026/10/26/cisa-adds-two-known-exploited-vulnerabilities-catalog)
- [CISA KEV addition, Oct 23](https://www.cisa.gov/news-events/alerts/2026/10/23/cisa-adds-one-known-exploited-vulnerability-catalog)
- [BleepingComputer: Ivanti zero-day](https://www.bleepingcomputer.com/news/security/cisa-warns-admins-to-immediately-fix-ivanti-flaw-exploited-as-zero-day/)
- [SecurityOnline: October 2026 Patch Tuesday](https://securityonline.info/october-2026-patch-tuesday/)
- [The Hacker News: CISA KEV Plex2Shell, Siemens, SiteGuard](https://thehackernews.com/2026/10/cisa-orders-federal-agencies-to-patch.html)
- [Rescana: PwC vendor incident](https://www.rescana.com/post/pwc-third-party-vendor-data-breach-2026-everest-cyber-extortion-incident-analysis)
- [CYFIRMA weekly intelligence, Oct 23](https://www.cyfirma.com/news/weekly-intelligence-report-23-oct-2026/)
