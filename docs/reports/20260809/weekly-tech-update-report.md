# Weekly Tech Update Report — Week Ending August 9, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-08-02 to 2026-08-09**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, and vendor advisories.

## TL;DR

- **Released this week:** Node.js 26.7.0, pg (node-postgres) 8.23.0, Netty 4.2.17.Final/4.1.137.Final, Vert.x 5.1.6/4.5.32, JUnit 6.1.3, Gradle 9.7.0, Apache Cassandra 5.0.9, Elasticsearch 9.5.0, Google BigQuery (rolling features), Docker Desktop 4.85.0, and axios 1.19.0 (edge of window).
- **Patch now — critical security:**
  - **PostgreSQL** — 11 CVEs fixed in the May release still current until 2026-08-13's next quarterly patch; includes CVSS 8.8 stack-overflow/buffer-overflow/SQL-injection issues (CVE-2026-6473, -6475, -6477, -6637) and an MD5 timing side-channel (CVE-2026-6478).
  - **Oracle Database** — July 21 CPU addressed **1,235 CVEs** (largest CPU ever), max CVSS 9.9, including unauthenticated remotely exploitable TimesTen issues.
  - **MongoDB 8.3.7** — 24 CVEs fixed (CVE-2026-9737, CVE-2026-13055–13078); SCRAM-SHA-1 now incompatible with FIPS mode.
  - **Jackson (all lines)** — CVE-2026-54512 (insecure deserialization), CVE-2026-54515 and CVE-2026-59889 (privilege escalation via `@JsonIgnore`/`@JsonView` bypass) — upgrade to ≥2.18.9 / 2.21.5 / 2.22.1 / 3.1.5 / 3.2.1 immediately.
  - **Spring Framework 7.0.8 / 6.2.19** — 9 CVEs fixed (CVE-2026-41838–41846), including session fixation, WebSocket session-ID predictability, and unsafe Jackson JMS deserialization.
  - **axios** — CVE-2026-40175 (Critical, CVSS 9.9, SSRF→RCE gadget chain) and CVE-2026-25639 (DoS); also note the March 2026 supply-chain compromise of versions 1.14.1/0.30.4 — verify installed versions.
  - **pgjdbc (PostgreSQL JDBC)** — CVE-2026-54291: driver silently downgraded SCRAM channel binding, stripping a protection meant to prevent MITM; fixed in 42.7.12+.
  - **Docker Desktop 4.85.0** — CVE-2026-8936, VM panic via unbounded recursion in `grpcfuse` kernel module, fixed this release.
  - **Sequelize 6.37.8** — CVE-2026-30951, JSON where-clause cast-type validation to prevent injection-style abuse.
- **Breaking-change watch:** Spring Boot 4.0 baseline (Jackson 3 default, Spring Security 7 defaults can silently break REST APIs, Undertow dropped); MongoDB 8.3 sharded-cluster DDL/upsert-error-code changes; GCP Translation API Basic/Advanced client libraries are **not** cross-compatible; Kubernetes 1.33/1.30 support windows close 2026-08-31 (EKS clusters on those versions need an upgrade plan now).

---

### Java

**Window checked:** 2026-08-02 to 2026-08-09. Netty, Vert.x, JUnit, and Gradle shipped releases inside this exact window; other tools' latest stable falls earlier and is reported as such.

**Java (core / JDK)**
- Latest: **JDK 25.0.4** (LTS) and **JDK 26.0.2** (current) — both released 2026-07-21 as part of Oracle's quarterly CPU. Not within the past week; next update targets 2026-10-20.
- Security: bundled in Oracle's July 2026 CPU (see Database section — 1,235 CVEs addressed CPU-wide).
- No breaking changes (routine patch releases).

