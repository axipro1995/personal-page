# Weekly IT Security Update — Week Ending September 24, 2026

**Scope:** Notable security incidents, tool/platform security updates, and international/large-organization security reporting from roughly September 14 – September 24, 2026. This edition bridges two weeks since the last report (September 9), so a handful of significant carry-forward items from that gap (notably McKesson) are included and flagged as such. Compiled from public vendor advisories, CISA, and security press.

## TL;DR
- **A coordinated wave hits network-edge security appliances simultaneously — Check Point, F5, Arista, and Cisco all confirm active exploitation:** F5 BIG-IP APM (CVE-2026-94127, CVSS 9.8, heap overflow → unauthenticated RCE), Check Point Security Gateway (CVE-2026-85102, pre-auth RCE) and Management Server (CVE-2026-93616, path traversal, exploited since as early as July 23), and Arista VeloCloud Orchestrator (CVE-2026-93952, CVSS 10.0) were all added to CISA's KEV catalog on September 22 with a September 25 federal remediation deadline. Days earlier, Cisco disclosed CVE-2026-76460 (CVSS 10.0 auth bypass → root command execution in Identity Services Engine/ISE-PIC) and CVE-2026-76461 (root RCE in Secure Email Gateway/AsyncOS), both under active exploitation and added to KEV September 16.
- **WordPress patches its most serious core flaw in years — attackers probing within 5 hours:** CVE-2026-87902 (CVSS 9.2) is an unauthenticated local-file-inclusion-to-RCE bug affecting every WordPress core release from 4.7.0 through 7.1.1 — nearly a decade of versions. Fixed in 7.1.2 (September 22); Patchstack logged the first exploitation probes at 17:44 UTC that same day.
- **Brevo supply-chain compromise pushes ClickFix malware to 100,000+ websites:** Attackers used a stolen Cloudflare API key belonging to the customer-engagement platform Brevo to inject malicious JavaScript into embedded widgets for roughly 5.5 hours on September 14. Visitors saw a fake Cloudflare CAPTCHA instructing them to paste and run a command — the "ClickFix" social-engineering technique — leading to malware execution on victim machines.
- **"Plugin4Shell": zero-click RCE breaks plugin-integrity verification in four major AI coding agents:** AIR Security disclosed a flaw bypassing SHA-pinning — the mechanism meant to lock an installed plugin to a reviewed version — in Claude Code, OpenAI Codex, GitHub Copilot, and Google Gemini CLI. As of disclosure (~September 17-18), two of the four remained unpatched.
- **McKesson confirms the largest healthcare breach disclosed in 2026 (carry-forward from late August):** ShinyHunters claims ~284 million rows of patient data (names, SSNs, diagnoses, billing, and sensitive details like terminal-illness status) exfiltrated from McKesson's Salesforce and Snowflake environments over four days ending August 25, via compromised third-party application access. A $55.2M ransom demand's September 1 deadline passed with no confirmed payment; no lawsuits had been filed as of this period, though several firms are investigating claims.
- **Metaencryptor and ShinyHunters keep up pressure across manufacturing and government targets:** Metaencryptor ransomware hit a South Korean camera-module/equipment maker and a Japanese automotive-parts manufacturer this period, while ShinyHunters separately claimed a breach of a U.S. federal law-enforcement agency's data.
- **A hidden Meta Muse setting could turn the AI assistant into a backdoor:** A proof-of-concept published September 21 shows malware on a Mac can silently flip a configuration flag so that a user's dictated prompts route to an attacker instead of Meta's assistant — a novel AI-agent hijack class rather than a traditional memory-corruption bug.
- **Regulatory shift: CISA retires its weekly Vulnerability Bulletin September 28**, moving from severity-based to risk-based vulnerability communication, while the EU's Cyber Resilience Act active-exploitation reporting obligation (in force since September 11) and ENISA's NIS360 2026 report both push European critical-sector organizations toward faster, more structured incident disclosure.

---

## 1. Major Security Incidents

