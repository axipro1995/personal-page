# Weekly Technology Update — Week Ending August 5, 2026

**Scope:** ~40 tools/libraries across Java, Node.js, Databases/Storage, and OS/Platform/Cloud. Coverage emphasizes the past week, with major releases from the past month for context. "Needs attention" flags call out HIGH/CRITICAL security items.

## TL;DR
- **Patch now:** The highest-severity items this cycle are the PostgreSQL May 14, 2026 security release (11 CVEs — three at CVSS 8.8 with practical exploit paths), Sequelize v6 SQL-injection CVE-2026-30951, PostgreSQL JDBC channel-binding downgrade CVE-2026-54291, the MongoDB 8.0.28 CVE cluster (plus "MongoBleed" CVE-2025-14847, CVSS 8.7, on CISA's KEV list), Redis RESTORE/use-after-free RCE CVEs, and two Docker Desktop flaws (root privesc CVE-2026-31431 and VM-panic CVE-2026-8936). Node.js also shipped a July 29 advisory fixing 11 CVEs (3 High).
- **Major version state:** Java 26 (non-LTS) is current with Java 25 the latest LTS; Spring Boot 4.1 and Spring Framework 7.0.8 are current; Node.js 26 is Current and Node.js 24 is Active LTS; React 19.2.x is current; Kubernetes 1.36 is current.
- **Watch these deprecations:** Google Maps Platform is removing/deprecating Directions, Distance Matrix and Heatmap APIs; Firebase is shutting down Imagen models (as early as June 30, 2026) and sunsetting Firebase Studio (Mar 2027); GKE multi-cloud (on AWS/Azure) is now maintenance-only.

---

## Java Ecosystem

**⚠️ Needs attention:** Netty patched two CVEs — CVE-2026-33871 (HTTP/2 CONTINUATION frame flood DoS, rated High, CVSS 8.7) and CVE-2026-33870 (HTTP/1.1 request smuggling) — disclosed March 24, 2026 and fixed in **4.2.11.Final and 4.1.132.Final** (advisory GHSA-w9fj-cfpg-grvv); the current 4.1.136.Final and 4.2.x lines carry the fixes. No other HIGH/CRITICAL advisories in the core Java tooling this week.

| Tool | Latest Version | Release Date | Key Changes / Breaking Changes / Security Fixes |
|---|---|---|---|
| Java (JDK/OpenJDK) | JDK 26 (current feature release); JDK 25 (latest LTS) | JDK 26: Mar 17, 2026 | JDK 26 is a non-LTS, 6-month release (10 JEPs; AOT cache made GC-agnostic via JEP 516, JEP 500 preparing to disallow final-field mutation through deep reflection). JDK 25 remains the LTS for production. JDK 21 NFTC updates end Sept 2026. |
| Netty | 4.1.136.Final; 4.2.x line current (4.2.15.Final) | 4.1.136.Final: Jul 9, 2026 | ⚠️ CVE-2026-33871 (HTTP/2 CONTINUATION flood DoS, CVSS 8.7) and CVE-2026-33870 (request smuggling) fixed in 4.2.11.Final / 4.1.132.Final (GHSA-w9fj-cfpg-grvv). Ongoing epoll CPU-usage and OpenSSL robustness fixes. |
| Vert.x | 5.1.5 (5.x current); 4.5.31 (4.5 maintenance) | 5.1.5: Jul 13, 2026; 4.5.31: Jul 20, 2026 | Bug fixes plus vulnerability fixes; 4.5.29 shipped Netty 4.1.136.Final to inherit Netty's CVE fixes. Vert.x 5.1 GA'd May 27, 2026. |
| Spring (Framework / Boot) | Spring Framework 7.0.8; Spring Boot 4.1.0 (also 4.0.7) | Boot 4.1.0: Jun 10, 2026 | Boot 4.1 requires Java 17 min, compatible up to Java 26, needs Spring Framework 7.0.8+. Spring Boot 3.5 reached OSS EOL June 30, 2026 (final patch 3.5.16 on Jun 25) — a breaking migration point for 3.x users. |
| Hibernate ORM | 7.4.5 (latest stable); 8.0.0.Beta1 (dev) | 7.4.5: July 2026 | 7.4.x adds Elasticsearch/OpenSearch compatibility and bugfixes. 8.0 (dev) targets Jakarta Persistence 4.0. 7.3 reached EOL June 7, 2026. |
| JDBC (drivers/spec) | pgjdbc 42.7.12; MySQL Connector/J 9.7.0 (26.7 line); Oracle ojdbc11/ojdbc17 | pgjdbc 42.7.12: Jun 29, 2026; Connector/J 9.7.0: Apr 22, 2026 | ⚠️ pgjdbc 42.7.12 fixes CVE-2026-54291 (channel-binding downgrade under `channelBinding=require`). JDBC spec remains 4.3; most drivers implement 4.2. Oracle/MySQL rebranded version schemes. |
| Protobuf (protobuf-java) | 4.35.1 | Jun 11, 2026 | Protobuf release 35.x; Java runtime is 4.35.x. Edition 2026 work underway (enforce_proto_limits, STRICT default symbol visibility). Java 3.x line has a 36-month support window. |
| Dagger 2 (DI) | Latest 2.x (2026 release) | 2026 | Recent changes: Hilt minSDK raised to 23 (breaking for older minSDK), Kotlin updated to 2.3.21, Jakarta `@Singleton` support, generated Factory/MembersInjector constructors changed from public to private. |
| Jackson | 3.1.3 (3.1 LTS line); 2.x still active (2.21.0) | 3.1.3: May 1, 2026; 2.21.0: Jan 18, 2026 | Jackson 3.1 (Feb 23, 2026) is an LTS with 100+ changes; AtomicReference deserialization default changed (potential behavior break). 2.x remains maintained in parallel. |
| Log4j2 | 2.26.1 | Jul 2026 (2.26.0 was May 7, 2026) | 2.26.x delivers full GraalVM native-image reachability metadata, Pattern Layout exception-handling rewrite, JMX disabled by default. No new Log4Shell-class CVE. |
| Guava | 33.6.0 (-jre / -android) | Apr 14, 2026 | Deprecated CacheBuilder TimeUnit APIs in favor of Duration; migrating away from finalize() toward PhantomReference; deserialization no longer mutates final fields. |
| JUnit (Jupiter) | 6.1.2 (Platform+Jupiter+Vintage unified) | Jul 12, 2026 | JUnit 6 (Sept 30, 2025) raised baseline to Java 17/Kotlin, unified version numbers, removed junit-platform-runner/jfr, switched CSV parsing to FastCSV, added Kotlin coroutine + cancellation support. Vintage engine deprecated. |
| Maven | 3.9.16 (current stable); 4.0.0-rc-6 and 3.10.0-rc-1 in preview | 3.9.16: May 13, 2026 | Maven maintains only the two latest minor series (no LTS). Maven 4 (RC) requires Java 17 and brings major POM/build changes. |
| Gradle | 9.6.x (current stable); 9.7.0-rc-3 in preview | 9.6: June 2026; 9.7.0-rc-3: Aug 4, 2026 | Gradle 9 embeds Kotlin 2.2.x, uses Kotlin language version 2.2, productized Configuration Cache. Isolated Projects targeted for a later 9.x/Gradle 10. |

---

## Node.js Ecosystem

**⚠️ Needs attention:** Sequelize v6.37.8 fixes CVE-2026-30951, a SQL-injection flaw via unescaped cast type in JSON/JSONB WHERE clauses (affects v6.x ≤ 6.37.7; v7 alpha unaffected). Socket.IO bumped its `ws` dependency for CVE-2026-48779 and CVE-2026-45736. Node.js itself, in its **July 29, 2026 advisory** (v22.23.2 / v24.18.1 / v26.5.1), patched **11 CVEs (3 High, 5 Medium, 3 Low)** — the Highs being two HTTP/2 flaws (heap use-after-free **CVE-2026-56848** and maxSessionMemory-bypass **CVE-2026-56846**) plus a Permission Model path-traversal (**CVE-2026-58043**).

| Tool | Latest Version | Release Date | Key Changes / Breaking Changes / Security Fixes |
|---|---|---|---|
| Node.js core | 26.6.0 (Current); 24.15.0 (Active LTS) | 26.6.0: Aug 3, 2026 | Node 26 (Current, May 2026) enables Temporal API by default, V8 14.6, Undici 8. Node 24 is the production LTS. Node 25 hit EOL Jun 1, 2026. From Node 27 (Oct 2026) the project moves to one annual release, all LTS. ⚠️ See July 29 advisory (11 CVEs, above). |
| axios | 1.19.0 | ~late July 2026 | Adds Node zstd response decompression, safer AxiosError serialization (non-enumerable `cause`), restored Basic auth on same-origin redirects, staged npm publishing with provenance. |
| bcryptjs | 3.0.3 | Nov 2, 2025 | Stable, zero-dependency; v3 defaults to `$2b$` hashes. No new CVEs. |
| express | 5.2.1 (current); 4.22.2 (v4 maintenance) | 5.2.1: Dec 1, 2025; 4.22.2: May 11, 2026 | Express 5.2 is the TC-endorsed production release. Express 4 targeted EOL no sooner than Oct 1, 2026. |
| moment | 2.30.1 | Dec 27, 2023 | In maintenance/legacy mode — no new features, no v3. Team recommends Luxon/day.js/date-fns/Temporal. |
| pg (node-postgres) | 8.22.0 | ~June 2026 | Active maintenance; no open vulnerabilities on current version. |
| sequelize | 6.37.8 (v6 stable); v7 in alpha | 6.37.8: Mar 7, 2026 | ⚠️ Fixes CVE-2026-30951 (SQL injection via JSON/JSONB cast). v7 (@sequelize/core) remains alpha and unaffected. |
| socket.io | 4.8.3 | Dec 2025 | ⚠️ Recent patches bumped `ws` to ~8.21.0 (CVE-2026-48779) and ~8.20.1 (CVE-2026-45736). |
| jest | 30.4.2 | ~May 2026 | Jest 30 (June 2025) dropped Node 14/16/19/21 (min Node 18), min TypeScript 5.4, JSDOM v26, removed alias matchers. 30.4.x adds `require()` of ES modules on Node 24.9+. |
| prettier | 3.9.6 | Jul 21, 2026 | 3.9.x upgraded Markdown parser to micromark v4 plus YAML/Flow/GraphQL/Angular parsers and `--no-semi` improvements; 3.9.6 adds `import defer` TS support. Pin exact versions. |
| React | 19.2.8 (also 19.1.9, 19.0.8 backports) | Jul 21, 2026 | React 19.2 (Oct 2025) added `<Activity>`, `useEffectEvent`, cacheSignal, Performance Tracks. React Compiler stable at 1.0. Stewardship moved to the React Foundation (Linux Foundation). No React 20 announced. |
| React Native | 0.85.1 | Apr 13, 2026 | New Architecture is now the baseline — old architecture dropped since 0.82 (breaking; apps must migrate). 0.85 adds new animation system and memory-leak fixes. |

---

## Database and Storage

**⚠️ Needs attention (multiple CRITICAL/HIGH):**
- **PostgreSQL** — the May 14, 2026 release "fixes 11 security vulnerabilities and over 60 bugs" (official announcement), of which three are CVSS 8.8 with practical exploit paths, including CVE-2026-6473 (SQL injection in pg_createsubscriber), CVE-2026-6477 (path traversal in pg_basebackup/pg_rewind) and a CREATE TYPE search_path hijack. This is the largest single-release PostgreSQL security batch in recent memory.
- **MongoDB** — 8.0.28 fixes ~12 CVEs; separately "MongoBleed" (**CVE-2025-14847**, CVSS 8.7) was disclosed Dec 19, 2025 as an unauthenticated remote memory-disclosure flaw affecting 87,000+ exposed instances, added to CISA's KEV catalog Dec 29, 2025 with a Jan 19, 2026 FCEB remediation deadline — patched across all supported lines.
- **Redis** — multiple RCE-class CVEs across 8.x (use-after-free CVE-2026-23479; RESTORE invalid-memory CVE-2026-25243/25589; RedisBloom RESTORE OOB write).

| Tool | Latest Version | Release Date | Key Changes / Breaking Changes / Security Fixes |
|---|---|---|---|
| Oracle Database | Oracle AI Database 26ai — RU 23.26.2.0.0 | On-prem GA 23.26.1.0.0: Jan 27, 2026; Apr RU: ~Apr 28, 2026 | "23ai" rebranded to "Oracle AI Database 26ai"; still the 23.x code line with `23.YEAR.QUARTER` versioning. 19c RU 19.31 also delayed to ~Apr 28, 2026. |
| PostgreSQL | 18.4 / 17.10 / 16.14 / 15.18 / 14.23 | May 14, 2026 | ⚠️ 11 CVEs + 60+ bug fixes (see flag above). PostgreSQL 19 is in Beta 2 (Jul 16, 2026). PG14 EOL Nov 12, 2026. |
| Redis | Redis Open Source 8.10 | ~July 2026 | ⚠️ Multiple RCE-class CVEs patched across 8.x. 8.10 adds compact hashes and HIMPORT. Tri-licensed (RSAL/SSPL/AGPL) since 8.0; 8.0 End of Support Dec 1, 2026. (Version conflict: some trackers still list 8.6.2 — treat 8.10 as latest but verify.) |
| Apache Kafka | 4.3.1 | Jun 25, 2026 | KRaft-only (ZooKeeper removed in 4.0). 4.3.1 fixes 16 issues including a Kafka Streams RocksDB native memory leak. 3.9.2 (Feb 2026) was a bugfix+security update. |
| MongoDB | 8.0.28 (stable); 8.2.12 / 8.3.7 (rapid); 7.0.39 | Jul 22, 2026 | ⚠️ 8.0.28 fixes ~12 CVEs (CVE-2026-9737, CVE-2026-13055–13066). MongoBleed (CVE-2025-14847) patched across lines. |
| Apache Cassandra | 5.0.8 | ~May 1, 2026 | Backports Automated Repair (CEP-37), TLS 1.3 in cassandra-stress, SnapshotLoader hardening. 6.0-alpha1 available. No new HIGH CVE. |
| Elasticsearch | 9.4.4 (9.x); 8.19.19 (8.x) | 9.4.4: Jul 21, 2026 | 9.5.0 (upcoming) adds repository overwrite-protection and batched multi-shard query phase. 9.3 EOL May 26, 2026; 8.19 supported to Jul 15, 2027. |
| Google BigQuery | Rolling service | Apr 2026 updates | Snowflake→BigQuery transfers GA; BigQuery Graph (GQL) preview; stateful continuous queries preview; advanced runtime becoming default for all projects through early 2026. |

---

## OS and Platform / Cloud Services

**⚠️ Needs attention:** Docker Desktop patched CVE-2026-31431 ("copy.fail," privilege escalation to host root) and CVE-2026-8936 (grpcfuse VM panic). Cloudflare's Nov 18, 2025 outage — its most severe since 2019 — was caused by a ClickHouse permissions change at 11:05 UTC that doubled the Bot Management "feature file" (from ~60 to >200 features), exceeding a hard-coded proxy limit; failures began 11:20 UTC and a known-good file was restored at 14:30 UTC (~5.5-hour disruption, not an attack). Google Maps Platform is in a major deprecation wave.

| Tool | Latest Version / State | Date | Key Changes / Deprecations / Security Fixes |
|---|---|---|---|
| Docker | Engine 29.4.0; Desktop 4.79.0 | Engine: Apr 7, 2026; Desktop: Jun 22, 2026 | ⚠️ Desktop fixes CVE-2026-31431 (root privesc) and CVE-2026-8936 (VM panic). Windows MSI can't update over 4.56–4.65 (uninstall first). "Docker AI" rebranded to "Gordon." |
| Kubernetes | 1.36.x (1.36.3 latest patch) | 1.36.2: Jun 9, 2026; 1.37 scheduled Aug 26, 2026 | Supported (N-2): 1.36/1.35/1.34. 1.33 hit EOL Jun 28, 2026. 1.34 graduated stable automated cgroup-driver detection; deprecated `cgroupDriver` kubelet setting. |
| Google Compute Engine | Rolling service | — | No standout GCE-specific launch isolated for the past week (flagged as not confirmed). |
| Google Kubernetes Engine (GKE) | Tracks upstream via channels | Docs updated Jul 23, 2026 | GKE on AWS and GKE on Azure (multi-cloud) are now maintenance-only (bug fixes only, no new functionality). |
| Google Maps API | Rolling service | Deprecations Feb–May 2026 | ⚠️ DirectionsService, DirectionsRenderer, DistanceMatrixService deprecated Feb 25, 2026; Heatmap Layer fully removed as of May 2026; Drawing Library being removed in 2026; legacy Places API frozen. |
| Google Cloud Translation API | Rolling service | — | No specific major launch/deprecation isolated for the past week (flagged as not found). |
| Google Cloud Storage | Rolling service | — | No standout GCS-specific launch isolated for the past week (flagged as not found). |
| Google Analytics (GA4) | Rolling service | Jul 2026 updates | New diagnostic for missing aggregate identifiers (GBRAID/gad_); native Google Business Profile integration; cross-channel budgeting and conversion-attribution report (beta). |
| Amazon S3 | Rolling service | Apr & Jul 2026 | Amazon S3 Files (file-system access, Apr 7–8, 2026); S3 Vectors GA (Jan 2026, up to 2B vectors/index); S3 Annotations (searchable JSON/XML/YAML context, Jul 2026). |
| Amazon EC2 | Rolling service | 2026 (previewed re:Invent 2025) | New Graviton5-powered M9g/M9gd instances GA: "up to 25% better compute performance… 35% faster web applications, 35% faster ML inference, and 30% faster databases," with 192 cores per chip, 5x larger cache, DDR5-8800 and PCIe Gen 6. |
| AWS Lambda | Rolling service | Jul 15, 2026 | Lambda now supports self-managed S3 buckets for code storage — eliminates the 75GB/region code-storage limit and speeds activation. |
| Amazon EKS | Standard support: k8s 1.35/1.34/1.33 | As of Apr 2026 | Follows upstream with EKS platform-version cadence. No HIGH CVE flagged. |
| Amazon CloudFront | Rolling service | Nov 2025 + 2026 updates | Flat-rate pricing plans (Free/Pro/Business/Premium) launched Nov 2025; 2026 added Lambda@Edge, CAPTCHA, mTLS, AI-activity dashboard, configurable usage allowances. |
| Amazon Route 53 | Rolling service | GA Mar 9, 2026 | Route 53 Global Resolver GA (recursive DNS beyond the VPC); original resolver renamed Route 53 VPC Resolver. Also DNS Firewall Advanced. |
| Cloudflare | Rolling service | 2026 | ⚠️ Nov 18, 2025 major outage (Bot Management feature-file config bug, ~5.5 hrs, not an attack); Feb 20, 2026 BGP/BYOIP outage. Q2 2026 Internet Disruption Summary published Jul 29, 2026. |
| Firebase | JS/Apple SDK v12.17.0; Cloud Functions SDK 7.3.0 | Jul 2026 | ⚠️ All Imagen models deprecated — "will shut down as early as June 30, 2026" (migrate to Gemini 3.x "Nano Banana" image models). Firebase Studio sunset Mar 22, 2027. Added Gemini 3.6-flash/3.5-flash-lite support. Apple SDK leaving CocoaPods after Oct 2026. |
| PayPal (developer/API) | JS SDK v6; v1 REST | Docs updated May–Jul 2026 | JS SDK v6 adds Apple Pay one-tap (36 countries/22 currencies); Q1 2026 added Verifone in-person payments, idempotency in Enterprise SDKs, Payment Account Reference. |
| Apple Pay (developer platform) | Rolling service | Docs updated Jul 24, 2026 | No standalone platform version/CVE surfaced; primary 2026 activity via integrations (e.g., PayPal JS SDK v6 Apple Pay support). |

---

## Recommendations

**Immediate (this week) — apply security patches:**
1. **PostgreSQL:** upgrade to 18.4/17.10/16.14/15.18/14.23 (May 14 release). Top priority given 11 CVEs, three at CVSS 8.8 with practical exploit paths.
2. **MongoDB:** move to 8.0.28 (or patched 8.2/8.3/7.0 equivalents) to close the ~12-CVE cluster and MongoBleed (on CISA KEV).
3. **Redis:** upgrade to 8.10 (or the latest patched 8.x) to remediate the RCE/use-after-free CVEs.
4. **Node.js:** apply the July 29 advisory (26.5.1/24.18.1/22.23.2) to fix the two HIGH HTTP/2 flaws and the Permission Model path-traversal.
5. **Docker Desktop:** update to 4.79.0+ to fix the root-privesc and VM-panic CVEs.
6. **Sequelize:** bump to 6.37.8 (or migrate to v7 alpha where feasible) for CVE-2026-30951.
7. **PostgreSQL JDBC:** upgrade to 42.7.12 if you use `channelBinding=require`.
8. **Netty / Vert.x / Socket.IO:** pull the current releases to inherit the Netty HTTP/2 DoS + smuggling fixes and the Socket.IO `ws` bumps.

**Short-term (this month) — plan migrations:**
- Spring Boot 3.5 is OSS-EOL as of June 30, 2026 — plan the move to 4.0/4.1 (a breaking upgrade) or arrange extended support.
- Node.js: keep production on 24 LTS; test 26 in CI ahead of its Oct 2026 LTS promotion and the new annual-release model from Node 27.
- Google Maps Platform: migrate off DirectionsService/DistanceMatrixService and the removed Heatmap Layer.
- Firebase: migrate off Imagen models before the June 30, 2026 shutdown window.

**Benchmarks that would change these recommendations:** a new PostgreSQL/MongoDB/Redis out-of-cycle security release; promotion of Kubernetes 1.37 (Aug 26) or Node.js 26 to LTS (Oct); Gradle 9.7 or Maven 4.0 reaching GA; or a new Netty/axios advisory at HIGH+ severity.

## Caveats
- Fast-moving cloud services (AWS, GCP, Cloudflare, Firebase, PayPal, Apple Pay) have no traditional version numbers; entries reflect notable feature launches, deprecations, and advisories rather than exhaustive changelogs.
- **Unconfirmed for the past week:** Google Compute Engine, Google Cloud Translation API, and Google Cloud Storage had no standout recent-week item isolated — flagged rather than guessed.
- **Version conflicts noted:** Redis (8.10 per GitHub/EOL trackers vs. 8.6.2 on some pages) and Jackson-databind (3.1.3/2.22.1 vs. a 3.2.1 listing on one tracker) — treat the higher/GitHub-sourced numbers as latest but verify against official release notes before acting.
- The Dagger 2 exact latest version number was not firmly confirmed; recent changes (Hilt minSDK 23, Kotlin 2.3.21, private generated constructors) are reported from the google/dagger release feed.
- Gradle's exact current stable (9.6.x) should be confirmed against gradle.org/releases; 9.7.0 is in RC as of Aug 4, 2026. Several patch-level version numbers (e.g., Log4j2 2.26.1, pg 8.22.0, jest 30.4.2) were approximated from release-tracker data and should be re-verified against primary release notes before deployment decisions.
- Because the live search budget was reached mid-research, the Node.js package tail (sequelize, jest, prettier, react-native, pg, bcryptjs), all Database/Storage entries, the cloud services, and JDBC driver details were gathered via a dedicated research pass rather than direct primary-source fetches by the lead author; the highest-severity, action-driving items (PostgreSQL, MongoDB, Node.js, Netty, Docker, Firebase, Cloudflare, EC2) were independently corroborated during enrichment.