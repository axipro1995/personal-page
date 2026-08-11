# Weekly Tech Update Report — Week Ending August 12, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-08-05 to 2026-08-12**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, and vendor advisories.

## TL;DR

- **Released this week:** Docker Desktop 4.86.0 (security release), Node.js v26.7.0, pg (node-postgres) 8.23.0, Netty 4.1.137.Final, Vert.x 5.1.6/4.5.32, JUnit 6.1.3, Gradle 9.7.0, Apache Cassandra 5.0.9, AWS Lambda scalable non-VPC network bandwidth, Amazon EC2 C8g/M8g regional expansions, and a cluster of Cloudflare "Agents Week" announcements (Cloudflare OS, Kitesurf browser engine, `@cloudflare/computer`).
- **Patch now — critical security:**
  - **Docker Desktop 4.86.0** (2026-08-10) — **CVE-2026-17106**, a destination-escape flaw in `docker container cp` that lets a crafted copy target escape the intended path. Upgrade immediately; this is a fresh release inside this week's window.
  - **Sequelize** — newly surfaced **CVE-2026-69240** (disclosed 2026-08-03, CVSS 9.8 Critical): SQL injection via the Oracle dialect's `sql-string.js` escape function, which fails to sanitize input beginning with `TO_TIMESTAMP`/`TO_DATE`. Fixed in 6.37.4+; confirm apps using the Oracle dialect are on ≥6.37.8 (current latest).
  - **Socket.IO** — **CVE-2026-69185** (disclosed 2026-08-03): zero-attachment memory exhaustion — a crafted packet can make the server buffer unbounded binary attachments, leading to OOM. Fixed in 3.3.6 / 3.4.5 / 4.2.7+; current latest (4.8.3) already includes the fix, but verify pinned versions.
  - **Jackson (all lines)** — CVE-2026-54512 (critical RCE bypassing `PolymorphicTypeValidator`), CVE-2026-54515 and CVE-2026-59889 (privilege escalation via `@JsonIgnore`/`@JsonView` bypass) — upgrade to ≥2.18.9 / 2.21.5 / 2.22.1 / 3.1.5 / 3.2.1 if not already there. Unchanged from last week; still the highest-impact outstanding Java CVE set.
  - **Spring Framework 7.0.8 / 6.2.19** — CVE-2026-41838 through -41846 (session fixation, WebSocket session-ID predictability, DoS, open redirect, XSS) plus CVE-2026-41850 (SpEL algorithmic DoS) and CVE-2026-41855 (unsafe Jackson JMS deserialization). No new patch this week — still the current mitigations.
  - **MongoDB 8.3.7** — ~24 CVEs fixed, including CVE-2026-13072 (CVSS 9.2, memory corruption in Compute Mode BSON processing) and CVE-2026-13078 (CVSS 7.7, arbitrary file read via crafted aggregation pipeline through the MozJS module-loading hook).
  - **PostgreSQL** — still on the May CVE batch (CVE-2026-6473/-6475/-6477/-6637, CVSS 8.8; CVE-2026-6478 MD5 timing side-channel) pending the **next quarterly release on 2026-08-13** — one day after this report's window closes; watch for it immediately.
  - **pgjdbc** — CVE-2026-54291 (silent SCRAM channel-binding downgrade, fixed 42.7.12+) remains the most recent pgjdbc security issue; no new CVE this week.
  - **axios** — CVE-2026-40175 (Critical, CVSS 9.9, SSRF→RCE gadget chain) and CVE-2026-25639 (DoS) remain the operative advisories; also recall the March 2026 supply-chain compromise of 1.14.1/0.30.4.
- **Breaking-change watch:**
  - **Oracle** targets its first-ever **monthly Critical Security Patch Update on 2026-08-18** (six days after this window closes) — a new, faster cadence supplementing the quarterly CPU; plan for more frequent Java/Database patch cycles going forward.
  - **Amazon EKS** — Kubernetes 1.33 standard support on EKS **ended 2026-07-29** and 1.30 extended support **ended 2026-07-23** — both already past. Clusters still on those versions are now paying the extended-support surcharge (or are unsupported); upgrade is urgent, not just "coming soon."
  - **Kubernetes** 1.34 enters maintenance mode 2026-08-27 (EOL 2026-10-27); **v1.37.0** is due 2026-08-26.
  - Spring Boot 4.0 baseline breaking changes remain in force for anyone migrating: Jackson 3 default, Spring Security 7 defaults (CSRF changes can silently break REST APIs), Undertow dropped.
  - **GCP Maps API** quarterly channel bump (v3.64→v3.65, weekly channel already moved 2026-08-03) — historically carries backwards-incompatible changes; full channel rollout worth monitoring into next week.
  - **GCP Translation API** Basic/Advanced ("v3") client libraries remain **not** cross-compatible — a standing migration hazard.
  - **React Native 0.87.0** is in release-candidate stage (rc.4 as of early August) with a "big cleanup" of deprecated APIs (`InteractionManager`, `SafeAreaView`, and more) — not GA yet, but review the deprecation list before it ships.

