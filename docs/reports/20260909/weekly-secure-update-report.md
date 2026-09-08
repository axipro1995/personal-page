# Weekly IT Security Update — Week Ending September 9, 2026

**Scope:** Notable security incidents, tool/platform security updates, and international/large-organization security reporting from roughly September 3 – September 9, 2026 (with immediate prior-week context where it directly informs this week's items). Compiled from public vendor advisories, CISA, and security press.

## TL;DR
- **Microsoft ships the largest Patch Tuesday on record — 966 CVEs, 2 exploited zero-days:** The September 8 release fixes CVE-2026-81963 (Windows Update Stack elevation-of-privilege via link-following) and CVE-2026-85880 (Windows ALPC heap-based buffer overflow EoP), both already used in attacks and both added to CISA KEV the same day. Microsoft attributes the volume surge to an AI-powered vulnerability-discovery pipeline; 105 of the fixes are Critical (81 of those RCE).
- **N-able N-central hit by a fourth emergency hotfix in five weeks — CVSS 10.0 pre-auth RCE:** CVE-2026-86218 lets any attacker who can reach the server run code with no credentials. Huntress observed exploitation from September 4, two days before the September 6 fix (N-central 2026.3 Hotfix 4 / build 2026.3.1.14); ~1,500 internet-facing servers were still exposed days later. Added to CISA KEV September 8.
- **Google patches the 6th actively exploited Chrome zero-day of 2026:** CVE-2026-85046, a V8 type-confusion bug, got an emergency desktop update (Chrome 152.0.7977.82/.83). Added to CISA KEV September 4 with a September 18 federal deadline.
- **Trezor supply-chain breach via logistics provider ShipMonk expands to ~80,000 customers:** ShinyHunters exploited an unauthenticated SQL-injection zero-day in ShipMonk's Metabase analytics instance (CVE-2026-72898, CVSS 10.0, password-reset endpoint), minted admin sessions, and bulk-exported order data. Trezor was told on September 2 the breach was far larger than the 13,689 figure disclosed in August; names, emails, phone numbers, and shipping addresses for 2019–2021 orders are exposed, and affected customers are already being targeted with phishing/vishing.
- **SonicWall SMA1000 zero-day chain confirmed exploited:** CVE-2026-83548 (pre-auth SSRF, CVSS 10.0) chained with CVE-2026-83549 (authenticated OS command injection → RCE) was added to CISA KEV September 2 as part of a seven-CVE batch that also covered JFrog Artifactory, Kestra OSS, BerriAI LiteLLM, Starlette, and Sangoma Switchvox — a batch heavy on AI/developer infrastructure, with attackers deploying reverse shells and cryptominers.
- **Broadcom patches critical VMware Workstation/Fusion VM-escape flaws:** CVE-2026-59346 (VMXNET3 integer overflow, CVSS 9.3) and CVE-2026-59347 (Host-Guest File System stack overflow, CVSS 8.1) let an attacker with local admin in a guest execute code on the host. Fixed only in version 26H1u1; no workarounds. No in-the-wild exploitation reported yet.
- **China-nexus "Fire Ant"/UNC3886 caught burrowing into network edge gear:** Sygnia (August 30) detailed a campaign compromising Cisco IOS XR routers, TACACS+ authentication servers, and Linux management hosts — using a `grub-rommon` persistence script, syslog/CLI-output suppression to hide GRE tunnels, and a "BridgeAgent" Linux backdoor masquerading as a Zabbix agent, harvesting TACACS credentials for lateral movement.
- **DOJ and international partners disrupt the 23-year-old Sality botnet:** A September 2 operation with Bulgaria, Hungary, Romania, CrowdStrike, and the Shadowserver Foundation took down infrastructure and poisoned the peer-to-peer peer lists of one of the longest-running malware ecosystems.
- **Unit 42 documents an AI-orchestrated ransomware intrusion compressed into under 10 hours:** Palo Alto's responders describe frontier models and agentic frameworks running reconnaissance, secrets extraction, and CI/CD compromise across ~50 MITRE ATT&CK techniques in a single day — using conventional access paths, not zero-days — and even auto-generating an 80-page assessment of the victim.
- **Regulatory pressure steps up:** The EU Cyber Resilience Act's active-exploitation reporting obligations begin phasing in September 11 (24-hour disclosure to ENISA for in-scope products with EU exposure), and the Five Eyes governments issued their first ministerial-level statement treating frontier AI models as national-security technology.

---

## 1. Major Security Incidents

| Incident | Date | Details |
|---|---|---|
| **Trezor / ShipMonk supply-chain breach — expands to ~80,000 customers** | Trezor notified Sept 2; updates Sept 4–5 | ShinyHunters exploited **CVE-2026-72898**, an unauthenticated SQL injection (CVSS 10.0) in the password-reset endpoint of the Metabase analytics platform run by Trezor's third-party logistics provider ShipMonk, then created admin-level sessions and bulk-downloaded customer order data. Exposed: names, emails, phone numbers, shipping addresses, order numbers for orders placed Nov 2019 – Aug 2021. Follows an August disclosure that covered only 13,689 customers; total now ~80,000+. Trezor says data ShipMonk claimed to have deleted was still present. Customers report follow-on phishing calls. |
| **N-able N-central — pre-auth RCE zero-day exploited before patch** | Exploitation from Sept 4; hotfix Sept 6 | **CVE-2026-86218** (CVSS 10.0) allows unauthenticated remote code execution on the N-central RMM server. Huntress observed attacks against the underlying API and appliance logs starting Sept 4; N-able's direct-to-customer notice called it a zero-day "observed being exploited in the wild" (its public statement is more hedged). Patched in N-central 2026.3 Hotfix 4 (build 2026.3.1.14) — the fourth N-central hotfix in five weeks. ~1,500 internet-facing servers remained unpatched days after release. CISA KEV Sept 8. |
| **Fire Ant / UNC3886 — China-nexus espionage on network edge infrastructure** | Sygnia disclosure Aug 30 (ongoing) | Campaign compromising Cisco IOS XR routers, TACACS+ auth servers, and Linux management hosts. Persistence via an `/etc/rc.d/init.d/grub-rommon` script disguised as a system process; defense evasion by suppressing tunnel-related strings from syslog and altering router CLI output to hide GRE tunnel config; "BridgeAgent" Linux backdoor posing as a Zabbix agent / GNOME shell component; packet-capture exfiltration to external FTP plus TACACS credential harvesting. Strong overlap with Mandiant's UNC3886. |
| **Unit 42 — AI-orchestrated ransomware intrusion (unnamed enterprise)** | Disclosed early Sept 2026 | Palo Alto Networks incident responders describe an intrusion in which frontier AI models and agentic attack frameworks compressed a typically ~2-week operation into under 10 hours, automating reconnaissance, secrets extraction, and CI/CD pipeline compromise across ~50 MITRE ATT&CK techniques, and generating an 80-page assessment of the victim environment. The attack used conventional access paths, not zero-days. |
| **DOJ + international takedown of the Sality botnet** | Sept 2 | Coordinated action with Bulgaria, Hungary, Romania, CrowdStrike, and the Shadowserver Foundation disrupted infrastructure and manipulated the peer-to-peer peer lists of the Sality malware ecosystem, active for roughly 23 years and historically used for spam, credential theft, and secondary-payload delivery. |
| **North Korea-aligned Linux espionage toolkit with HAProxy backdoor** | Reported ~Sept 7 | Researchers detailed a stealthy Linux toolkit used by a DPRK-aligned actor that embeds a custom backdoor directly into HAProxy 2.8.12, blending malicious traffic handling into a legitimate load-balancer process on compromised hosts. |
| **Coder registry compromise → malicious Terraform modules** | Incident Aug 31 (reported this week) | Per CISA/Cloud Security Alliance briefing: an attacker compromised a Cloudflare API key and rerouted Coder registry traffic for ~14 hours, serving malicious Terraform modules that harvested cloud credentials, CI/CD secrets, and AI-provider keys. Reported to affect AI development pipelines at several large organizations. Terraform's lock file does not hash-verify remote modules, masking the substitution. *(Single-source aggregator report — treat scope/victim claims as unconfirmed.)* |
| **Boston Scientific — continuing recovery** | Detected Aug 25; recovery through early Sept | Carry-forward: the incident that took manufacturing, order processing, and shipping systems offline is still in recovery. CrowdStrike-assisted investigation continues to find no unauthorized activity since Aug 25; cloud systems and active implanted cardiac devices unaffected; some remote-activation workflows disrupted. |

## 2. Major Tool / Platform Security Updates

**⚠️ Actively exploited (CISA KEV / confirmed in-the-wild):** Microsoft Windows (2 zero-days), N-able N-central, Google Chrome V8, SonicWall SMA1000, Adobe Commerce/Magento, plus the Sept 2 KEV batch (JFrog Artifactory, Kestra OSS, LiteLLM, Starlette, Sangoma Switchvox), and continuing PaperCut NG/MF exploitation in the education sector. **⚠️ High-priority, no workaround:** VMware Workstation/Fusion.

| Product | Issue | CVE(s) | Severity | Status / Action |
|---|---|---|---|---|
| Microsoft Windows | Windows Update Stack EoP via improper link resolution (link following) | CVE-2026-81963 | Important, actively exploited | Sept 2026 Patch Tuesday. Added to CISA KEV Sept 8. Deploy immediately — chained with an RCE or phishing foothold this yields local privilege escalation to SYSTEM. Credited to Romain Deperne / MSTIC. |
| Microsoft Windows | Windows ALPC heap-based buffer overflow EoP | CVE-2026-85880 | Important, actively exploited | Sept 2026 Patch Tuesday. Added to CISA KEV Sept 8. Reported by Volexity and Proofpoint researchers — reporter profile suggests use in targeted intrusions. |
| Microsoft (portfolio) | Record 966 CVEs total — 105 Critical (81 RCE, 20 EoP), 258 RCE overall, 438 EoP overall. Spans Windows, Office/SharePoint, Exchange, SQL Server, Azure (AI Language, Cosmos DB, AD B2C, HDInsight), Teams, Dynamics 365, Edge. | — | Multiple Critical | Prioritize the two exploited zero-days, then internet-facing Exchange/SharePoint/SQL and any Critical RCE reachable from untrusted networks. Volume driven by Microsoft's AI-assisted discovery — expect elevated monthly counts to continue. |
| N-able N-central | Unauthenticated remote code execution | CVE-2026-86218 | Critical, CVSS 10.0, actively exploited | Upgrade to 2026.3 Hotfix 4 (build 2026.3.1.14) immediately; N-central should never be internet-exposed — restrict to a management network/VPN. Hunt appliance logs and API access back to at least Sept 4. CISA KEV Sept 8. |
| Google Chrome (V8) | Type confusion in the V8 JavaScript engine | CVE-2026-85046 | High, actively exploited (6th Chrome zero-day of 2026) | Emergency update to Chrome 152.0.7977.82/.83 (Windows/macOS), 152.0.7977.82 (Linux). Force browser restarts fleet-wide. CISA KEV Sept 4, federal deadline Sept 18. |
| SonicWall SMA1000 | Pre-auth SSRF chained with authenticated OS command injection → unauthenticated RCE | CVE-2026-83548 (CVSS 10.0), CVE-2026-83549 | Critical, actively exploited | Hotfixes released; CISA KEV Sept 2, federal deadline Sept 5. SMA1000 is EoL-track — plan migration. Audit for reverse shells and unexpected admin tokens/accounts. |
| Adobe Commerce / Magento | Improper neutralization of special elements in a template engine → code execution | CVE-2026-75650 | Critical, actively exploited | Added to CISA KEV Sept 8. Apply Adobe's security update; e-commerce storefronts are high-value targets for card-skimming (Magecart-style) persistence. |
| VMware Workstation / Fusion | VMXNET3 integer overflow (VM escape); Host-Guest File System stack buffer overflow | CVE-2026-59346 (CVSS 9.3), CVE-2026-59347 (CVSS 8.1) | Critical | Fixed only in **26H1u1** (affects Workstation/Fusion 25H2 and 26H1). No workarounds. Attacker needs local admin in a guest with a VMXNET3 adapter — patch any host running untrusted or internet-exposed VMs first. No exploitation reported yet. |
| MikroTik RouterOS | Two chained flaws enabling unauthenticated SSH auth bypass → router takeover ("MikroTrick") | CVE-2026-67276, CVE-2026-86060 | Critical | Patches in RouterOS 6.49.21, 7.23.4, 7.24.2. Exploitation reportedly began ~3 days before disclosure. Restrict SSH/management to trusted sources; MikroTik edge devices are routinely conscripted into DDoS botnets. *(Single-source aggregator report — confirm CVE IDs against MikroTik's advisory.)* |
| Kestra OSS / BerriAI LiteLLM / Starlette / Sangoma Switchvox / JFrog Artifactory | Sept 2 CISA KEV batch — command injection / auth bypass / SQL injection across workflow-orchestration and AI-gateway tooling; attackers deployed reverse shells, minted admin tokens, and ran XMRig miners | CVE-2026-49869 (CVSS 10.0), CVE-2026-59822, CVE-2026-48710, CVE-2026-9586, CVE-2026-82329 (CVSS 9.8) | Critical/High, actively exploited | Federal patch deadline Sept 5 for most, Sept 16 for the Starlette/LiteLLM pair. JFrog Artifactory (carry-forward from last week) is still the highest-priority of the set for most enterprises — audit for unauthorized admin tokens even after patching. |
| PaperCut NG / MF | Continued exploitation of the Aug auth-bypass + unsafe-reflection RCE chain, now focused on the education sector in the US and Europe | CVE-2026-81578, CVE-2026-82078 | Critical, actively exploited | Carry-forward: confirm emergency patches (versions 24/25/26) are applied and Application Servers are not internet-facing; hunt logs back to Aug 26. |

## 3. Security Reports — International / Large-Organization Highlights

**EU Cyber Resilience Act — active-exploitation reporting phase-in (from September 11):**
- Manufacturers of "products with digital elements" that have EU exposure must report actively exploited vulnerabilities to ENISA within 24 hours (early warning), with follow-up notifications thereafter. This is the first hard obligation to bite under the CRA ahead of the full December 2027 conformity regime — vendors need an incident pipeline that can hit a 24-hour clock now.

**Five Eyes — first ministerial-level statement on frontier AI as national-security technology:**
- Australia, Canada, New Zealand, the UK, and the US formalized oversight commitments and pledged to "deepen collaboration with industry" on "timely access to frontier models." No concrete scrutiny criteria were published, and analysts flag that divergence between jurisdictions on model access/scrutiny is itself becoming a supply-chain and compliance risk.

**Toshiba Cyber Security Report 2026 (English edition, August 31):**
- Emphasis on threats increasingly targeting control systems and industrial equipment; covers ransomware, targeted attacks, and supply-chain risk. Consistent with a broader OT-focus shift seen across vendor reporting this year.

**Waterfall Security — Threat Report 2026 (referenced this period):**
- Finds a ransomware slowdown against industrial/OT targets masking a deeper shift toward nation-state operations against critical infrastructure. Reinforces the case for consequence-based engineering and hard IT/OT segmentation rather than relying on ransomware-volume trends as a risk barometer.

**Black Kite — 2026 Ransomware Report (standing research, referenced this period):**
- 7,551 publicly disclosed ransomware victims tracked over the prior 12 months (+24.9% YoY, fourth consecutive record year); 146 active groups as of June 2026. Notable structural finding: a majority of victims had unpatched medium/high vulnerabilities *and* misconfigured DMARC records at the time of disclosure — i.e. the fundamentals were already failing.

**Google / Mandiant — M-Trends 2026 (standing research):**
- Grounded in 500,000+ hours of 2025 frontline investigations; the recurring headline is that the large majority of successful intrusions still stem from fundamental human and systemic failures (unpatched exposure, credential theft, missing MFA) rather than novel technique.

## 4. Recommendations from Professionals / Organizations

**On the record Patch Tuesday (Microsoft, 966 CVEs):**
- **Consensus across patch-management analysts (Action1, Tenable/ZDI commentary, BleepingComputer):** raw CVE count and CVSS are the wrong triage signal at this volume — sequence by *confirmed exploitation* first (CVE-2026-81963 and CVE-2026-85880), then Critical RCE reachable from untrusted networks, then everything else. Expect Microsoft's AI-assisted discovery to keep monthly totals elevated, so build the triage muscle now rather than treating September as an anomaly.
- Treat the two zero-days as privilege-escalation building blocks: they matter most when paired with an existing foothold, so pair patching with EDR hunting for the corresponding exploitation artifacts.

**On internet-facing management planes (N-able, MikroTik, SonicWall):**
- **Huntress (N-able N-central):** patching is necessary but not sufficient — hunt the appliance API logs and system logs back to at least September 4 for signs of pre-patch compromise, and rotate credentials/secrets the RMM had access to.
- **General guidance:** RMM consoles, router SSH, and VPN/SMA admin interfaces should not be reachable from the open internet — put them behind a management VLAN or VPN, enforce MFA, and alert on new admin accounts/tokens. These three products were all exploited *before* their public advisories this week.

**On the Trezor/ShipMonk supply-chain breach (ShinyHunters, Metabase zero-day):**
- **Trezor's own response and analyst commentary:** inventory exactly which third-party processors hold your customer/PII data and what analytics/BI tooling they run it through (Metabase, Snowflake, Databricks have all been entry points in ShinyHunters campaigns this year); demand contractual breach-notification SLAs and proof of deletion; and pre-stage customer comms because exposed contact data drives immediate phishing/vishing against your users.

**On VM-escape flaws (Broadcom/VMware Workstation & Fusion):**
- **Broadcom advisory guidance:** with no workarounds available, upgrade to 26H1u1. Prioritize hosts running untrusted, multi-tenant, or internet-exposed guests; where a guest is untrusted, do not treat the VM boundary as a hard security boundary.

**On AI-accelerated intrusions (Unit 42; CyberInfos threat analysis):**
- Detection playbooks built on multi-day dwell-time assumptions need revisiting — an unusually short interval between initial access and mass exfiltration can itself be a signal of automated/agentic orchestration. Invest in fast, automated credential revocation and containment, because the human response window is shrinking.

**On critical infrastructure / OT (Waterfall, Toshiba):**
- Don't read the ransomware slowdown against OT as reduced risk — segment OT from IT, apply consequence-based engineering to the highest-impact processes, and assume nation-state interest in the network edge (see Fire Ant/UNC3886) rather than just the endpoints.

**On regulatory readiness (EU CRA):**
- In-scope manufacturers need a working 24-hour exploitation-reporting path to ENISA operational by September 11 — this includes a triage process to decide "actively exploited" quickly and a designated reporting owner.

---

## Caveats
- Compiled from public vendor advisories, CISA alerts, and security-press aggregation (The Hacker News, BleepingComputer, SecurityWeek, Help Net Security, Infosecurity Magazine, Cloud Security Alliance briefings, and weekly-roundup sites) rather than direct primary-source verification of every claim; confirm exact scope, CVSS scores, CVE IDs, and patch status against vendor advisories before acting.
- Several items lean on a single aggregator or threat-actor claim and are flagged inline as such — notably the Coder registry compromise, the MikroTik "MikroTrick" CVE IDs, the "llms.txt" package-installation research, the Dropbox/Lenovo-ID SSO account-compromise reports, and the Aesto Health breach figure. Treat victim counts and scope for these as unconfirmed.
- N-able's public statement on CVE-2026-86218 ("no evidence of exploitation in production") conflicts with its direct-to-customer notice ("observed being exploited in the wild"); Huntress reports exploitation from September 4. The report follows the exploitation timeline.
- Microsoft's CVE total is reported as 966 by BleepingComputer and 973–995 by other outlets depending on whether Edge/Chromium and republished CVEs are counted; the exact figure varies by methodology.
- The Black Kite, Waterfall, Toshiba, and M-Trends reports are standing/periodic research referenced this period rather than newly published in this exact week.
- The Boston Scientific and Fire Ant/UNC3886 situations were still active/unresolved as of this report's compilation; scope may shift.

## Sources
- [BleepingComputer — Microsoft September 2026 Patch Tuesday fixes 966 flaws, 2 zero-days](https://www.bleepingcomputer.com/news/microsoft/microsoft-september-2026-patch-tuesday-fixes-966-flaws-2-zero-days/)
- [CISA — Adds Four Known Exploited Vulnerabilities to Catalog (Sept 8, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/08/cisa-adds-four-known-exploited-vulnerabilities-catalog)
- [CISA — Adds One Known Exploited Vulnerability to Catalog (Sept 4, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/04/cisa-adds-one-known-exploited-vulnerability-catalog)
- [CISA — Adds Seven Known Exploited Vulnerabilities to Catalog (Sept 2, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/02/cisa-adds-seven-known-exploited-vulnerabilities-catalog)
- [The Hacker News — CISA Adds Seven Exploited Flaws as Attackers Deploy Reverse Shells and Crypto Miners](https://thehackernews.com/2026/09/cisa-adds-seven-exploited-flaws-as.html)
- [Help Net Security — N-able patches critical N-central zero-day exploited in the wild (CVE-2026-86218)](https://www.helpnetsecurity.com/2026/09/07/n-able-n-central-hotfix-cve-2026-86218/)
- [The Hacker News — N-able Issues Fourth N-central Hotfix in Five Weeks for Unauthenticated RCE Flaw](https://thehackernews.com/2026/09/n-able-issues-fourth-n-central-hotfix.html)
- [SecurityWeek — N-able Patches Critical Zero-Day in N-central](https://www.securityweek.com/n-able-patches-critical-zero-day-in-n-central/)
- [Huntress — Critical N-able N-central Vulnerability and Active Exploitation](https://www.huntress.com/blog/n-able-vulnerability-exploitation)
- [The Hacker News — Trezor Says ShipMonk Breach Exposed 67,000 U.S. Customers' Data It Said Was Deleted](https://thehackernews.com/2026/09/trezor-says-shipmonk-breach-exposed.html)
- [Cybersecurity News — Trezor Confirms ShipMonk Data Breach Exposed 67,000 Additional US Customers](https://cybersecuritynews.com/trezor-confirms-shipmonk-data-breach/)
- [Rescana — Trezor ShipMonk Breach Exposes 67,000 U.S. Customer Records via Metabase Zero-Day (CVE-2026-72898)](https://www.rescana.com/post/trezor-shipmonk-breach-exposes-67-000-u-s-customer-records-via-metabase-zero-day-vulnerability-cve-2026-72898)
- [The Hacker News — Critical VMware Workstation and Fusion Flaw Lets VM Admins Execute Host Code](https://thehackernews.com/2026/09/critical-vmware-workstation-and-fusion.html)
- [SecurityOnline — CVE-2026-59346 (CVSS 9.3): VMware Flaw Allows Running Code on the Host](https://securityonline.info/vmware-cve-2026-59346-code-execution-host/)
- [CyberInfos — Weekly Cybersecurity Report: August 31 – September 6, 2026](https://www.cyberinfos.in/weekly-cybersecurity-report-aug-31-sept-6-2026/)
- [Boston Institute of Analytics — Cybersecurity This Week: August 29 – September 4, 2026](https://bostoninstituteofanalytics.org/blog/cybersecurity-this-week-august-29-september-4-2026-major-attacks-zero-days-data-breaches-and-ai-security/)
- [Cybersecurity News — Weekly Cybersecurity Newsletter Bulletin (Sept 7, 2026)](https://cybersecuritynews.com/weekly-cybersecurity-newsletter-bulletin-sept-2026/)
- [Cloud Security Alliance — CISO Daily Briefing, September 7, 2026](https://labs.cloudsecurityalliance.org/research/ciso-daily-briefing-20260907/)
- [Industrial Cyber — Waterfall Threat Report 2026 finds ransomware slowdown masks shift toward nation-state attacks on critical infrastructure](https://industrialcyber.co/reports/waterfall-threat-report-2026-finds-ransomware-slowdown-masks-deeper-shift-toward-nation-state-attacks-on-critical-infrastructure/)
- [Google Cloud — M-Trends 2026: Data, Insights, and Strategies From the Frontlines](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026)
- [Black Kite — 2026 Ransomware Report](https://blackkite.com/reports/2026-ransomware-report)
- [Action1 — Patch Tuesday September 2026](https://www.action1.com/patch-tuesday/patch-tuesday-september-2026/)