| Incident | Date | Details |
|---|---|---|
| **Brevo / Cloudflare API key — ClickFix supply-chain attack** | Sept 14 (~5.5 hrs) | Attacker obtained a compromised Cloudflare API key for customer-engagement platform Brevo and used it to inject malicious script into embedded Brevo widgets across 100,000+ customer websites. Visitors were shown a fake Cloudflare CAPTCHA instructing them to paste and execute a command in the Windows Run dialog — the "ClickFix" technique — resulting in malware execution on victims' machines. |
| **McKesson / ShinyHunters — largest 2026 healthcare breach** *(carry-forward, discovered Aug 25)* | Ongoing fallout through this period | ShinyHunters pivoted into McKesson's Salesforce and Snowflake environments via compromised third-party application access, exfiltrating ~1TB / ~284 million rows of patient and business data (names, addresses, SSNs, diagnoses, medications, billing, terminal-illness status, sexual orientation) over four days. A $55.2M ransom demand's September 1 deadline passed; McKesson has not confirmed payment. No lawsuits filed yet as of this period, though plaintiffs' firms are actively soliciting affected individuals. |
| **Metaencryptor ransomware — manufacturing targets** | This period (Week 4 Sept) | Hit a South Korean camera-module/equipment manufacturer and a Japanese automotive-parts manufacturer, per ASEC's weekly ransom tracking. |
| **ShinyHunters claims breach of a U.S. federal law-enforcement agency** | This period | Group claims theft of sensitive data from a federal law-enforcement agency; scope and confirmation not yet independently verified. |
| **WordPress core mass-probing following CVE-2026-87902 disclosure** | Probing began Sept 22, ~5 hrs post-patch | Once WordPress 7.1.2 shipped (see §2), attackers immediately began scanning for unpatched sites running the vulnerable local-file-inclusion path, per Patchstack telemetry — a reminder that patch releases themselves function as exploitation roadmaps. |
| **Plugin4Shell — zero-click compromise path into AI coding agents** | Disclosed ~Sept 17-18 | AIR Security's PoC shows an attacker can bypass SHA-pin verification to swap a trusted plugin for a malicious one with no user interaction, across Claude Code, OpenAI Codex, GitHub Copilot, and Google Gemini CLI. Two of the four agents remained unpatched at disclosure — a supply-chain risk directly inside developer AI tooling. |
| **Meta Muse hidden-setting hijack PoC** | PoC published Sept 21 | Malware with local access on macOS can toggle an undocumented Muse configuration flag to reroute a user's voice-dictated prompts to an attacker-controlled endpoint instead of Meta's assistant, effectively turning the AI assistant into a covert backdoor/exfiltration channel. |

## 2. Major Tool / Platform Security Updates

**⚠️ Actively exploited (CISA KEV / confirmed in-the-wild):** Cisco ISE/ISE-PIC, Cisco Secure Email Gateway (AsyncOS), F5 BIG-IP APM, Check Point Security Gateway & Management Server, Arista VeloCloud Orchestrator, Zyxel GS1900 switches, Acronis Backup. **⚠️ High-priority, mass-scanning underway:** WordPress Core.