---

### Java

**Window checked:** 2026-08-05 to 2026-08-12. Netty (4.1.x), Vert.x, JUnit, Gradle, and Cassandra (see Database section) shipped inside this exact window; other tools' latest stable falls earlier and is reported as such.

**Java (core / JDK)**
- Latest: **JDK 25.0.4** (LTS) and **JDK 26.0.2** (current) — both released 2026-07-21 as part of Oracle's quarterly CPU. Not within the past week.
- **Upcoming:** Oracle's first **monthly Critical Security Patch Update (CSPU)** is targeted for **2026-08-18** — a new faster cadence (previously quarterly-only); next full CPU is 2026-10-20.

**Netty**
- Latest: **4.1.137.Final** (2026-08-06) — **within the past week**. **4.2.17.Final** (2026-08-04) is just outside the window.
- Both are bug-fix releases (buffer-leak fixes in `SslHandler`/`HttpObjectEncoder`, new pluggable decompressor API, io_uring `SO_INQ` support, WebSocket handshake routing fix); no new CVEs.
- Prior security release **4.2.15.Final** (2026-06-02) fixed ~20 CVEs (memory exhaustion, DNS cache poisoning, HTTP/2 DDoS, TLS hostname bypass) — still critical to be on if not already upgraded.

**Vert.x**
- Latest: **5.1.6** (2026-08-05) and **4.5.32** (2026-08-07) — **both within the past week**.
- Bump bundled Netty to 4.2.17.Final (inheriting its fixes) plus bug/vulnerability fixes per the release notes; no breaking changes called out.

**Spring (Framework / Boot)**
- Framework latest: **7.0.8 / 6.2.19** (2026-06-08). Boot latest: **4.1.0** GA (2026-06-10), **3.5.16** on the 3.5.x line. Not within the past week.
- Security: 7.0.8/6.2.19 fixed CVE-2026-41838 through -41846 (session-ID predictability, session fixation, DoS, open redirect, XSS), plus **CVE-2026-41850** (SpEL algorithmic DoS, affects 7.0.0-7.0.7/6.2.0-6.2.18/6.1.0-6.1.27/≤5.3.48) and CVE-2026-41855 (unsafe Jackson JMS deserialization).
- Breaking changes carried from Boot 4.0 baseline: modularized packages, Jackson 3 default, Spring Security 7 CSRF-default changes, Undertow dropped, JSpecify null-safety annotations.

**Hibernate**
- Latest: **7.4.5.Final** (2026-07-12); maintenance releases 7.3.13/7.2.24/7.1.35/6.6.55 shipped 2026-08-02 (just outside window). Next-gen **8.0.0.Beta1** (2026-06-16) targets Jakarta Persistence 4.0.
- No new CVEs surfaced this window.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06). Security fix CVE-2026-54291 (silent SCRAM channel-binding downgrade) remains fixed in 42.7.12+; no new release this week.
- **MySQL Connector/J**: latest **26.7.0** (2026-07-28) — versioning scheme now aligns with the calendar year (26.x = 2026) rather than the old 9.x line. No new release this week.

**Protobuf**
- Latest stable: **v35.1** (2026-06-11); **v36.0-rc2** (2026-08-03) remains a release candidate, not GA, and falls just outside this week's window. No CVEs surfaced.

**Dagger**
- Latest: **2.60.1** (2026-07-06). No new release, breaking changes, or CVEs this window.

**Jackson**
- Latest: **3.2.1** (3.x line), **2.22.1** (2.x line). Not within the past week.
- **Security (unchanged, still the top action item):** CVE-2026-54512 (critical RCE bypassing `PolymorphicTypeValidator`), CVE-2026-54515 and CVE-2026-59889 (privilege escalation via `@JsonIgnore`/`@JsonView` bypass) — fixed in ≥2.18.9/2.21.5/2.22.1/3.1.5/3.2.1. Upgrade immediately if not already there.