**Netty**
- Latest: **4.2.17.Final** (2026-08-04) and **4.1.137.Final** (2026-08-06) — **within the past week**.
- Bug-fix releases: buffer-leak fixes in `SslHandler`/`HttpObjectEncoder`, new pluggable decompressor API (Bzip2/Snappy/Zstd/Brotli/Lz4Frame), io_uring `SO_INQ` support, WebSocket handshake routing fix. No new CVEs in these two releases.
- Prior security release **4.2.15.Final** (2026-06-02) fixed ~20 CVEs (memory exhaustion, DNS cache poisoning, HTTP/2 DDoS, TLS hostname bypass) — upgrade is critical if still below 4.2.15/4.1.135.

**Vert.x**
- Latest: **5.1.6** (2026-08-05) and **4.5.32** (2026-08-07) — **within the past week**.
- Both bump bundled Netty to 4.2.17.Final, inheriting its fixes, plus general bug fixes. No breaking changes.

**Spring (Framework / Boot)**
- Framework latest: **7.0.8 / 6.2.19** (2026-06-08). Boot latest: **4.1.0** GA (2026-06-10), **3.5.16** (2026-06-25) on the 3.5.x line. Not within the past week.
- Security: 7.0.8/6.2.19 fixed **CVE-2026-41838 through -41846** (session-ID predictability in WebSocket, session fixation in WebFlux, DoS via multipart requests, cache info disclosure, DoS/path-traversal via versioned static resources, open redirect, XSS) plus **CVE-2026-41855** (unsafe deserialization via Jackson JMS converters). Affects 7.0.0–7.0.7, 6.2.0–6.2.18, 6.1.0–6.1.27, 5.3.0–5.3.48.
- Boot 4.1 adds gRPC server/client support, unified Jackson auto-config across JSON/XML/CBOR. Boot 4.0 baseline carries bigger breaking changes: modularized packages, Jackson 3 default, Spring Security 7 defaults (CSRF changes can silently break REST APIs), Undertow dropped, JSpecify null-safety annotations.

**Hibernate**
- Latest: **7.4.5.Final** (2026-07-12); maintenance releases 7.3.13/7.2.24/7.1.35/6.6.55 shipped 2026-08-02 (edge of window, maintenance branches only). Next-gen **8.0.0.Beta1** (2026-06-16) targets Jakarta Persistence 4.0.
- No CVEs surfaced this window.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06); **42.7.12** (2026-06-29) fixed **CVE-2026-54291** — driver silently downgraded `SCRAM-SHA-256-PLUS` (channel binding) to plain SCRAM-SHA-256 for Ed25519/Ed448/post-quantum cert signatures, stripping MITM protection. Upgrade if channel binding matters to you.
- **MySQL Connector/J**: latest **26.7.0** (2026-07-28); versioning scheme changed (no longer tracks 9.x).

**Protobuf**
- Latest stable: **v35.1** (2026-06-11); **v36.0-rc2** (2026-08-03) is a release candidate, not GA. "Edition 2026" shipped 2026-07-13. No CVEs surfaced.

**Dagger**
- Latest: **2.60.1** (2026-07-06). Incremental KSP/Kotlin processor fixes only; no breaking changes or CVEs.

**Jackson**
- Latest: **3.2.1** (jackson-databind/core, 3.x line), **2.22.1** (2.x line). Not within the past week.
- **Security: CVE-2026-54512** (high-severity insecure deserialization, ≤2.18.7, fixed 2.18.8/2.21.4/3.1.4), **CVE-2026-54515** (privilege escalation — ignored `@JsonIgnore` becomes writable via case-insensitive binding, fixed 2.18.9/2.21.5/3.1.4+), **CVE-2026-59889** (privilege escalation bypassing `@JsonView`, fixed 2.18.9/2.21.5/2.22.1/3.1.5/3.2.1). **Upgrade immediately if below these versions.**

**Log4j**
- Latest active line: **2.26.1** (2026-07-02); actively-maintained **2.25.5** (2026-07-06). 2.24.x is EOL. No new CVEs this window.

