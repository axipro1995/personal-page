# Weekly Tech Update Report — Week Ending August 27, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-08-20 to 2026-08-27**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, and vendor advisories.

## TL;DR

- **Released this week:** A coordinated Spring-portfolio wave (Boot 4.1.1/4.0.8, Cloud 2025.1.3 "Oakwood", Spring AI 2.0.1, 2026-08-20/21) tied to the ~91-CVE disclosure from last week's window boundary, Hibernate 7.4.6.Final/6.6.56.Final (2026-08-23), Protobuf v36.0 GA and Gradle 9.7.1 (both 2026-08-20), Elasticsearch 9.5.2 (2026-08-20), Node.js 26.8.0/24.20.0 (2026-08-26, feature-only), axios 1.20.0 (2026-08-24/26), React Native 0.87.1/0.86.3 (2026-08-24/26), Docker Desktop 4.88.0/4.88.1 (2026-08-24/25), Kubernetes v1.37.0 "Garhwal" (2026-08-26), Kubernetes 1.34 entering maintenance mode (2026-08-27), and a cluster of AWS/GCP/Cloudflare/Firebase platform updates detailed below.
- **Patch now — critical security:**
  - **Spring Security CVE-2026-59270 (CVSS 9.4, Critical)** — the embedded UnboundID LDAP test server exposes a well-known admin bind DN on all interfaces, letting an attacker authenticate and read/modify in-memory directory entries. Check any dev/test configs using this embedded server and upgrade immediately.
  - **Spring for GraphQL CVE-2026-59285 (CVSS 9.2, Critical)** — unsafe deserialization on paginated queries when Jackson 2.x is used for JSON binding; likely the same issue family as June's CVE-2026-41699. Confirm you're on Spring for GraphQL ≥2.0.4/1.4.6 or later.
  - **BigQuery Data Transfer Service — CVE-2026-12717 (CVSS v4.0 9.40, Critical)**, disclosed via bulletin **GCP-2026-056 (2026-08-26)**: improper input validation in the bundled CData JDBC driver (pre-2026-05-01) allowed an authenticated attacker RCE in the connector container and tenant-project privilege escalation. The underlying flaw was already patched 2026-05-01 — this is a late public disclosure, no customer action required, but worth confirming your DTS connector predates that fix.
  - **Docker Desktop 4.88.0 (2026-08-24)** fixes **CVE-2026-17106** — a `docker container cp` destination-escape flaw letting a malicious container create/overwrite arbitrary host files, potentially achieving code execution as the Docker-invoking user. Upgrade to 4.88.1.
  - **Redis — newly detailed CVEs from the 2026-08-17 coordinated release** (just outside this window but not previously itemized): **CVE-2026-23479** (use-after-free in unblock-client flow, CVSS 7.7 High, possible RCE), **CVE-2026-23631** (Lua use-after-free in master-replica sync, CVSS 8.1 High), **CVE-2026-25243** (invalid-memory-access in `RESTORE` via crafted payload, CVSS 7.7 High) — all fixed in 8.10.1/8.8.2/8.6.6/8.4.6/8.2.9/7.4.11/7.2.16/6.2.24; confirm you're on those floors.
  - **Netty CVE-2026-62243 (CVSS 7.5, High)** — TLS hostname verification disabled on the OpenSSL client path under certain trust-manager conditions; already fixed in 4.1.137.Final/4.2.17.Final, disclosed just before this window.
  - **GCP Compute Engine — CVE-2026-6726 (CVSS ≈8.5)**, tracked under bulletin **GCP-2026-054**: TPM 2.0 reference-code out-of-bounds read on AMD Ryzen/EPYC-based Confidential/Shielded VM guests — info-leak/side-channel risk.
  - **Cloudflare WAF (2026-08-25 release)** added a new Generic RCE detection for **Apache ActiveMQ (CVE-2026-34197)** and moved four other detections from Log to Block mode — verify no false positives before the mode switch takes broad effect.
