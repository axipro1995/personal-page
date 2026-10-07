# Weekly Tech Update Report — Week Ending October 8, 2026

**Scope:** Releases, security fixes and breaking changes for core backend tools, checked against **2026-10-01 to 2026-10-08**.

**Coverage caveat:** Web search returned very little dated inside this window. Most results were older release pages or generic aggregators. Items below are marked **[in window]** only when a source placed them there. Everything else is the latest known state, taken from this week's searches or carried forward from the 2026-10-01 report. Treat "no release found" as "not confirmed", not "none happened". Verify against official release pages before acting.

## TL;DR

- **Nothing confirmed as released inside the window** across the searched tooling. The week's real signal is upcoming dates.
- **Upcoming dates:**
  - **2026-10-12** — React Native 0.88 GA scheduled (rc.0 09-08, rc.1 09-15, rc.3 09-28).
  - **2026-10-15** — PostgreSQL 19 RC1 targeted, GA targeted **10-29** (per the 10-01 report).
  - **2026-10-20** — Oracle Critical Patch Update, which includes the Java CPU/CSPU for JDK 8/11/21/25.
  - **2026-10-27** — Kubernetes 1.34 end of life.
  - **2026-11-12** — PostgreSQL 14 end of life.
  - **October 2026** — Node.js 26 enters LTS, and Node.js 27 opens its alpha channel under the new once-a-year release model.
- **Security items to confirm patched (disclosed before the window, still relevant):**
  - **Jackson-databind:** fixes for CVE-2026-19032 (arbitrary file-system provider via `Path` binding) and CVE-2026-68497 (DoS via huge `Duration`/`XMLGregorianCalendar` numbers) are in **2.22.2+**. Latest is **2.22.3** (2026-09-21). Also confirm CVE-2026-91776, fixed in 2.22.3 / 2.21.7 / 2.18.11, per the 10-01 report.
  - **Elasticsearch:** three DoS CVEs (CVE-2026-94399, -94396, -94408), fixed in 8.19.22 / 9.4.7 / 9.5.3+. Upgrade is the only mitigation.
  - **Spring:** CVE-2026-59313 (9.8, SSE injection in WebMvc.fn) and Spring Security CVE-2026-59270 (embedded LDAP admin DN exposed).
  - **MongoDB:** CVE-2026-82067 (8.1, auth bypass), fixed in 8.0.30/8.0.32.
  - **Amazon EKS:** CVE-2026-86831 (NetworkPolicy bypass), fixed in network-policy-agent 1.4.0+.
- **Breaking-change watch:**
  - Maven 3.10.0 switched classpath resolution to breadth-first ordering.
  - MongoDB 9.0 changed null-comparison semantics.
  - Netty 4.1 has an EOL announcement (2026-09-09).
  - Protobuf v38 breaking-change notice (Bazel 9 minimum, C++ major bump), targeted for 2027 Q1.

---

## Java

| Tool | Latest known | Notes |
|---|---|---|
| Java (core) | Java 27 (GA 2026-09-15) | October CPU on **2026-10-20**. Azul now ships monthly CSPUs (since Aug 2026), which Oracle's Java CSPU cadence mirrors. |
| Netty | 4.2.18.Final / 4.1.138.Final (2026-09-09) | **Netty 4.1 EOL announced 2026-09-09.** Plan the move to 4.2. HTTP/2 header validation is opt-in by default in this line. |
| Vert.x | 5.2.0 (2026-09-21) | Search also surfaced 4.5.32 (08-07) and 5.1.6 (08-05) on the older lines. |
| Spring | Boot 4.2.0-M2, Framework 7.1.0-M2, Security 7.2.0-M2 | Week of 09-21 milestones. No GA-line release confirmed in window. Boot 4.2 GA expected November. |
| Hibernate ORM | 7.4.11.Final (2026-09-27) | No newer release found. 8.0.0.Beta3 on the dev line. |
| JDBC | pgjdbc 42.7.13 (07-07), MySQL Connector/J 26.7.0 (07-29) | No new release found. |
| Protobuf | 4.36.2 / 7.36.2 (~09-17) | v38 breaking-changes notice published 09-22. |
| Dagger | not found | No data surfaced this week. |
| Jackson | 2.22.3 (2026-09-21) | See security section above. |
| Log4j | log4j-core last seen 2026-07-07 | No newer release found. |
| Guava | last seen 2026-04-14 | No newer release found. |
| JUnit | Jupiter last seen 2026-04-26 | No newer release found. |
| Maven | 3.10.0 (2026-09-27) | Breadth-first classpath resolution. Re-verify diamond dependency conflicts. |
| Gradle | not found | No data surfaced this week. |

## Node.js