**Guava**
- Latest: **33.6.0** (2026-04-14). No newer release or CVEs found.

**JUnit**
- Latest: **JUnit 6.1.3** (2026-08-07) — **within the past week**. (JUnit 5 is maintenance-only since JUnit 6 shipped Sept 2025; latest 5.x is 5.14.4.) Routine patch release.

**Maven**
- Latest stable: **3.9.16** (2026-05-17). Preview tracks 3.10.0-rc-1 and 4.0.0-rc-6 (2026-07-30) not yet GA.

**Gradle**
- Latest: **9.7.0** (2026-08-07) — **within the past week**.
- Isolated Projects graduates experimental→incubating (parallel project configuration perf win); Configuration Cache gains `ResolutionResult` API access and third-party Java agent support in TestKit; test-framework startup failures now surface in console output; problem reporting shows up to 2050 source-located problems (up from 50). No explicit breaking changes beyond the standard Isolated Projects caveat.

---

### Node.js

- **Node.js core**: Latest **v26.7.0** (2026-08-05) — **within the past week**. Adds OpenSSL STORE loader (load private keys directly from HSMs/cloud KMS), updated root cert store (NSS 3.125), Perfetto tracing, crypto correctness fixes. Most recent security release was 2026-07-29 (undici updates, HTTP/2 memory-exhaustion fix in 26.5.1). Node is moving to one major release/year starting v27 (Oct 2026).
- **axios**: Latest **v1.19.0** (~2026-08-01, edge of window). No breaking changes; fixes to `AxiosError` serialization, `Content-Length` enforcement, interceptor error handling. **Security history: CVE-2026-40175** (Critical, CVSS 9.9, SSRF→RCE gadget chain, fixed ≥1.15.0), **CVE-2026-25639** (DoS via `__proto__`, fixed 0.30.3/1.13.5); also a supply-chain compromise (2026-03-31) of versions 1.14.1/0.30.4 — confirm you're not on those.
- **bcryptjs**: Latest **v3.0.3** (~Nov 2025). Not within the past week; no CVEs found.
- **express**: 4.x latest **v4.22.2** (2026-05-11); 5.x stable **v5.2.1** (2025-12-01). Not within the past week. 4.22.2 fixes query-string parsing (array notation beyond old 20-item cap, up to 1000). No new CVEs.
- **moment**: Latest **v2.30.1** (2023, unchanged). Project is in maintenance/legacy mode — recommends date-fns/Luxon/Temporal for new projects.
- **pg (node-postgres)**: Latest **v8.23.0** (~2026-08-09) — **within the past week**. Introduces query pipelining (send multiple queries before waiting on responses — throughput win). No CVEs.
- **sequelize**: Latest stable **v6.37.8** (2026-03-07); v7 still alpha. **Security: CVE-2026-30951** — validates cast types in JSON where-clauses to prevent injection-style abuse. Confirm apps are on ≥6.37.8.
- **socket.io**: Server latest **v4.8.3** (Dec 2025). Bundled `ws` dependency bumped to fix CVE-2026-48779 and CVE-2026-45736 — verify transitive `ws` version is current.
- **jest**: Latest **v30.4.2** (~May 2026). Jest 30 drops Node 14/16/19/21 support (min Node 18.x) and requires TypeScript ≥5.4 — a breaking change from Jest 29 if upgrading.
- **prettier**: Latest **v3.9.6** (~2026-07-23); minor 3.9.0 shipped 2026-06-27. New official `@prettier/plugin-oxc` (Rust-based OXC parser) for faster formatting. No CVEs.
- **ReactJS**: Latest **v19.2.8** (2026-07-21). Performance fix only, non-breaking.
- **React Native**: Latest **v0.86.2** (~2026-08-01, edge of window). Security: bumped `fast-xml-parser` (CVE-2026-25896) and `minimatch` (CVE-2026-27903/-27904). Crash fix for `View`/`removeClippedSubviews`.