- **Breaking-change watch:**
  - **GCP Cloud Storage — confirmed live 2026-08-26:** in hierarchical-namespace-enabled buckets, the Object Lifecycle Management Delete action now also deletes empty folders once rule conditions are met. Audit lifecycle rules on HNS buckets to avoid unintended folder deletion — this closes out the item flagged as "not yet occurred" last week.
  - **Amazon EKS CA rotation (announced 2026-08-20):** clusters running since 2018 have 10-year CAs nearing rotation; EKS auto-manages the trust update, but **customers must replace worker nodes and update external clients to trust the successor CA before activation** — concrete action item for long-lived clusters. Separately, **EKS Kubernetes 1.33 standard support and 1.30 extended support both end 2026-08-31** (just after this window) — finalize upgrade plans now.
  - **Kubernetes v1.37.0 "Garhwal" (2026-08-26)** shipped 67 enhancements (16 Stable/23 Beta/27 Alpha, 1 removal); **1.34 entered maintenance mode 2026-08-27** (EOL 2026-10-27, critical-fixes-only from here). Carried-forward breaking items: containerd 2.0+ required from 1.35 on; static Pods can no longer reference Secrets/ConfigMaps.
  - **Google BigQuery** — forward-looking deprecation announced 2026-08-20: Graph processing will be restricted to Enterprise/Enterprise Plus editions effective **2027-04-26**; teams on Standard edition/on-demand billing using BigQuery Graph should plan a migration or edition upgrade well ahead of that date.
  - **Firebase Apple SDK v12.18.0 (2026-08-24)** removed deprecated Imagen methods (Imagen models shut down in August 2026; migrate to Gemini "Nano Banana") and reiterates that the Firebase Apple SDK **stops publishing to CocoaPods after October 2026** — teams still on CocoaPods should plan an SPM migration now.
  - **axios 1.20.0** adds `ContentTooLarge`/`UnprocessableContent` status-code names alongside the old `PayloadTooLarge`/`UnprocessableEntity` aliases (non-breaking today, but code referencing the old names should migrate ahead of an eventual removal).

---

### Java

**Window checked:** 2026-08-20 to 2026-08-27. Hibernate, Protobuf, Gradle, and the Spring portfolio shipped inside this exact window; other tools' latest stable falls earlier and is reported as such.

**Java (core / JDK)**
- No release inside the window. Oracle's monthly CSPU landed **2026-08-18**, two days before the window opens (already covered last week): 19 Java SE patches, 17 remotely exploitable without authentication, highest Java-SE CVSS 7.5. No new Java-specific patch news 08-20→08-27.

**Netty**
- No new release this window; latest remains **4.1.137.Final / 4.2.17.Final** (2026-08-04/06). **CVE-2026-62243** (CVSS 7.5, High) — TLS hostname verification disabled on the OpenSSL client path under certain trust-manager conditions — was disclosed just before the window and is already fixed in these versions. Also fixed in the same line: CVE-2026-59903 (Vary-header CORS cache-poisoning, covered last week) and CVE-2026-62380 (SOCKS4/5 client encoder injection).
- No breaking changes.

**Vert.x**
- No new release this window. Latest remains **5.1.6 / 4.5.32** (2026-08-07/08). No new CVEs.