**Log4j**
- Latest active line: **2.26.1** (2026-07-02); actively-maintained **2.25.5** (2026-07-06). No new CVEs this window.

**Guava**
- Latest: **33.6.0** (2026-04-14). No newer release or CVEs found.

**JUnit**
- Latest: **JUnit 6.1.3** (2026-08-07) — **within the past week**. Routine patch release on the JUnit 6.x line (JUnit 5 is maintenance-only).

**Maven**
- Latest stable: **3.9.16** (2026-05-17). **Maven 4.0.0-rc-2** (2026-08-01) is a release candidate just outside the window; Maven 4 is still not GA.

**Gradle**
- Latest: **9.7.0** (2026-08-07) — **within the past week**.
- Isolated Projects graduates experimental→incubating; Configuration Cache gains dependency-resolution API access and Java-agent/TestKit interplay; test-framework startup failures now surface in console output; problem reporting now shows up to 2050 source-located problems (up from 50). No breaking changes beyond the standard Isolated Projects caveat.

---

### Node.js

- **Node.js core**: Latest **v26.7.0** (2026-08-05) — **within the past week**. Adds an OpenSSL STORE loader (load private keys directly from HSMs/cloud KMS), updated root cert store (NSS 3.125), Perfetto tracing support, and `Symbol.dispose` in `ModuleHooks`. Most recent security release remains 2026-07-29 (v26.5.1, v24.18.1 LTS, v22.23.2 LTS — HTTP/2, DNS, permissions, and compression-utility fixes), just outside the window but still the operative security baseline.
- **axios**: Latest **v1.19.0** (~2026-07-28), still outside the window and still the current latest. No new CVEs this week; standing advisories are CVE-2026-40175 (Critical, SSRF→RCE) and CVE-2026-25639 (DoS), plus the March 2026 supply-chain compromise of 1.14.1/0.30.4 — verify installed versions.
- **bcryptjs**: Latest **v3.0.3** (~Nov 2025). Not within the past week; no CVEs found.
- **express**: 4.x latest **v4.22.2** (2026-05-11); 5.x stable **v5.2.1** (2025-12-01). Not within the past week; no new CVEs.
- **moment**: Latest **v2.30.1**, unchanged for ~3 years. Maintenance-mode project; recommends date-fns/Luxon/Temporal for new work.
- **pg (node-postgres)**: Latest **v8.23.0** (~2026-08-09) — **within the past week**. Query pipelining (send multiple queries before waiting on responses) remains the headline throughput feature. No CVEs.
- **sequelize**: Latest stable **v6.37.8** (2026-03-07); v7 still alpha. **New this window: CVE-2026-69240** (disclosed 2026-08-03, CVSS 9.8 Critical) — SQL injection via the Oracle dialect's string-escaping logic when input begins with `TO_TIMESTAMP`/`TO_DATE`. Fixed since 6.37.4; confirm Oracle-dialect users are current.
- **socket.io**: Server latest **v4.8.3** (Dec 2025). **New this window: CVE-2026-69185** (disclosed 2026-08-03) — zero-attachment memory exhaustion via unbounded binary-attachment buffering; fixed in 3.3.6/3.4.5/4.2.7+ (current latest already includes the fix). Also recall the earlier `ws` dependency bumps for CVE-2026-48779 and CVE-2026-45736.
- **jest**: Latest **v30.4.2** (~2026-05). Jest 30 drops Node 14/16/19/21 support (min Node 18.x) and requires TypeScript ≥5.4 — a breaking change from Jest 29 if upgrading.
- **prettier**: Latest **v3.9.6** (~2026-07-21). New official `@prettier/plugin-oxc` (Rust-based OXC parser) for faster formatting. No CVEs.
- **ReactJS**: Latest **v19.2.8** (2026-07-21). Performance fix only, non-breaking.
- **React Native**: Latest stable **v0.86.2** (~2026-08-01), outside the window. **v0.87.0 is in release-candidate stage** (rc.4 as of early August, not yet GA) and is described as a "big cleanup release" removing/deprecating APIs including `InteractionManager` and `SafeAreaView` — review before adopting once it ships.

---

### Database and Storage