---

### Database and Storage

- **Oracle Database**: Latest CPU baseline **23.26.3.0.0 / 21.22.0.0.260721 / 19.32.0.0.260721** (2026-07-21). Not within the past week; next CPU is October 2026. **Security: 1,235 CVEs addressed CPU-wide (largest CPU ever)**, max CVSS 9.9; TimesTen In-Memory DB issues remotely exploitable without authentication. Apply immediately.
- **PostgreSQL**: Latest **18.4 / 17.10 / 16.14 / 15.18 / 14.23** (2026-05-14). Not within the past week — next quarterly release is 2026-08-13. **Security: 11 CVEs fixed**, notably CVE-2026-6473/-6475/-6477/-6637 (CVSS 8.8 each — integer wraparound, symlink following, client stack overwrite, stack buffer overflow + SQL injection) plus an MD5 password timing-channel disclosure (CVE-2026-6478).
- **Redis**: Latest **8.10.0** (2026-07-29). Not within the past week. New "compact hashes" encoding, new commands (`HIMPORT`, `LMOVEM`/`BLMOVEM`, `SUNIONCARD`, `SDIFFCARD`, `BACKUP`), TLS peer-cert server-to-server auth. No CVEs specific to 8.10.0.
- **Apache Kafka**: Latest **4.3.1** (2026-06-25). Not within the past week. Fixes a critical Kafka Streams RocksDB native memory leak. No CVEs flagged.
- **MongoDB**: Latest **8.3.7** (2026-07-22), GA 8.3 was 2026-05-04. Not within the past week. **Breaking changes**: sharded-cluster DDL/`applyOps` must run on `mongos` only; sharded→replica-set conversion no longer reversible; upsert oversized-BSON error code changed. **Security: 24 CVEs fixed** (CVE-2026-9737, CVE-2026-13055–13078); SCRAM-SHA-1 now incompatible with FIPS mode.
- **Apache Cassandra**: Latest **5.0.9** (2026-08-07) — **within the past week**. Routine stability/bugfix maintenance patch; no CVEs called out.
- **Elasticsearch**: Latest **9.5.0** (2026-08-04) — **within the past week**. Multi-shard searches now batch into a single round trip; Columnar/`logsdb_columnar` index modes in tech preview; ES|QL gains flattened-field support; ES95 becomes default doc-values codec (~30% storage reduction). No CVEs specific to 9.5.0.
- **Google BigQuery** (managed, rolling releases): Notable items 2026-08-03 to 2026-08-07 — **within the past week**. Cross-cloud connections to AWS/Azure/Salesforce Data 360 from all regions (Preview); JDBC driver GA OpenTelemetry tracing; `APPENDS`/`CHANGES` functions GA for row-level change tracking; Google ODBC driver GA. No breaking changes or CVEs (managed service).

---

### OS and Platform