| Product | Issue | CVE(s) | Severity | Status / Action |
|---|---|---|---|---|
| Cisco Identity Services Engine / ISE-PIC | Insufficient authentication on an API endpoint → unauthenticated bypass of the management interface with root command execution | CVE-2026-76460 | Critical, CVSS 10.0, actively exploited | Patch immediately; added to CISA KEV Sept 16. ISE is a core identity/NAC control point — treat any exposed instance as potentially already compromised and rotate downstream credentials/certificates it issued. |
| Cisco Secure Email Gateway (AsyncOS) | Email-parsing flaw exploitable remotely without authentication → root-level arbitrary command execution | CVE-2026-76461 | Critical, CVSS 9.8, actively exploited | Patch immediately; disclosed alongside the ISE flaw this period. |
| F5 BIG-IP Access Policy Manager (APM) | Heap-based buffer overflow → unauthenticated RCE | CVE-2026-94127 | Critical, CVSS 9.8, actively exploited | F5 advisory and CISA KEV both Sept 22; federal deadline Sept 25. Confirmed targeted in the wild alongside the Check Point and Arista flaws below. |
| Check Point Security Gateway (Quantum) | Pre-authentication remote code execution | CVE-2026-85102 | Critical, actively exploited | Patched Sept 9; exploitation began within days. Added to KEV Sept 22. |
| Check Point Management Server | Path traversal | CVE-2026-93616 | Critical, actively exploited | Exploited as far back as July 23 per vendor telemetry — predates public disclosure by roughly two months. Added to KEV Sept 22. |
| Arista VeloCloud Orchestrator (on-prem VCO) | Improper input validation → remote access to privileged internal functionality | CVE-2026-93952 | Critical, CVSS 10.0, actively exploited | Arista: "discovered externally and known to be actively exploited." Patch immediately; added to KEV Sept 22, federal deadline Sept 25. |
| Zyxel GS1900-series switches | Stack-based buffer overflow | CVE-2026-7273 | High, actively exploited | Added to CISA KEV Sept 21. |
| Acronis Backup | Incorrect default permissions | CVE-2026-87886 | High | Added to CISA KEV Sept 16 alongside the Cisco ISE flaw. |
| WordPress Core (4.7.0 – 7.1.1) | Unauthenticated local file inclusion in page-template resolution → remote code execution | CVE-2026-87902 | Critical, CVSS 9.2 | Fixed in 7.1.2 (Sept 22), backported to every branch back to 4.7 still receiving security fixes — nearly a decade of releases affected. Attackers began probing within 5 hours of release; update immediately, don't wait for auto-update cycles. |
| Claude Code / OpenAI Codex / GitHub Copilot / Google Gemini CLI | "Plugin4Shell" — zero-click bypass of plugin SHA-pin verification enabling malicious plugin substitution | — (vendor-specific advisories) | Critical (supply-chain) | Verify each vendor's patch status individually — two of the four remained unpatched at disclosure. Treat AI coding-agent plugin ecosystems as a live supply-chain attack surface, not a trusted-by-default one. |
| Microsoft (portfolio, carry-forward) | September Patch Tuesday total revised to 973-995 CVEs (source-dependent) including a CVSS 10.0 Azure AI Foundry flaw and two exploited zero-days (CVE-2026-81963, CVE-2026-85880) | — | Multiple Critical | Carry-forward from last edition: if DNS, DHCP, Failover Cluster, Internet Connection Sharing, or HTTP Print Provider RCEs from this cycle aren't yet remediated, prioritize them — CISO analysts (Action1) flag these as the highest-consequence, most network-reachable items in the batch. |

## 3. Security Reports — International / Large-Organization Highlights

**CISA — retiring the weekly Vulnerability Bulletin (effective September 28):**
- CISA is discontinuing its long-running weekly Vulnerability Bulletin as part of a shift from severity-based (CVSS-driven) vulnerability communication to a modern, risk-based approach — likely reflecting the reality that CVSS-only triage no longer scales against months like this one, where dozens of Critical/CVSS-10 flaws land in parallel across unrelated vendors.

**ENISA — NIS360 2026 report (EU critical-sector cybersecurity maturity):**
- Third annual edition finds uneven progress against the NIS2 directive: banking, electricity, and telecoms lead in maturity; trust services, aviation, and financial-market infrastructure are advancing into the high-maturity band. A "risk zone" — sectors where criticality outpaces maturity — now includes health, railway, maritime, ICT service management, space, public administration, and water.

**EU Cyber Resilience Act — active-exploitation reporting now in force (since September 11):**
- Manufacturers of "products with digital elements" with EU exposure must report actively exploited vulnerabilities to ENISA within 24 hours via the newly launched Single Reporting Platform, ahead of the full conformity regime in December 2027. This week's cluster of simultaneously-exploited edge-device CVEs (Cisco, F5, Check Point, Arista) is the first real stress test of that pipeline for in-scope vendors.

**Zscaler — 2026 AI Security Report:**
- Reports an 83% year-over-year surge in enterprise AI activity, alongside growing oversight gaps — many organizations still lack a basic inventory of which AI models and embedded AI features are active in their environment, or where sensitive data flows through them.

**Check Point Research — AI Security Report 2026:**
- Documents the same trend from the threat side: rising use of AI by attackers for reconnaissance, phishing content generation, and — as this week's Meta Muse and Plugin4Shell disclosures illustrate — AI agents themselves becoming a new class of exploitable asset rather than purely a defensive tool.

**ASEC (AhnLab) — Ransom & Dark Web Issues, Week 4 September 2026:**
- Standing weekly tracker documenting this period's Metaencryptor activity against South Korean and Japanese manufacturers and continued dark-web trading of breach data, consistent with sustained ransomware-as-a-service operational tempo through September.

## 4. Recommendations from Professionals / Organizations