- **Node.js:** v26.0.0 released 2026-05-05 (Temporal on by default, V8 14.6, Undici 8, removal of the legacy `_stream_*` modules). **Node 26 moves to LTS in October 2026.** Starting October, the project moves to **one release per year**, with Node 27 shipping April 2027 and reaching LTS October 2027. Node 20 is already EOL. The latest security releases found were 2026-06-18 (12 vulnerabilities) and **2026-07-27** (highest severity HIGH on 22.x/24.x/26.x). No October security release was found.
- **axios:** the notable CVE batch (CVE-2026-42033 / -42035 / -42039: prototype-pollution gadgets, header injection, `toFormData` recursion DoS) dates from April 2026. Confirm you run a fixed version.
- **express, bcryptjs, moment, pg, sequelize, socket.io, jest, prettier:** no new release or advisory found in window. From the last report: pg 8.23.1, socket.io 4.8.4, Sequelize 6.37.8 (fixes CVE-2026-30951), body-parser 1.20.6/2.3.0 (fixes CVE-2026-12590).
- **React / React Native:** React Native **0.88 is in RC** (rc.0 09-08, rc.1 09-15). GA is scheduled for **2026-10-12**. It carries the 0.87 breaking changes: strict TypeScript API default, deep-import removal and `InteractionManager` removal.

## Database and Storage

- **Oracle:** next CPU **2026-10-20**.
- **PostgreSQL:** 19 is still in beta. RC1 is targeted 2026-10-15 and GA 2026-10-29, per the 10-01 report. Beta 4 dropped six planned features, including SQL/PGQ. **PostgreSQL 14 EOL is 2026-11-12.** Upgrade to 18 or 19.
- **Redis:** the advisories found were older (2026-05-05, 07-27 and 08-28; the July one affects versions before 8.8.0). Nothing new in window.
- **Kafka:** 4.2.2 (2026-09-29, from the last report). Nothing newer found.
- **MongoDB:** 9.0 GA'd 2026-09-29/30 with breaking changes (null-comparison semantics for dotted paths, a cap on concurrent multi-document transactions, a renamed metric). Nothing newer found.
- **Cassandra:** no data surfaced.
- **Elasticsearch:** the 09-25 DoS advisories above. Nothing newer found.
- **BigQuery:** continuous queries can now write into Apache Iceberg managed tables via `INSERT`. The BigQuery Data Transfer Service MCP server is GA. Neither item is dated in the search result.

## OS and Platform

- **Docker:** newest Docker Engine release notes found were 29.6.1 (June 2026), which fixes memory exhaustion from malicious `/etc/passwd`-style files in images and a BuildKit custom-frontend hardening bypass. Docker Sandboxes 0.42.0+ addresses CVE-2026-77179 and CVE-2026-79994.
- **Kubernetes:** no new minor release in October. Latest minor is 1.36 (patch 1.36.4 on 08-11, 1.36.5 on 09-15). **1.34 EOL is 2026-10-27.** The 09-15 patches fixed CVE-2026-39821 (CVSS 9.6, `x/net/idna`).
- **GKE:** 1.35.7-gke.1027000 became the Extended-channel default on 08-26. The 512-pods-per-node GA and the managed ISTIOD deprecation (09-28) are from the last report.
- **GCP Compute Engine:** flexible CUDs are now GA for G2 and G4 GPU machine series (undated in the search result).
- **GCP Maps / Translate / Storage, Google Analytics:** no new data. Last known: Places SDK for Android 6.0.2 (09-30) removed several methods.
- **AWS (S3, EC2, Lambda, EKS, CloudFront, Route 53):** no October items found. Recent context: Route 53 Accelerated recovery (60-minute RTO), EKS EFA/placement-group support on Auto Mode and Karpenter (July), S3 Tables Iceberg V3 (09-30, previous report). EKS CVE-2026-86831 is still worth confirming.
- **Cloudflare:** nothing new found. The 09-25 emergency WAF release (WordPress CVE-2026-87902, Adobe Commerce CVE-2026-75650) is from the last report.
- **Firebase, PayPal, Apple Pay:** nothing new found. Firebase Apple SDK stops publishing to CocoaPods after October 2026.

## Suggested actions

1. Confirm Jackson ≥ 2.22.3, Elasticsearch patched, Spring/Spring Security patched, and the EKS agent ≥ 1.4.0.
2. Start Kubernetes 1.34 and PostgreSQL 14 upgrade planning. Both EOL dates are within about five weeks.
3. Schedule Netty 4.1 → 4.2 migration.
4. Prepare for the 10-20 Oracle/Java CPU.
5. Re-run this check mid-week after 10-12 (React Native 0.88) and 10-15 (PostgreSQL 19 RC1).

## Sources

Netty 4.1 EOL announcement (netty.io/news/2026/09/09); InfoQ Spring news roundup (2026-09-21); endoflife.ai Jackson 2.22; HeroDevs CVE-2026-19032 / CVE-2026-68497; OpenJS Foundation Q2 2026 security update; ecorpit.com Node.js release model articles; nodejs.org v26.0.0 release post; gitclear React Native v0.88.0-rc.0/rc.1; versionlog.com PostgreSQL 19; wz-it.com PostgreSQL 14 EOL; endoflife.date October 2026 timeline; cyber.gc.ca Redis advisories; releases.sh Docker Engine; Google Cloud release notes; AWS News Blog; Cloudflare changelog; Azul / Oracle CPU pages; plus the 2026-10-01 weekly report for carried-forward items.