- **Docker**: Docker Desktop **4.85.0** (2026-08-03) — **within the past week**. `docker sbom` command deprecated. **Security: CVE-2026-8936** (VM panic via unbounded recursion in `grpcfuse` kernel module, triggerable by a container via bind-mounted host folder) — fixed this release.
- **Kubernetes**: Latest **v1.36.3** (2026-07-23, just outside window). Next monthly patch batch ~2026-08-11; **v1.37.0** due 2026-08-26. **Watch for**: CVE-2026-3865/-3864 (CSI SMB/NFS path traversal), CVE-2026-3288 (ingress-nginx config injection), CVE-2026-33186 (gRPC-Go HTTP/2 `:path` validation, relevant to Go-based control-plane/ingress components). Kubernetes 1.34 enters maintenance 2026-08-27; **1.33 support ends 2026-08-31**.
- **GCP Compute Engine**: 2026-08-03 — shared Hyperdisk ML minimum provisioned throughput reduced 100→20 MiB/s per instance (cost win for shared ML storage). **Security**: firmware fix for CVE-2025-61971/-61972 and CVE-2024-36315 (AMD SEV-SNP guest compromise), host-side.
- **GKE**: 2026-08-05 — Preview of AI-generated optimized configs for stateful workloads (Redis/MySQL); Arm workload support on Autopilot GA. 2026-08-03 — TPU7x (Ironwood) Dynamic Subslicing reached GA. GKE Standard now supports up to 100 simultaneous surge-upgraded nodes.
- **GCP Maps API**: No release in-window; quarterly channel bump v3.64→v3.65 planned week of 2026-08-14 — historically these include backwards-incompatible changes, worth monitoring.
- **GCP Translate API**: No release in-window. **Breaking-change note: Translation API Basic and Advanced ("v3") client libraries are not backward/forward compatible** — a real migration hazard if mixing tiers.
- **GCP Cloud Storage**: No release in-window; recent notable: partial-success option for bucket listing (returns reachable buckets instead of erroring) — useful for resilient backend list operations.
- **Google Analytics (GA4)**: No release in-window; Measurement Protocol update (Feb 2026) added `client_id`/`session_id` cookie support, relevant for server-side GA4 event sending.
- **Amazon S3**: 2026-08-06 — AWS Backup for S3 now supports Access Points for direct read-only access to backup data. 2026-08-04 — S3 Vectors now available in AWS European Sovereign Cloud (Germany). Both **within the past week**.
- **Amazon EC2**: 2026-08-04/08-06 — C8g and M8g (Graviton4) instance regional expansions only; no new families or breaking changes.
- **AWS Lambda**: Early Aug 2026 — scalable network bandwidth for non-VPC functions, now scaling with memory up to 3,000 Mbps at 10GB.
- **Amazon EKS**: No release in-window. **Action item: Kubernetes 1.33 standard support and 1.30 extended support both end 2026-08-31** — plan upgrades now.
- **Amazon CloudFront**: No release in-window; most recent (2026-07-14) — CloudFront Functions can write custom data into access logs via `cf.logCustomData()`.
- **Amazon Route53**: No release in-window; most recent updates from Q1–Q2 2026 (DNS64/IPv6 resolver forwarding, Global Resolver GA).
- **Cloudflare**: 2026-08-07 — bot mitigation shifted to continuous trust evaluation; AI Gateway + Workers AI unified into one control plane. 2026-08-06 — AI Search launched. 2026-08-03 — early preview of `@cloudflare/computer` agent runtime. All **within the past week**; additive Workers/AI platform features, no breaking changes or CVEs.
- **Firebase**: August 2026 SDK releases — C++ SDK 13.11.0, JS SDK 12.17.1, Apple SDK 12.17.0, CLI 15.25.0 (bumps Firestore emulator, adds App Check debug-token management). Firebase AI Logic added gemini-3.6-flash/gemini-3.5-flash-lite support.
- **PayPal**: Most recent quarterly changelog (2026-07-01) — Enterprise Payouts webhook/GET API for direct-debit return reasons, self-service REST API credential management with IP allowlisting; Orders API added Google Pay/Apple Pay as payment sources.
- **Apple Pay**: WWDC 2026 redesigned checkout (swipe between cards, grid view, inline rewards/balance) rolling out with iOS 27 (beta ~2026-07-24, GA fall 2026). UI-only redesign — no breaking changes for existing integrations, but merchants should visually test checkout against the iOS 27 beta.

---

## Caveats
- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the past week" is judged against the 2026-08-02 to 2026-08-09 window; where a tool's most recent release predates this window, that release is still reported as the current latest.
- Managed cloud services (BigQuery, GCP/AWS features, Cloudflare, Firebase, PayPal, Apple Pay) don't version the same way as OSS libraries — entries reflect the most recent dated announcements found, not a single canonical "latest version."