**On the simultaneous network-edge exploitation wave (Cisco, F5, Check Point, Arista):**
- **Consensus across vendor advisories and CISA guidance:** these are management-plane and VPN/SSL-gateway products that should never be reachable from the open internet. Patch to the fixed versions immediately, then hunt logs back to the earliest known exploitation date for each CVE — notably Check Point's CVE-2026-93616, which vendor telemetry places as far back as July 23, well before public disclosure. Treat "patched" as necessary but not sufficient; assume compromise until logs prove otherwise.
- **CISA (KEV federal deadline framing):** the shared September 25 remediation deadline across four unrelated vendors this week is a useful forcing function even for non-federal organizations — align internal patch SLAs to KEV deadlines rather than vendor-published severity alone.

**On WordPress (CVE-2026-87902):**
- **Patchstack and Security Affairs:** given probing began within 5 hours of the patch shipping, do not rely on staged or delayed auto-update windows for this one — apply 7.1.2 immediately across all sites, including those on older 4.7–7.0 branches that still receive security backports.

**On AI coding-agent supply chain (Plugin4Shell):**
- **AIR Security's disclosure guidance:** SHA-pinning alone is not sufficient plugin-integrity assurance if the verification step itself can be bypassed. Engineering teams using Claude Code, Codex, Copilot, or Gemini CLI should confirm vendor patch status individually, audit which plugins are installed and from where, and treat AI-agent plugin marketplaces with the same scrutiny as any other third-party dependency source.

**On the McKesson breach and third-party SaaS exposure (carry-forward):**
- **General healthcare/enterprise guidance reinforced by this incident:** inventory exactly which third-party applications hold write/read access into core SaaS platforms (Salesforce, Snowflake) that house regulated data, and require attestation of least-privilege API scoping from vendors — the entry point here was compromised third-party application access, not a direct McKesson-side flaw.

**On AI-agent-as-attack-surface (Meta Muse hijack PoC):**
- **Emerging guidance from this period's disclosures:** treat AI assistant configuration settings (especially ones not surfaced in the primary UI) as a security-relevant attack surface requiring the same change-monitoring and integrity controls as system-level settings — local malware modifying an assistant's routing config is a materially different threat model than prompt injection alone.

**On patch prioritization generally (Action1, CISA risk-based shift):**
- With CISA moving away from pure severity-based bulletins and this period showing multiple unrelated CVSS-10.0 flaws exploited in parallel, security teams should prioritize by *confirmed exploitation and network reachability* first, matching the KEV-deadline-driven approach federal agencies now use, rather than triaging by CVSS score alone.

---