**Spring (Framework / Boot / Cloud / AI)**
- **Within the window:** **Spring Boot 4.1.1 / 4.0.8**, **Spring Boot 4.2.0-M1**, **Spring Cloud 2025.1.3 "Oakwood"**, **Spring Integration 7.2.0-M1**, **Spring AMQP 4.2.0-M1**, **Spring Batch 6.0.5/6.1.0-M1**, **Spring Data 2026.1.0-M1/2026.0.1/2025.1.7** — all **2026-08-20** — plus **Spring AI 2.0.1 (2026-08-21)**.
- These releases carry the fixes for the **~91-CVE Spring-portfolio disclosure (2026-08-20, in-window)** flagged at a high level last week; notable named CVEs confirmed this window: **CVE-2026-59270** (Spring Security embedded UnboundID LDAP admin-DN exposure, CVSS 9.4 Critical), **CVE-2026-59285** (Spring for GraphQL unsafe deserialization → possible RCE, CVSS 9.2 Critical, likely the same family as June's CVE-2026-41699), plus Spring Cloud 2025.1.3's fixes for CVE-2026-47837/-47894/-59315/-59291/-59297/-59298/-59299/-59300/-59301, and over a dozen additional High-severity CVEs across Spring Security, Spring Data REST, Spring Integration, Reactor Core/Netty, Spring AMQP, Spring Batch, and Spring AI (XSS, info disclosure, RCE, DoS, auth bypass).
- Follow-up "This Week in Spring" (2026-08-25) reported no additional CVEs or advisories since the 08-20 wave.
- No new breaking changes beyond the Boot 4.0 baseline already carried forward (Jackson 3 default, Spring Security 7 CSRF-default changes, Undertow dropped, JSpecify null-safety annotations).

**Hibernate**
- **Within the window:** **7.4.6.Final / 6.6.56.Final (2026-08-23)** — routine patch release, no CVEs, no breaking changes. 8.0 remains in Beta.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06) — outside the window, no new release or CVE. Standing security history unchanged: CVE-2026-54291 (SCRAM channel-binding downgrade, fixed 42.7.12) and CVE-2026-42198 (client-side DoS via malicious SCRAM auth response, fixed 42.7.11).
- **MySQL Connector/J**: latest **9.7.0** (2026-04-22) — outside the window; no new release or CVE found.

**Protobuf**
- **Within the window:** **v36.0 GA** shipped **2026-08-20** (preceded by rc2 08-03, rc1 07-09). No CVEs. Deprecation warnings for generic service options; improved Edition 2026 support (carried-forward breaking notes: `sun.misc.Unsafe` removal from Java codegen, Bazel 7 support dropped in favor of Bazel 9+).

**Dagger**
- No new release. Latest remains **2.60.1** (2026-07-06). No CVEs or breaking changes.

**Jackson**
- No new release this window. Standing critical set unchanged and still the top outstanding Java CVE risk for laggards: **CVE-2026-54512** (CVSS 8.1, PolymorphicTypeValidator bypass), **CVE-2026-54513/-54514** (sibling PTV/DNS-resolution bypasses), **CVE-2026-54515** (case-insensitive deserialization reopens `@JsonIgnoreProperties`-protected fields), **CVE-2026-59889** (CVSS 6.5, `@JsonView` bypass via `@JsonUnwrapped`). Confirm ≥2.18.9/2.21.5/2.22.1/3.1.5/3.2.1.

**Log4j**
- No release inside the window. Latest remains **2.26.1** (2026-07-02). No new CVEs.

