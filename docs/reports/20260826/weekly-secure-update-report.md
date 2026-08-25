# Weekly IT Security Update — Week Ending August 26, 2026

**Scope:** Notable security incidents, tool/platform security updates, and international/large-organization security reporting from roughly August 19–26, 2026 (with immediate prior-week context where it directly informs this week's items). Compiled from public vendor advisories, CISA, and security press.

## TL;DR
- **Cl0p's Windchill/FlexPLM extortion wave widens:** Shell confirmed it is investigating a claimed **89GB data theft** by Cl0p, which now lists Shell, GE, and Philips among roughly **50 organizations** hit via mass exploitation of PTC Windchill/FlexPLM (CVE-2026-12569, CVSS 9.3) — the same webshell campaign flagged in prior weeks, now reaching household-name victims.
- **Azure credential-theft campaign hits Fortune 500 tenants:** A dark-web seller ("TheHatman") is selling internal directory data pulled from compromised Azure/Entra tenants at **McDonald's (1.7M records), TCS (~800K), Vodafone (~425K), and HCL (~250K)**, plus IHG, Gap, and Wyndham — traced to infostealer-compromised credentials rather than an Azure zero-day.
- **T-Mobile's "cut the cable" Salt Typhoon eviction goes public:** CSO Jeff Simon revealed that in 2024, T-Mobile's security team physically severed a network cable with scissors to instantly evict China-linked Salt Typhoon operators from a compromised server — avoiding the risk that a software-based kickout would tip off the attackers. The FBI says Salt Typhoon hit 200+ companies.
- **DOJ charges 17 IRGC-linked hackers:** A 14-count superseding indictment (Aug 21) accuses Iran's Mabna Institute of a decade-long campaign stealing **31.5TB of research data (~$3.4B in IP)** from 144 U.S. and 178 foreign universities and 42 private companies; DOJ posted $10M bounties for five still-at-large defendants.
- **Critical GitLab flaw exploited within minutes of disclosure:** CVE-2026-19478 (CVSS 9.4), an unauthenticated GraphQL code-injection bug letting attackers delete public projects and forge merge history, got an Aug 17 out-of-band GitLab patch — watchTowr reproduced working exploits almost immediately, and in-the-wild attempts followed fast.
- **Stripe merchant keys dumped for free:** A forum seller published **659 live Stripe secret keys** (650 `sk_live`) spanning **688,000 customer records across 42 countries**, harvested from compromised merchant integrations rather than any Stripe breach itself.
- **npm supply-chain: AI-powered Linux backdoor:** 14 trojanized npm packages disguised as calendar/streak utilities silently deployed **RedC2 4.0**, a cross-platform implant with an AI-driven "Red Agent" that lets operators run post-exploitation tasks via natural-language commands — no install hooks needed, so `--ignore-scripts` doesn't block it.
- **9,300+ AWS keys still live after years of exposure:** Truffle Security found over 9,300 publicly leaked AWS access keys (from GitHub, Hugging Face, Docker images, CI logs) collected between 2022–2026 still authenticate, **88% of them active**, including 526 root keys.
- **Shadow AI called out as the top enterprise exposure:** Akamai's *Enterprise AI Usage Risk Report 2026* found nearly half of enterprise AI sessions run on unmanaged personal accounts, with the top 5% of "power users" driving 12x the AI interaction of the bottom half of the workforce — while ~75% of AI browser extensions request high/critical permissions and 16.3% carry known vulnerabilities.

---

## 1. Major Security Incidents

| Incident | Date | Details |
|---|---|---|
| **Shell / Cl0p — PTC Windchill & FlexPLM extortion wave** | Claimed on Cl0p's leak site mid-Aug, escalated this period | Cl0p claims theft of ~89GB of Shell's engineering drawings, facility photos, project roadmaps, and testing reports, via the ongoing CVE-2026-12569 (CVSS 9.3) unauthenticated RCE chain in PTC Windchill/FlexPLM (pre-auth WSDL info-disclosure + login-servlet deserialization → hex-named JSP webshells). Shell is investigating without confirming a breach; no evidence yet that the claimed data is circulating. GE and Philips are also named, with the campaign now touching roughly 50 organizations across manufacturing, automotive, aerospace, and retail. |
| **Azure/Entra credential-theft data-selling campaign** | Ongoing, escalated ~Aug 18–21 | A seller ("TheHatman") is offering internal employee-directory data allegedly pulled from Azure/Entra tenants at McDonald's (1.7M+ records), TCS (~800K), Vodafone (~425K), HCL (~250K), plus IHG, Gap, Hexaware, and Wyndham. Hudson Rock traced the access to infostealer-compromised credentials, not an Azure platform flaw. Vodafone says the exposed data is old, "business card"–level information; other victims are still assessing. |
| **T-Mobile's physical Salt Typhoon eviction disclosed** | Incident occurred 2024; publicly disclosed Aug 18, 2026 | T-Mobile CSO Jeff Simon revealed that a security team physically drove to a Bellevue, WA data center and cut a network cable with scissors to instantly sever a server compromised by Chinese state-linked Salt Typhoon, rather than risk a software-based eviction alerting the attackers. The FBI attributes 200+ telecom/government compromises to the same campaign. The cut cable is now displayed at T-Mobile HQ. |
| **DOJ indicts 17 Iranian IRGC-linked hackers (Mabna Institute)** | Indictment unsealed Aug 21, 2026 | A 14-count superseding indictment accuses the group of a cyber-espionage campaign (active ~2013–2017, still being prosecuted) that stole at least 31.5TB of research data/IP (~$3.4B) from 144 U.S. and 178 foreign universities plus 42 U.S. private companies, compromising ~8,000 academic email accounts. Eight defendants are newly charged; DOJ is offering up to $10M per lead on five still-at-large defendants. |
| **Stripe merchant API key leak** | Dataset posted Aug 18, 2026 | A forum seller ("Satanic") published 659 live Stripe merchant API keys (650 `sk_live` secret keys) and ~35GB of pulled customer/payment data — 688,363 customer records across 42 countries, largest concentrations in the U.S. (212 accounts), UK (81), and France (57). Stripe's own systems were not breached; keys were harvested from individual merchants' exposed integrations. |
| **GitLab CVE-2026-19478 exploited within minutes of disclosure** | Emergency patch Aug 17, 2026; exploitation attempts immediate | Unauthenticated GraphQL code-injection flaw (CVSS 9.4) in self-managed GitLab CE/EE lets attackers modify/delete public projects, forge merge records to fake a fix landing, and ban maintainers. watchTowr reproduced a working exploit almost immediately after disclosure. Fixed in 19.2.4, 19.1.6, 19.0.8, 18.11.11; GitLab.com/Dedicated already patched. |
| **npm supply-chain: RedC2 4.0 AI-powered Linux backdoor** | Disclosed ~Aug 21, 2026 | 14 trojanized npm packages (e.g., `streak-metrics-math`, `kit-map-vim`, `map-streak-kit`) posed as calendar/streak utilities but silently loaded RedC2 4.0, a cross-platform implant sold on cybercrime forums with an AI-driven "Red Agent" for natural-language-orchestrated post-exploitation. No install hooks are used, so `--ignore-scripts` doesn't prevent execution. |

## 2. Major Tool / Platform Security Updates

**⚠️ Actively exploited (CISA KEV):** Windows IKE Service, SharePoint (weak auth), VMware vCenter, Apple macOS, TrueConf Server, GitLab, PTC Windchill/FlexPLM. **⚠️ Corrected exploitation status:** Microsoft Entra ID (initially flagged exploited, later confirmed not exploited).

| Product | Issue | CVE(s) | Severity | Status / Action |
|---|---|---|---|---|
| Microsoft Entra ID | Deserialization of untrusted data → unauthenticated remote code execution in Microsoft's cloud identity service | CVE-2026-69836 | Critical, CVSS 10.0 | Fully mitigated server-side by Microsoft on Aug 21; no customer action required. Advisory briefly listed exploitation as confirmed, then corrected to "not exploited in the wild." |
| GitLab (self-managed CE/EE) | Unauthenticated GraphQL directive code injection — delete/modify public projects, forge merge records | CVE-2026-19478 | Critical, CVSS 9.4 | Out-of-band patch Aug 17; upgrade to 19.2.4 / 19.1.6 / 19.0.8 / 18.11.11 immediately — exploitation attempts observed within minutes of disclosure. |
| Microsoft Windows IKE Service Extensions | Double-free vulnerability | CVE-2026-33824 | High, actively exploited (KEV Aug 18) | Patch via Windows Update; added to CISA KEV with confirmed in-the-wild exploitation. |
| Microsoft SharePoint | Weak authentication vulnerability | CVE-2026-55040 | High, actively exploited (KEV Aug 18) | Added to CISA KEV Aug 18; patch on-prem SharePoint servers immediately given SharePoint's history as a high-value target this year. |
| Apple macOS | Improper authentication vulnerability | CVE-2026-65400 | High, actively exploited (KEV Aug 18) | Added to CISA KEV Aug 18; update macOS to the latest release. |
| TrueConf Server | Missing authentication for critical function + code injection allowing sandbox escape → arbitrary OS command execution | CVE-2026-72529, CVE-2026-72530 | Critical, actively exploited (KEV Aug 20) | Exploited by the Head Mare group since ≥July 2026 to replace client installers with backdoored versions. CISA gave federal agencies a 3-day patch window; upgrade to TrueConf Server 5.3.9 / 5.4.9 / 5.5.5. |
| PTC Windchill / FlexPLM | Pre-auth info disclosure chained with deserialization RCE; JSP webshells dropped | CVE-2026-12569 | Critical, CVSS 9.3, actively exploited (KEV since June) | Cl0p-affiliated actors continue exploiting unpatched internet-exposed instances this period (Shell, GE, Philips); patch immediately and hunt for hex-named JSP webshells under `/Windchill/login/`. |
| npm registry (14 packages) | Trojanized "calendar/streak" utility packages loading RedC2 4.0 implant with no install hooks | — | High (supply chain) | Packages have been reported/pulled; audit dependency trees for the named packages, and note that `--ignore-scripts` does not block this loader technique. |

## 3. Security Reports — International / Large-Organization Highlights

**Akamai — State of the Internet: Enterprise AI Usage Risk Report 2026:**
- Nearly **half of all enterprise AI conversations** happen through unmanaged personal identities — a "shadow AI" blind spot security teams aren't tracking.
- The top **5% of AI "power users"** interact with AI models at **12x** the rate of the bottom 50% of the workforce, concentrating disproportionate data-leakage and autonomous-agent risk in a small group.
- Identifies three 2026 AI-native attack vectors: **Vibe hacking, CursorJacking, and CometJacking**; ~75% of AI browser extensions request high/critical permissions and **16.3% contain known vulnerabilities**.

**Truffle Security — exposed AWS keys research:**
- **9,300+ publicly leaked AWS access keys** (out of 64,024 unique key pairs found across GitHub, Hugging Face datasets, Docker images, and CI logs, 2022–2026) are still active — **88%** of complete credential pairs still authenticate, including **526 root keys**.
- **90.5%** of accounts tied to leaked keys have no spend/budget alert configured; 43% of exposed secrets appear more than once across different repos/datasets.

**CISA / NSA / FBI / DC3 / international partners — Siemens S7 PLC advisory (Aug 20):**
- Warns of active threat activity, including AI-generated scripts, targeting Siemens S7-series PLCs across critical manufacturing, energy, water/wastewater, chemical, and food & agriculture sectors.

## 4. Recommendations from Professionals / Organizations

**On this week's active exploitation chains:**
- **CISA (TrueConf, GitLab, PTC Windchill):** treat CISA KEV listing as an immediate action trigger, not a queue item — CISA gave federal agencies just a 3-day window on TrueConf given confirmed Head Mare exploitation.
- **watchTowr / GitLab:** for internet-facing GitLab self-managed instances, patch on the out-of-band schedule rather than waiting for the next regular release — working exploits appeared within minutes of the advisory going public.
- **Incident responders (Windchill/FlexPLM):** patching CVE-2026-12569 is not sufficient on its own — actively hunt for hex-named JSP webshells already dropped under `/Windchill/login/` on internet-exposed instances, since Cl0p affiliates have had a multi-month head start.

**On T-Mobile's Salt Typhoon disclosure:**
- **T-Mobile CSO Jeff Simon:** when a nation-state actor is confirmed inside a system, consider physical/out-of-band isolation over a software-based kickout — a network command can be logged or intercepted by the attacker, while physically cutting a connection removes any window to react or dig in deeper.

**On credential and secret hygiene:**
- **Hudson Rock / incident analysts (Azure credential-theft campaign):** the exposure vector here is infostealer-compromised employee credentials, not a cloud-platform flaw — prioritize endpoint infostealer detection and credential rotation over waiting on a vendor patch.
- **Truffle Security (leaked AWS keys):** delete unused root access keys across every account including personal ones, sort IAM keys by age with a maximum-age policy, set spend/budget alerts (90.5% of compromised-key accounts had none), and treat any exposed secret as permanently compromised rather than assuming it went unnoticed.
- **Security teams (Stripe key leak, npm supply chain):** scan merchant/CI code and dependency trees for hardcoded secrets before publishing, and don't rely on `--ignore-scripts` alone as a supply-chain control — the RedC2 loader technique bypasses install hooks entirely.

**On AI-agent and shadow-AI risk:**
- **Akamai's 5-point CISO roadmap:** govern high-usage AI "power users" specifically rather than applying uniform policy, actively eliminate shadow AI by extending visibility to personal/unmanaged accounts, inspect the AI interaction layer itself, vet AI browser extensions before allowing them, and apply distinct security controls to autonomous AI agents rather than treating them like standard user accounts.

---

## Caveats
- Compiled from public vendor advisories, CISA alerts, and security-press aggregation (The Hacker News, BleepingComputer, SecurityWeek, Cybersecurity Dive, Help Net Security, GBHackers, CyberPress, Cybernews, and others) rather than direct primary-source verification of every claim; confirm exact scope, CVSS scores, and patch status against vendor advisories before acting.
- The Shell/Cl0p breach and the Azure/Entra credential-theft campaign are both based on threat-actor claims and dark-web listings; affected organizations are still investigating and have not all confirmed scope or authenticity.
- Microsoft's Entra ID CVE-2026-69836 was briefly flagged as exploited in the wild before Microsoft corrected the advisory to "not exploited" — included here for completeness given the CVSS 10.0 severity, not as an active-exploitation item.
- The T-Mobile Salt Typhoon cable-cutting incident occurred in 2024; it is included this week because it was only publicly disclosed on August 18, 2026.
- The Akamai and Truffle Security reports are standing research referenced this period rather than newly published this exact week.

## Sources
- [Cybersecurity News — Shell Investigating Data Breach Following Cl0p Ransomware Group Claim](https://cybersecuritynews.com/shell-investigating-data-breach/)
- [GBHackers — Shell Investigates Data Breach After Cl0p Ransomware Claims Theft of 89GB Corporate Data](https://gbhackers.com/shell-investigates-data-breach/)
- [CPO Magazine — Clop Ransomware Gang Breaches 50+ Organizations, Including GE, Philips, and Shell](https://www.cpomagazine.com/cyber-security/clop-ransomware-gang-breaches-50-organizations-including-ge-philips-and-shell/)
- [Help Net Security — JSP webshells being dropped on unpatched PTC Windchill instances](https://www.helpnetsecurity.com/2026/06/29/ptc-windchill-cve-2026-12569-exploited/)
- [BleepingComputer — Clop ransomware targets Windchill, FlexPLM in data theft attacks](https://www.bleepingcomputer.com/news/security/clop-ransomware-targets-windchill-flexplm-in-data-theft-attacks/)
- [Cybernews — Hackers dump 1.7M McDonald's records in Azure credential theft hitting Fortune 500 firms](https://cybernews.com/security/mcdonalds-vodafone-azure-microdoft-credential-theft/)
- [SecurityWeek — Fortune 500 Companies Hit in Azure Data Theft Campaign](https://www.securityweek.com/fortune-500-companies-hit-in-azure-data-theft-campaign/)
- [Help Net Security — Hacker claims millions of records stolen from corporate Azure tenants](https://www.helpnetsecurity.com/2026/08/18/azure-data-leak-fortune-500-companies/)
- [TechCrunch — T-Mobile "chopped a cable" to expel Chinese hackers from its network](https://techcrunch.com/2026/08/19/t-mobile-chopped-a-cable-to-expel-chinese-hackers-from-its-network/)
- [Cybernews — T-Mobile cut cable by hand to stop Chinese hackers](https://cybernews.com/security/t-mobile-cut-cable-stop-chinese-hackers/)
- [Cybersecurity News — DOJ Charges 17 Iranian Hackers in IRGC-Linked Campaign That Stole 31.5TB of Research Data](https://cybersecuritynews.com/doj-charges-17-iranian-hackers/)
- [The Record — US charges Iranians for sprawling hacking campaign on government agencies, universities](https://therecord.media/iran-cyberattacks-us-doj)
- [BleepingComputer — US charges Iranian hackers over $3.4 billion intellectual property theft](https://www.bleepingcomputer.com/news/security/us-charges-iranian-hackers-over-34-billion-intellectual-property-theft/)
- [GBHackers — 659 Stripe Merchant API Keys Leaked Online, Exposing 688,000 Customer Records](https://gbhackers.com/659-stripe-merchant-api-keys/)
- [Security Affairs — 50,000 Stripe Secrets Leaked in Public Code](https://securityaffairs.com/197504/cyber-crime/50000-stripe-secrets-leaked-in-public-code.html)
- [The Hacker News — Critical GitLab GraphQL Flaw Could Let Unauthenticated Attackers Delete Public Projects](https://thehackernews.com/2026/08/critical-gitlab-graphql-flaw-could-let.html)
- [eSecurity Planet — GitLab Patches Critical CVE-2026-19478 GraphQL Vulnerability](https://www.esecurityplanet.com/threats/gitlab-patches-critical-cve-2026-19478-graphql-vulnerability/)
- [The Hacker News — 14 Trojanized npm Packages Drop RedC2 4.0 Linux Backdoor With AI-Assisted C2](https://thehackernews.com/2026/08/14-trojanized-npm-packages-drop-redc2.html)
- [The Hacker News — ⚡ Weekly Recap: AI-Powered PLC Attacks, GitLab Attacks, Stripe Key Leaks and More](https://thehackernews.com/2026/08/weekly-recap-ai-powered-plc-attacks.html)
- [Help Net Security — Microsoft patches critical Entra ID vulnerability (CVE-2026-69836)](https://www.helpnetsecurity.com/2026/08/21/microsoft-entra-id-vulnerability-cve-2026-69836/)
- [The Hacker News — Microsoft Patches Severe Entra ID Flaw (CVSS 10.0) Allowing Remote Code Execution](https://thehackernews.com/2026/08/microsoft-entra-id-flaw-cvss-100.html)
- [CISA — CISA Adds Four Known Exploited Vulnerabilities to Catalog (Aug 18, 2026)](https://www.cisa.gov/news-events/alerts/2026/08/18/cisa-adds-four-known-exploited-vulnerabilities-catalog)
- [CISA — CISA Adds Two Known Exploited Vulnerabilities to Catalog (Aug 20, 2026)](https://www.cisa.gov/news-events/alerts/2026/08/20/cisa-adds-two-known-exploited-vulnerabilities-catalog)
- [BleepingComputer — CISA orders feds to patch actively exploited TrueConf Server flaws](https://www.bleepingcomputer.com/news/security/cisa-orders-feds-to-patch-actively-exploited-trueconf-server-flaws/)
- [Security Affairs — U.S. CISA adds TrueConf Server flaws to its Known Exploited Vulnerabilities catalog](https://securityaffairs.com/197602/security/u-s-cisa-adds-trueconf-server-flaws-to-its-known-exploited-vulnerabilities-catalog.html)
- [Industrial Cyber — CISA, NSA, FBI warn of Siemens S7 PLC exploitation using AI-generated scripts](https://industrialcyber.co/industrial-cyber-attacks/cisa-nsa-fbi-warn-of-siemens-s7-plc-exploitation-using-ai-generated-scripts-to-disrupt-critical-industrial-processes/)
- [The Hacker News — The Outsized Shadow: Why 5% of AI Users Are Your Biggest Security Risk (Akamai)](https://thehackernews.com/2026/08/the-outsized-shadow-why-5-of-ai-users.html)
- [Akamai — Enterprise AI Usage Risk Report 2026](https://www.akamai.com/lp/state-of-the-internet/enterprise-ai-risk-report)
- [BleepingComputer — Hundreds of leaked AWS keys give full control over corporate accounts](https://www.bleepingcomputer.com/news/security/hundreds-of-leaked-aws-keys-give-full-control-over-corporate-accounts/)
- [Infosecurity Magazine — Researchers Uncover Thousands of Leaked AWS Keys](https://www.infosecurity-magazine.com/news/researchers-thousands-eaked-aws/)