- **Oracle Database**: Latest CPU baseline **23.26.3.0.0 / 21.22.0.0.260721 / 19.32.0.0.260721** (2026-07-21). Not within the past week. **Upcoming: first-ever monthly Critical Security Patch Update on 2026-08-18** (six days after this window closes) — a new, faster cadence layered on top of the quarterly CPU (next full CPU 2026-10-20).
- **PostgreSQL**: Latest **18.4 / 17.10 / 16.14 / 15.18 / 14.23** (2026-05-14). Not within the past week — **the next quarterly release lands 2026-08-13**, one day after this window closes; expect the current CVE batch (CVE-2026-6473/-6475/-6477/-6637 at CVSS 8.8, plus MD5 timing-channel CVE-2026-6478) to be addressed then.
- **Redis**: Latest **8.10.0** (2026-07-29). Not within the past week. Compact-hash encoding, `HIMPORT`, `BACKUP`, and cert-based server-to-server TLS remain the headline features. No new CVEs; the 2026 RCE cluster (CVE-2026-23479, -25243, -25588, -25589, -23631) was patched back in May across all supported lines.
- **Apache Kafka**: Latest **4.3.1** (2026-06-25). Not within the past week. No new CVEs flagged.
- **MongoDB**: Latest **8.3.7** (2026-07-22). Not within the past week. **Security**: ~24 CVEs from the 2026-07-22 coordinated release remain the operative set, including CVE-2026-13072 (CVSS 9.2, memory corruption in Compute Mode) and CVE-2026-13078 (CVSS 7.7, arbitrary file read via a crafted aggregation pipeline through MozJS's module-loading hook). Breaking changes (sharded-cluster DDL restrictions, upsert error-code changes) unchanged from last week.
- **Apache Cassandra**: Latest **5.0.9** (2026-08-07) — **within the past week**. Routine stability/bugfix maintenance patch; no CVEs called out.
- **Elasticsearch**: Latest **9.5.0** (2026-08-04) — just outside the window (one day early). Multi-shard searches batch into a single round trip; Columnar/`logsdb_columnar` index modes in tech preview; native Prometheus/PromQL support and Agent Builder enhancements added. No CVEs specific to 9.5.0.
- **Google BigQuery** (managed, rolling releases): Notable items in the 2026-08-03 to 2026-08-07 range (partially overlapping this window) — cross-cloud connections to AWS/Azure/Salesforce Data 360 from all regions (Preview), JDBC driver GA OpenTelemetry tracing, `APPENDS`/`CHANGES` functions GA for row-level change tracking, Google ODBC driver GA. No breaking changes or CVEs (managed service).

---

### OS and Platform

- **Docker**: Docker Desktop **4.86.0** (2026-08-10) — **within the past week**. **Security: CVE-2026-17106**, a destination-escape flaw in `docker container cp` allowing a crafted copy target to escape the intended path — fixed this release, patch now. (Prior release 4.85.0, 2026-08-03, fixed CVE-2026-8936 grpcfuse VM-panic — just outside this window.)
- **Kubernetes**: Latest **v1.36.3** (2026-07-23), outside window. **v1.37.0** due **2026-08-26**; Kubernetes **1.34 enters maintenance mode 2026-08-27** (EOL 2026-10-27). No dated patch release confirmed inside this window.
- **GCP Compute Engine**: August 2026 — Confidential VM instances on AMD SEV-SNP may see longer boot times/performance changes through November 2026 due to a guest-kernel migration and security update (SEV/TDX unaffected). Hyperdisk Balanced High-Availability max throughput doubled to 2,400 MiB/s; C4D machine series now supports it.
- **GKE**: 2026-08-03 to 08-05 (partially overlapping window) — TPU7x (Ironwood) Dynamic Subslicing reached GA; Arm workload support on Autopilot GA; Preview of AI-generated optimized configs for stateful workloads (Redis/MySQL).
- **GCP Maps API**: Weekly channel already moved to **v3.65 on 2026-08-03** (just outside window); full quarterly channel bump historically carries backwards-incompatible changes — monitor as it completes rollout.
- **GCP Translate API**: Adaptive Translation reached GA (adds Portuguese, raises character limits, lower latency); Romanization/transliteration now in Preview (release notes updated 2026-07-29, outside window). **Standing breaking-change note:** Basic and Advanced ("v3") client libraries remain not cross-compatible.
- **GCP Cloud Storage**: **Upcoming 2026-08-26** — hierarchical-namespace buckets' Object Lifecycle Management Delete action will start deleting empty folders that meet lifecycle-rule conditions (not yet in effect this window). No release inside the window itself.
- **Google Analytics (GA4)**: No release in-window; most recent (2026-07-30) added a diagnostic for aggregate parameter/URL-tracking issues.
- **Amazon S3**: 2026-08-06 — AWS Backup for S3 now supports Access Points for direct read-only access to backup data (**within the past week**). 2026-08-04 — S3 Vectors now available in AWS European Sovereign Cloud (Germany) (just outside window).
- **Amazon EC2**: 2026-08-07 — Amazon GameLift Servers now supports 21 additional EC2 instance types (C8a/C8i/C9g, M8a/M8i/M9g) spanning x86 and Arm (**within the past week**). 2026-08-06 — M8g (Graviton4) regional expansion to Taipei and Mexico Central (**within the past week**). 2026-08-04 — C8g regional expansion to Paris, Cape Town, Tel Aviv, Calgary (just outside window). No new instance families or breaking changes.
- **AWS Lambda**: 2026-08-05 — scalable network bandwidth for non-VPC functions, now scaling with memory from 625 Mbps at 2GB up to 3,000 Mbps at 10GB (**within the past week**).
- **Amazon EKS**: No release in-window. **Action item (now overdue, not just upcoming):** Kubernetes 1.33 standard support on EKS **ended 2026-07-29**, and 1.30 extended support **ended 2026-07-23** — both already past. Clusters still on those versions should upgrade immediately (extended-support surcharges apply).
- **Amazon CloudFront**: No release in-window; most recent (2026-05) — Premium flat-rate plan now supports configurable, self-service usage levels from 500M-6B requests / 50TB-600TB.
- **Amazon Route53**: No release in-window; most recent (2026-05) — 34 new TLDs added to Route 53 Domains (.app, .dev, .health, etc.); (2026-03) Global Resolver reached GA across 30 regions.
- **Cloudflare**: "Agents Week" — 2026-08-05 Cloudflare OS open-sourced (self-hostable agent workspace with zero-trust Gatekeepers); 2026-08-06 Kitesurf, a from-scratch Rust/Wasm browser engine running inside Workers V8 isolates; 2026-08-03 early preview of `@cloudflare/computer` (Durable Object workspace routing agent work across isolates and Linux containers). All within or at the edge of the past week; additive Workers/AI platform features, no breaking changes or CVEs. Cloudflare for Government also achieved FedRAMP Class D (High) Certified status this period.
- **Firebase**: August 2026 SDK releases — C++ SDK 13.11.0, JS SDK 12.17.1, CLI 15.25.0 (bumps Firestore emulator, adds App Check debug-token commands), Unity SDK 13.15.0. Realtime Database bug fixes across SDKs.
- **PayPal**: A developer-focused event ran 2026-08-04 to 08-06 (within the window), though no concrete API changelog entries were found for it. Most recent substantive changelog (2026-07-01, outside window) added APMs (Twint, Bizum, PayU, Skrill) for Enterprise Payments orchestration and Visa Account Funding Transactions support.
- **Apple Pay**: iOS 27's Wallet/Apple Pay redesign continues rolling out in beta (swipe-between-cards checkout, grid card view, new Amex-rewards-points payment partnership, "Tap to Share" in-store feature launching this fall, later-2026 debit-card reloading in Wallet). UI/feature additions, no breaking changes for existing merchant integrations, but test checkout flows against the iOS 27 beta before GA this fall.

---

## Caveats
- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the past week" is judged against the 2026-08-05 to 2026-08-12 window; where a tool's most recent release predates this window, that release is still reported as the current latest. Several items (e.g. Netty 4.2.17.Final, Elasticsearch 9.5.0, MySQL Connector/J 26.7.0) fall just one to two days outside the window edge and are flagged as such rather than omitted, since they are the most recent available data point.
- Some dates surfaced by automated web-page summarization tools were internally inconsistent (e.g. showing a wrong year on relative-date GitHub release pages); where a date looked suspect it was cross-checked against a second source or the vendor's own dated blog/changelog before being included here.
- Managed cloud services (BigQuery, GCP/AWS features, Cloudflare, Firebase, PayPal, Apple Pay) don't version the same way as OSS libraries — entries reflect the most recent dated announcements found, not a single canonical "latest version."
- The EKS/Kubernetes 1.33 and 1.30 support-window dates in this report (ended 2026-07-29 and 2026-07-23 respectively) were verified directly and differ from a similar note in last week's report (which cited 2026-08-31); treat this week's dates as the corrected figures.