**Guava**
- No release inside the window. Latest remains **33.6.0** (2026-04-14, per this week's research — note this differs from last week's reported 33.7.0/33.7.1; treat 33.6.0/33.7.1 both as recent-stable claims pending reconciliation and confirm against google/guava's release page directly before acting).

**JUnit**
- No new release. Latest remains **JUnit 6.1.3** (2026-08-07). No CVEs. Carried-forward JUnit 5→6 migration notes: Java 17 minimum, Kotlin 2.2+, several removed modules/APIs, CSV engine switched to FastCSV.

**Maven**
- No new GA release. Latest stable **3.9.16** (2026-05-13); pre-release **4.0.0-rc-6** (2026-08-04, still not GA, requires Java 17). No CVEs.

**Gradle**
- **Within the window:** **9.7.1 (2026-08-20)**, first patch release for 9.7.0. No CVEs. Notable 9.7-line changes: Isolated Projects graduates to incubating; Kotlin DSL can no longer import relocated `org.gradle.internal.impldep.*` classes; embedded Kotlin moves to 2.4.0 (drops Kotlin 1.9 language-version support).

---

### Node.js

**Window checked:** 2026-08-20 to 2026-08-27. Node.js core, axios, and React Native shipped inside this exact window; the rest of the stack was quiet.

- **Node.js core**: **Within the window** — Current **v26.8.0** and LTS **v24.20.0 "Krypton"** both released **2026-08-26**. Neither is a security release (feature-only: Cipher/Decipher SIV & GCM-SIV modes, RSA-OAEP `mgf1Hash`, zlib `ZipEntry/ZipFile/ZipBuffer`, stabilized `TracingChannel` on 26.8.0; `AsyncLocalStorage` `using` scopes, `permission.drop()`, package maps, `node:stream/iter`, JSPI support on 24.20.0). No breaking changes. Confirm the July 29 security release (22.23.2/24.18.1/26.5.1 — Permission Model bypass, HTTPS Agent mTLS/session-reuse issues, plus DNS/SQLite/zlib fixes) is already applied; it remains the most recent security baseline.
- **axios**: **Within the window** — **v1.20.0** (GitHub release 2026-08-24, npm publish 2026-08-26). No new CVE, but proactive hardening against shared/foreign prototype pollution in behavioral-config reads and interceptor-replacement objects. Adds `ContentTooLarge`/`UnprocessableContent` status names (old aliases retained). Also fixes an interceptor handler-array leak and a Node socket memory leak.
- **bcryptjs**: Latest **v3.0.3** (2025-11-02) — outside the window, ~10 months with no release. No CVEs.
- **express**: 5.x latest **v5.2.1** (2025-12-01); 4.x latest **v4.22.2** (2026-05-11) — both outside the window, no release this week. **CVE-2026-12590** (body-parser, Low, disclosed 2026-07-31, outside window) — invalid `limit` option silently disabled body-size enforcement (DoS); affects body-parser <1.20.6 (Express 4.x) and >=2.0.0 <2.3.0 (Express 5.x), fixed in 1.20.6/2.3.0. Confirm your body-parser floor.
- **moment**: Latest **v2.30.1** (2023-12-27) — no change. Maintenance-mode; continue migration off it.
- **pg (node-postgres)**: Latest **v8.23.0** (2026-08-08) — outside the window. No CVEs, no breaking changes.
- **sequelize**: v6 latest **v6.37.8** (2026-03-07); v7 remains alpha (2026-02-04) — both outside the window. **CVE-2026-30951** (SQL injection via unescaped JSON/JSONB cast-type in `_traverseJSON()`, affects <6.37.8, fixed 6.37.8, v7 unaffected) — outside window but a good prompt to confirm you're past 6.37.8 if not already applied.
- **socket.io**: Server latest **v4.8.3** (2026-03-18); parser latest **v4.2.7/v3.3.6** (2026-07-16) — both outside the window. **CVE-2026-69185** (CVSS 7.5 High, zero-attachment memory-exhaustion DoS, disclosed 2026-08-03, fixed in parser 3.3.6/3.4.5/4.2.7) appears to be a follow-up hardening of the earlier CVE-2026-33151 (March 2026) — confirm `socket.io-parser` resolves to ≥4.2.7.
- **jest**: Latest **v30.4.2** (2026-05-09) — outside the window, no CVEs, no release this week.
- **prettier**: Latest **v3.9.6** (2026-07-21) — outside the window, no CVEs, no release this week.
- **ReactJS**: Latest **v19.2.8** (2026-07-21) — outside the window, no new CVEs. RSC-specific DoS CVEs (CVE-2026-23864/-23870, affecting `react-server-dom-*` packages) remain relevant only if using RSC/Next.js Server Actions.
- **React Native**: **Within the window** — **v0.87.1 (2026-08-26)**: SwiftPM autolinking now hard-fails instead of silently misconfiguring, Hermes bumped, TypeScript typing fixes for `Animated`, several iOS/SwiftPM bug fixes. **v0.86.3 (2026-08-24)**: Hermes updates plus Modal/EventEmitter/font-rendering fixes. Both are routine patch releases on top of last week's 0.87.0 GA — no security fixes, no breaking changes.

---

### Database and Storage

- **Oracle Database**: No release inside the window. August CSPU shipped **2026-08-18** (covered last week); no follow-up out-of-band Security Alert found in-window. Next CSPU 2026-09-15; next quarterly CPU 2026-10-20.
- **PostgreSQL**: No release inside the window. Latest remains **18.6/17.11/16.15/15.19/14.24** (2026-08-13, covered last week). No new CVEs; only minor community-tooling news landed in-window (pg_statviz 1.2, LibreDB Studio).
- **Redis**: No release inside the window. Latest remains **8.10.1** (2026-08-17, covered last week for CVE-2026-62356), but this week's research surfaced additional CVEs from the same coordinated release not previously itemized: **CVE-2026-23479** (use-after-free in unblock-client flow, CVSS 7.7 High, possible RCE), **CVE-2026-23631** (Lua use-after-free in master-replica sync, CVSS 8.1 High/6.1 Medium depending on scale, possible RCE), **CVE-2026-25243** (invalid memory access in `RESTORE` via crafted payload, CVSS 7.7 High, possible RCE) — plus ACL key-permission bypasses and TLS UAF/cert-auth-bypass fixes. All fixed in the 8.10.1/8.8.2/8.6.6/8.4.6/8.2.9/7.4.11/7.2.16/6.2.24 train — confirm you're current.
- **Apache Kafka**: No release inside the window. Latest remains **4.3.1** (2026-06-25). No new CVEs.
- **MongoDB**: No release inside the window. The 27-vulnerability disclosure (2026-08-11, covered last week — led by CVE-2026-18691, CVSS 9.0) remains the operative advisory; no follow-up bulletin or new CVE found 08-20→08-27.
- **Apache Cassandra**: No release inside the window. Latest remains **5.0.9** (2026-08-07). No new CVEs.
- **Elasticsearch**: **Within the window** — **9.5.2 (2026-08-20)** — reliability/security updates across search, ES|QL, vector search, inference, snapshot-restore, transforms/ML; no specific new CVE ID surfaced for 9.5.2 in available sources (check Elastic's security-advisories page directly for compliance reporting). Notable behavior change: `search.batched_query_phase` now enabled by default (multi-shard queries batched into a single round-trip with partial reductions on data nodes) — review if you rely on per-shard round-trip timing/metrics. Async reindex operations now survive graceful node shutdown. 8.x line latest remains **8.19.20** (2026-08-11), outside the window.
- **Google BigQuery** (managed, rolling releases): **Within the window** — **2026-08-26**: security bulletin **GCP-2026-056**, **CVE-2026-12717** (CVSS v4.0 9.40, Critical) in the CData JDBC driver bundled with BigQuery Data Transfer Service — already patched 2026-05-01, this is a late disclosure, no action needed if you predate that fix. **2026-08-25**: monitoring added for data agents/conversations. **2026-08-24**: data-governance tags now available in Terraform (column-level security/masking as IaC). **2026-08-20**: three new time-series ML TVFs (`ML.TREND`/`ML.SEASONALITY`/`ML.DETECT_CHANGE_POINTS`, Preview); deprecation notice that BigQuery Graph processing will be restricted to Enterprise/Enterprise Plus editions effective **2027-04-26** — plan ahead if on Standard edition/on-demand billing.

---

### OS and Platform

- **Docker**: **Within the window** — Docker Desktop **4.88.0 (2026-08-24)** and **4.88.1 (2026-08-25)**. 4.88.0 fixes **CVE-2026-17106** (`docker container cp` destination-escape → arbitrary host file creation/overwrite, potential code execution as the invoking user). Carried-forward breaking notes: `docker sbom` deprecated in favor of `docker scout sbom`; QEMU backend removed on macOS in favor of Apple Virtualization.
- **Kubernetes**: **Within the window** — **v1.37.0 "Garhwal" (2026-08-26)**, 67 enhancements (16 Stable, 23 Beta, 27 Alpha, 1 deprecation/removal). **1.34 entered maintenance mode 2026-08-27** (EOL 2026-10-27, critical-fixes-only from here). Carried-forward breaking items: containerd 2.0+ required since 1.35 (last release to support containerd 1.x); static Pods can no longer reference Secrets/ConfigMaps; `kubectl run --filename` deprecated. Related CVEs worth confirming patched regardless of exact disclosure date: ingress-nginx CVE-2026-1580/-24512/-24513/-24514 (up to CVSS 8.8, fixed in v1.13.7/v1.14.3+); CSI driver path-traversal CVE-2026-3864 (NFS)/CVE-2026-3865 (SMB).
- **GCP Compute Engine**: **Within the window** — security bulletin **GCP-2026-054**, **CVE-2026-6726** (CVSS ≈8.5) — TPM 2.0 reference-code out-of-bounds read on AMD Ryzen/EPYC-based Confidential/Shielded VM guests (info-leak/side-channel). No confirmed dated feature release strictly in-window; Debian 11 Batch-image EOS (2026-08-31) and Confidential VM (AMD SEV-SNP) guest-kernel migration remain open items just outside/spanning the window.
- **GKE**: **Within the window** — 2026-R35 rollout (2026-08-20): Rapid default 1.36.3-gke.1537000, Regular default 1.35.6-gke.1710000, Stable adds 1.34.9-gke.1322001/1610001, Extended default 1.35.6-gke.1710000, several versions on 90-day deprecation clocks. 2026-08-21: no-channel cluster creation now restricted to existing customers only (new customers must pick a release channel); Windows Server 2019 (LTSC) node image enters security-patch-only phase — migrate to Windows Server 2022 LTSC. 2026-08-25: fixed a GPUDirect-TCPX/a3-highgpu-8g compatibility block on 1.34/1.35+ (requires installer ≥3.1.9, sidecar ≥2.0.12 before upgrading). The 15GB node-pool boot-disk minimum (1.36.3-gke.1480000+) remains dated 2026-08-18, just outside the window, as flagged last week.
- **GCP Maps API**: No release inside the window. Latest **v3.66.1a** (2026-08-17, accessibility fix) — outside the window.
- **GCP Translate API**: No release inside the window; page last updated 2026-07-29. Standing note unchanged: recommend migrating off `www.googleapis.com` to `translation.googleapis.com`.
- **GCP Cloud Storage**: **Within the window and breaking** — **2026-08-26**: the previously-flagged Object Lifecycle Management change **has now occurred** — in hierarchical-namespace-enabled buckets, the Delete lifecycle action now also deletes empty folders once rule conditions are met; audit lifecycle rules on HNS buckets. **2026-08-21**: Rapid Cache now supports prefix-level ingest-on-write filtering. No new CVEs.
- **Google Analytics (GA4)**: No release inside the window; most recent item (custom integer lookback windows) dated 2026-08-11, outside window.
- **Amazon S3**: No confirmed in-window announcement. Most recent items (Backup direct-access via Access Points, S3 Tables Variant type) fall in late July/early August, outside window.
- **Amazon EC2**: Within the window (borderline) — 2026-08-20: P6-B300 (8x NVIDIA Blackwell Ultra GPU) instances now available in Asia Pacific (Seoul); R8a instances expanded to Asia Pacific (Taipei), dated just before/at window start. Routine regional expansion; no CVEs or breaking changes.
- **AWS Lambda**: **Within the window** — **2026-08-25**: Lambda functions now support full IAM resource-based policies (multiple principals/actions, full condition-key support in one policy document) — review existing simplified resource policies for consolidation opportunities in cross-account/fine-grained invoke permissions. No new CVEs.
- **Amazon EKS**: **Within the window and notable** — **2026-08-20**: EKS Certificate Authority rotation with automated lifecycle management announced; EKS manages the AWS-side trust update, but **customers must replace worker nodes and update external clients to trust the successor CA before activation** on clusters running since 2018. **Action item, deadline just outside this window:** Kubernetes 1.33 standard support and 1.30 extended support on EKS both end **2026-08-31** — finalize upgrades this week if not already done.
- **Amazon CloudFront**: **Within the window** — **2026-08-20**: native Origin Access Control (OAC) support for S3 Multi-Region Access Points (MRAP), with CloudFront auto-signing SigV4a requests, removing the need for a custom Lambda@Edge signing function. No CVEs.
- **Amazon Route53**: No announcement found for this window.
- **Cloudflare**: **Within the window** — WAF release **2026-08-25**: moved four detections from Log to Block mode (verify no false positives before broad enforcement), merged the XSS/HTML-Injection-Script-Tag-Beta rule into the base rule, and added a new Generic RCE detection for **Apache ActiveMQ (CVE-2026-34197)** plus an updated Magento 2 unrestricted-file-upload signature. Cloudflare One added stateless MCP (2026-07-28 protocol) support for server portal connections, auto-compatible with 2025 Streamable HTTP clients.
- **Firebase**: **Within the window** — Firebase Apple SDK **v12.18.0 (2026-08-24)**: removed deprecated Imagen methods/types (Imagen models shut down in August 2026, migrate to Gemini "Nano Banana"), added `RealtimeInputConfig` for the Live API, fixed a race condition causing missed `session_start` events. Reiterates: Firebase Apple SDK **stops publishing to CocoaPods after October 2026** — plan an SPM migration if still on CocoaPods. No CVEs.
- **PayPal**: No confirmed in-window release. Latest **Checkout Components v5.0.427** (2026-08-14) — outside the window.
- **Apple Pay**: **Within the window** — 2026-08-26: reporting on an iOS 27 beta tweak to the Wallet U.S. passport/Digital ID feature. iOS 27 developer beta 6 (2026-08-17) remains the most recent confirmed build, outside the window; GA still expected September 2026 — validate PassKit/Apple Pay integrations against the beta SDK ahead of GA, especially the redesigned swipeable-card checkout flow flagged last week.

---

## Caveats
- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the window" is judged against 2026-08-20 to 2026-08-27. Several items are reported as flagged-but-outside (e.g. Redis's 2026-08-17 coordinated release, MongoDB's 2026-08-11 disclosure) because they remain the most recent operative security guidance even though they landed just before the window opened.
- A data conflict surfaced on **Guava**: last week's report listed 33.7.0/33.7.1 (2026-08-17/18) as the latest; this week's research instead surfaced 33.6.0 (2026-04-14) as latest with no record of 33.7.x. Treat this as unresolved and confirm the actual latest release directly against google/guava's GitHub releases page before reporting a version floor.
- This report was compiled by four parallel research passes (Java; Node.js; Database/Storage; OS/Platform) that were then merged into this single document — cross-tool overlaps (e.g. Spring/Jackson/Netty CVEs referenced from multiple angles) were reconciled but minor phrasing duplication may remain.
- This report closes out the GCS "watch next window" item flagged in the 2026-08-20 report: the Object Lifecycle Management empty-folder-deletion change has now occurred (2026-08-26), exactly as forecast. The EKS 1.33/1.30 support-window action item (now with a firm 2026-08-31 deadline) and the BigQuery Graph edition-restriction deprecation (2027-04-26) should both be carried forward to next week's report as open items.