## Caveats
- Compiled from public vendor advisories, CISA alerts, and security-press aggregation (The Hacker News, BleepingComputer, SecurityWeek, Help Net Security, Patchstack, Security Affairs, ASEC, and weekly-roundup sites) rather than direct primary-source verification of every claim; confirm exact scope, CVSS scores, CVE IDs, and patch status against vendor advisories before acting.
- This edition spans roughly ten days (Sept 14–24) rather than a strict single week, because the prior report covered the week ending September 9 and no edition was produced for the intervening week — some items (e.g., Cisco ISE/ISE-PIC, WeChat's "WeWorm" flaw patched Aug 21) may have first been reported slightly earlier than their KEV/press-coverage date shown here.
- The McKesson/ShinyHunters entry is carried forward from late August because its ransom-deadline outcome and lack of confirmed lawsuits only became clear during this period; treat the 284-million-record figure as ShinyHunters' claim (rows of raw data, not confirmed unique patients) pending McKesson's own accounting.
- ShinyHunters' claimed breach of a U.S. federal law-enforcement agency is not yet independently confirmed at time of writing.
- The Zscaler and Check Point AI security reports are standing 2026 annual research referenced this period rather than newly published in this exact window.

## Sources
- [The Hacker News — Weekly Recap: Cisco 0-Day, AI Agent RCE, ClickFix Attacks, ClickFix Surge, and Browser Hijacks](https://thehackernews.com/2026/09/weekly-recap-cisco-0-day-ai-agent-rce.html)
- [SecurityWeek — Root RCE Zero-Day in Cisco Secure Email Gateway Under Active Exploitation](https://www.securityweek.com/root-rce-zero-day-in-cisco-secure-email-gateway-under-active-exploitation/)
- [Help Net Security — Plugin4Shell: Zero-click RCE vulnerability hit four major AI coding agents, two remain unpatched](https://www.helpnetsecurity.com/2026/09/18/plugin4shell-ai-coding-agents-vulnerability/)
- [The Register — AI coding agents' 0-click RCE flaw could hand attackers keys to the kingdom](https://www.theregister.com/security/2026/09/17/ai-coding-agents-0-click-rce-flaw-could-hand-attackers-keys-to-the-kingdom/5297335)
- [The Hacker News — One Hidden Meta Muse Setting Could Let Attackers Turn the AI Assistant Into a Backdoor](https://thehackernews.com/2026/09/one-hidden-meta-muse-setting-could-let.html)
- [Help Net Security — Attackers hit Check Point Management Servers and Spark firewalls, F5 BIG-IP APM instances](https://www.helpnetsecurity.com/2026/09/23/check-point-f5-big-ip-apm-zero-days-targeted/)
- [SecurityWeek — Arista Urges Immediate Patching of Exploited VCO Zero-Day](https://www.securityweek.com/arista-urges-immediate-patching-of-exploited-vco-zero-day/)
- [CISA — Adds Four Known Exploited Vulnerabilities to Catalog (Sept 22, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/22/cisa-adds-four-known-exploited-vulnerabilities-catalog)
- [CISA — Adds One Known Exploited Vulnerability to Catalog (Sept 21, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/21/cisa-adds-one-known-exploited-vulnerability-catalog)
- [CISA — Adds Two Known Exploited Vulnerabilities to Catalog (Sept 16, 2026)](https://www.cisa.gov/news-events/alerts/2026/09/16/cisa-adds-two-known-exploited-vulnerabilities-catalog)
- [Patchstack — WordPress 7.1.2 Security Release: Unauthenticated LFI to RCE](https://patchstack.com/articles/wordpress-7-1-2-security-release-unauthenticated-lfi-to-rce/)
- [Patchstack — CVE-2026-87902: Attackers Started Probing WordPress Sites Hours After the Patch](https://patchstack.com/articles/cve-2026-87902-attackers-started-probing-wordpress-sites-hours-after-the-patch/)
- [Security Affairs — CVE-2026-87902: how close is your WordPress to remote code execution?](https://securityaffairs.com/199564/hacking/cve-2026-87902-how-close-is-your-wordpress-to-remote-code-execution.html)
- [Cybernews — McKesson Breach: ShinyHunters Claims 284m Patient Records](https://cybernews.com/news/mckesson-breached-shinyhunters-claims-284m-records/)
- [HIPAA Journal — McKesson Cyberattack: Stolen Data Includes 6.4 Million Unique Email Addresses](https://www.hipaajournal.com/mckesson-data-breach/)
- [SecurityWeek — McKesson Confirms Data Breach as Attacker Deadline Looms](https://www.securityweek.com/mckesson-confirms-data-breach-as-attacker-deadline-looms/)
- [TechCrunch — Leaks, data breaches, and ransom notes: The worst hacks of 2026 so far](https://techcrunch.com/2026/09/15/the-worst-hacks-and-breaches-of-2026-so-far/)
- [ASEC — Ransom & Dark Web Issues Week 4, September 2026](https://asec.ahnlab.com/en/95545/)
- [content.govdelivery.com — CISA to Sunset Weekly Vulnerability Bulletin on September 28, 2026](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/42b055b)
- [SC Media — ENISA NIS360 2026 report shows uneven cybersecurity improvements across EU critical sectors](https://www.scworld.com/brief/enisa-nis360-2026-report-shows-uneven-cybersecurity-improvements-across-eu-critical-sectors)
- [ENISA — NIS360 2026](https://www.enisa.europa.eu/enisa-nis360-2026)
- [Industrial Cyber — ENISA launches Single Reporting Platform as EU Cyber Resilience Act vulnerability reporting obligations take effect](https://industrialcyber.co/regulation-standards-and-compliance/enisa-launches-single-reporting-platform-as-eu-cyber-resilience-act-vulnerability-reporting-obligations-take-effect/)
- [Zscaler — 2026 AI Security Report](https://www.zscaler.com/press/zscaler-2026-ai-threat-report-83-year-over-year-surge-ai-activity-creates-growing-oversight)
- [Check Point Research — AI Security Report 2026](https://research.checkpoint.com/2026/ai-security-report-2026/)
- [Action1 — September 2026 Patch Tuesday: CISO Executive Summary](https://www.action1.com/patch-tuesday/september-2026-patch-tuesday-ciso-executive-summary/)
- [CyberSecurityNews — Weekly Cybersecurity Newsletter Bulletin, Sept 21](https://cybersecuritynews.com/weekly-cybersecurity-newsletter-sept-21/)
