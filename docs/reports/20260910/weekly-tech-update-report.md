# Weekly Tech Update Report — Week Ending September 10, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-09-03 to 2026-09-10**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, GitHub release pages, and vendor advisories.

## TL;DR

- **Released this week:** Docker Engine **29.8.0** (2026-09-03), Node.js **v26.8.2** + **v24.21.0 LTS** (~2026-09-08, OpenSSL 3.5.8 bump), plus rolling platform updates to Firebase (Admin Node.js SDK 14.3.0), Google Cloud Storage, and AWS (Graviton5 GA for EC2/Lambda). **Java's library stack, the Node/DB library stack, PostgreSQL, Redis, Kafka, MongoDB, and Kubernetes had no release inside this window.**
- **Patch now / act — security:**
  - **Redis — CVE-2026-81934 (use-after-free in `tlsProcessPendingData()`, unauthenticated RCE against TLS-enabled Redis).** NVD scores it **9.8 Critical**; Redis rates it **7.5 High (CVSS v4)**. **A full public PoC and technical write-ups circulated this week** — treat as actively weaponized. Already fixed in the **2026-08-17 coordinated train** (8.10.1 / 8.8.2 / 8.6.6 / 8.4.6 / 8.2.9 / 7.4.11 / 7.2.16 / 6.2.24). If you run Redis with `tls-port` enabled and are below those versions, patch immediately or disable TLS termination in Redis (front with a proxy) as a stopgap.
  - **Carry-forward (still the operative guidance):** the 2026-08-20 ~91-CVE Spring-portfolio wave — **CVE-2026-59270** (Spring Security embedded UnboundID LDAP admin-DN exposure, CVSS 9.4; fixed in Spring Security 7.1.1 / 7.0.7) and **CVE-2026-59285** (Spring for GraphQL unsafe deserialization → possible RCE, CVSS 9.2), plus a dozen-plus High-severity CVEs across Spring Security, Data REST, Integration, Reactor, AMQP, Batch, and Spring AI. No new Spring advisory landed 09-03 → 09-10.
  - **Jackson** PolymorphicTypeValidator-bypass set (CVE-2026-54512 CVSS 8.1 and siblings) and the **MongoDB** July 27-CVE disclosure remain the current floors to confirm — no follow-up bulletins this week.
  - **Node.js v26.8.2 / v24.21.0** carry an **OpenSSL 3.5.8** upgrade and an NSS root-certificate refresh. Not published as a security release, but the OpenSSL bump is worth taking.
- **Breaking-change / operational watch:**
  - **Cloudflare AI-crawler defaults go live 2026-09-15.** Crawler traffic is split three ways — **Search** (allowed), **Training** (blocked by default on pages that serve ads), **Agent** (blocked by default on ad pages). Applies automatically to **all new domains, new customers, and existing Free-tier customers**; paid customers keep existing config. If you operate an AI agent/crawler, register and separate your Search vs Training vs Agent user-agents before the cutoff; if you own sites on Cloudflare Free, review the setting if you *want* AI access.
  - **PostgreSQL 19:** the **SQL/PGQ property-graph query** feature was **reverted from PG19 on 2026-09-07** — earliest return is PG20 (Sept 2027). PG19 GA still had **not** shipped as of 2026-09-10 (expected imminently). Adjust any roadmap that assumed `GRAPH_TABLE` in PG19.
  - **Kubernetes 09-15 patch train** (1.37.1 / 1.36.4 / 1.35.8 / 1.34.11) is targeted for next week — not yet released. **1.34 is in maintenance mode** (critical-fixes-only, EOL 2026-10-27).
  - **Java 27** (non-LTS) GA is expected **~2026-09-16**; next Oracle Critical Patch Update is **2026-10-20**. The next LTS is Java 29 (Sept 2027).
  - **iOS 27 / Apple Pay** GA still expected this month — validate PassKit / Apple Pay flows against the developer beta. WWDC 2026 PassKit changes (Poster Generic pass style, new barcode formats, Featured Actions) land with the OS.
  - Carried forward from the 4.0 baseline: Spring Boot 4 (Jackson 3 default, Spring Security 7 CSRF changes, Undertow dropped); React Native 0.87 (Strict TypeScript API default; Node 22 / AGP 9 / Kotlin 2.0 minimums); Gradle 9.7 (Isolated Projects incubating, embedded Kotlin 2.4); Firebase Apple SDK stops publishing to CocoaPods after **October 2026**.

---

### Java

**Window checked:** 2026-09-03 to 2026-09-10. **No Java-ecosystem tool shipped a release inside this window.** All items below are most-recent-stable and flagged as such.

**Java (core / JDK)**
- No release inside the window. **Java 26** (2026-03-17) is the current feature release; **Java 25 LTS** is the current long-term line. **Java 27** GA is expected **~2026-09-16** (non-LTS, six-month support). Oracle's next quarterly **Critical Patch Update is 2026-10-20**; the monthly security baseline remains the 2026-08-18 CSPU. No new Java-core security news 09-03 → 09-10.

**Netty**
- No release this window. Latest remains **4.1.137.Final / 4.2.17.Final** (early August). Standing fix set unchanged: CVE-2026-62243 (TLS hostname-verification bypass on the OpenSSL client path), CVE-2026-59903 (Vary-header CORS cache poisoning), CVE-2026-62380 (SOCKS4/5 encoder injection). No breaking changes.

**Vert.x**
- No release this window. Latest remains **5.1.6 / 4.5.32** (2026-08-07/08). No new CVEs.

**Spring (Framework / Boot / Cloud / AI / Modulith)**
- No release inside the window. Spring Boot remains at **4.1.1 / 4.0.8** (2026-08-20/21) with **4.2.0-M1**; Spring Framework at **7.0.9**; Spring Cloud at **2025.1.3 "Oakwood"**; Spring AI at **2.0.1**. No 4.1.2 yet.
- **Action still open:** the 2026-08-20 ~91-CVE portfolio wave. Critical named CVEs: **CVE-2026-59270** (Spring Security embedded UnboundID LDAP, CVSS 9.4) and **CVE-2026-59285** (Spring for GraphQL unsafe deserialization → possible RCE, CVSS 9.2). Open-source fixes are in **Spring Security 7.1.1 and 7.0.7**; the 7.1.0.1 / 7.0.6.1 / 6.5.12 / 6.4.19 / 5.8.28 / 5.7.26 back-ports are Enterprise-Support-Only.

**Hibernate**
- No release this window. Latest remains **7.4.7.Final / 6.6.56.Final** (August 2026). 8.0 still in Beta. No CVEs.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06). No new release/CVE. Standing history: CVE-2026-54291 (SCRAM channel-binding downgrade, fixed 42.7.12), CVE-2026-42198 (SCRAM-response DoS, fixed 42.7.11).
- **MySQL Connector/J**: latest **9.7.0** (2026-04-22). No new release/CVE.

**Protobuf**
- No release inside the window. Current line is the **Edition 2026** generation (protoc 33.x/34.x series; one source shows a v36.0 GA line from 2026-08-20 — treat the exact latest as needing direct confirmation against `protocolbuffers/protobuf` releases). No CVEs. Carried-forward breaking notes: `sun.misc.Unsafe` removal from Java codegen; Bazel `--proto_toolchain_for*` / `--proto_compiler` flags deprecated and no longer read by proto rules.

**Dagger**
- No release this window. Latest remains **2.60.1** (2026-07-06). No CVEs or breaking changes.

**Jackson**
- No release this window. Latest lines: **2.20.0** (2.x), **3.2.x** (3.x). Standing critical set still the top outstanding Java CVE risk for laggards: **CVE-2026-54512** (CVSS 8.1, PolymorphicTypeValidator bypass), **CVE-2026-54513/-54514** (sibling PTV/DNS bypasses), **CVE-2026-54515** (case-insensitive deserialization reopens `@JsonIgnoreProperties`-protected fields), **CVE-2026-59889** (`@JsonView` bypass via `@JsonUnwrapped`).

**Log4j**
- No release this window. Latest remains **2.26.1** (2026-07-02). No new CVEs.

**Guava**
- No release this window. Latest stable **33.6.0** (2026-04-14); some trackers show a **33.7.x** line from mid-August — unconfirmed against `google/guava`'s release page. No CVEs.

**JUnit**
- No release this window. Latest remains **JUnit 6.1.3** (2026-08-07). No CVEs. JUnit 5→6 migration notes stand: Java 17 minimum, Kotlin 2.2+, removed modules/APIs, FastCSV engine.

**Maven**
- No GA release this window. Latest stable **3.9.16** (2026-05-13); pre-release **4.0.0-rc-6** (2026-08-04, still not GA, requires Java 17). No CVEs.

**Gradle**
- No release this window. Latest remains **9.7.1** (2026-08-19/20). No CVEs. 9.7-line changes carried forward: Isolated Projects incubating; embedded Kotlin 2.4.0 (drops Kotlin 1.9 language-version support); relocated `org.gradle.internal.impldep.*` no longer importable from the Kotlin DSL.

---

### Node.js

**Window checked:** 2026-09-03 to 2026-09-10. **Only Node.js core shipped inside this window** — the whole library stack (axios, express, pg, sequelize, socket.io, jest, prettier, React, React Native, etc.) was quiet.

- **Node.js core**: **Within the window — v26.8.2 (Current) and v24.21.0 "Krypton" (LTS), released within ~24 hours of each other around 2026-09-08.** Both carry an **OpenSSL 3.5.8** upgrade and an NSS root-certificate update. v24.21.0 also adds a **STORE loader for hardware security modules (HSMs)** and notable URL-parsing / HTTP-response speedups; v26.8.2 bumps **Undici to 8.10.2**, deprecates `Server.prototype._listen2`, and improves riscv64 build flags. Published as regular (not security) releases, but take the OpenSSL bump. Node.js 26 enters **LTS in October 2026**; the security baseline remains the 2026-07-29 release (Permission Model bypass, HTTPS Agent mTLS/session-reuse, DNS/SQLite/zlib) — confirm applied.
- **axios**: No release this window. Latest **v1.20.0** (2026-08-24/26) — prototype-pollution hardening, `ContentTooLarge`/`UnprocessableContent` status names added (old aliases retained), interceptor/socket memory-leak fixes. No open CVE.
- **bcryptjs**: Latest **v3.0.3** (2025-11-02). No release, no CVEs.
- **express**: 5.x latest **v5.2.1** (2025-12-01); 4.x latest **v4.22.2** (2026-05-11). No release this window. Standing item: **CVE-2026-12590** (body-parser, Low) — invalid `limit` silently disabled body-size enforcement; fixed in body-parser 1.20.6 / 2.3.0.
- **moment**: Latest **v2.30.1** (2023-12-27). Maintenance-mode; keep migrating off it.
- **pg (node-postgres)**: Latest **v8.23.0** (2026-08-08). No release this window, no CVEs.
- **sequelize**: v6 latest **v6.37.8** (2026-03-07); v7 still alpha. No release this window. Standing item: **CVE-2026-30951** (SQL injection via unescaped JSON/JSONB cast type, fixed 6.37.8, v7 unaffected).
- **socket.io**: Server latest **v4.8.3** (2026-03-18); parser latest **v4.2.7 / v3.3.6** (2026-07-16). No release this window. Standing item: **CVE-2026-69185** (CVSS 7.5, zero-attachment memory-exhaustion DoS) — confirm `socket.io-parser` ≥ 4.2.7.
- **jest**: Latest **v30.4.2** (2026-05-09). No release this window, no CVEs.
- **prettier**: Latest **v3.9.6** (3.9 line from 2026-06-27; latest patch ~2026-07). No release this window, no CVEs. 3.9 line highlights: Markdown parser moved from remark-parse v8 to micromark v4; `import defer` support in the TypeScript parser.
- **ReactJS**: Latest **v19.2.8** (2026-07-21). No release this window, no new CVEs. RSC DoS CVEs (CVE-2026-23864/-23870 in `react-server-dom-*`) remain relevant only for RSC/Server Actions users.
- **React Native**: No release this window. Latest **v0.87.x** (0.87.0 on 2026-08-11). **0.88 is the next release** and is where the legacy-TypeScript-types opt-out remains available (removal targeted for the release after 0.88). 0.87 breaking notes carried forward: Strict TypeScript API is the default public JS API; Node 22 / AGP 9 / Kotlin 2.0+ minimums; Metro 0.87.

---

### Database and Storage

- **Oracle Database**: No release inside the window. Oracle AI Database **26ai** (release-update line 23.26.x; latest RU 23.26.3, July 2026) is current. Next quarterly **CPU 2026-10-20**.
- **PostgreSQL**: No release inside the window. Latest stable remains **18.6 / 17.11 / 16.15 / 15.19 / 14.24** (2026-08-13). **PostgreSQL 19 is still pre-GA** (Beta 3 from 2026-08-13, RC expected). **2026-09-07: the SQL/PGQ property-graph query feature was reverted from PG19** and will not return before PG20 (Sept 2027). Remaining PG19 headline features unchanged: `pg_plan_advice`, native `REPACK` / `REPACK CONCURRENTLY`, parallel autovacuum, expanded monitoring views, sequences in logical replication, `GROUP BY ALL`. Watch next week for the 19.0 GA.
- **Redis**: No release inside the window. **Security development this week: CVE-2026-81934** (TLS `tlsProcessPendingData()` use-after-free → unauthenticated RCE) now has a **public PoC and full technical write-ups**. NVD 9.8 / Redis 7.5 (CVSS v4). Already fixed in the **2026-08-17 coordinated train** (8.10.1 / 8.8.2 / 8.6.6 / 8.4.6 / 8.2.9 / 7.4.11 / 7.2.16 / 6.2.24), which also covered CVE-2026-62356 (CMSketch heap OOB write) and use-after-free / RESTORE / ACL-bypass / TLS siblings. **If you run TLS-enabled Redis below those floors, patch now.**
- **Apache Kafka**: No release inside the window. Latest remains **4.3.1** (2026-06-25). No new CVEs. (4.1 introduced KIP-932 queues in preview and the KIP-1071 Streams rebalance protocol in early access; 4.2 GA was 2026-02-17.)
- **MongoDB**: No release inside the window. Latest server lines **8.0.29** (2026-08-11) and rapid-release **8.3.8** (August 2026). The July 27-vulnerability disclosure (query-level RBAC bypass **CVE-2026-13059** CVSS 8.6; Compass command-injection **CVE-2026-14881** CVSS 8.4) remains the operative advisory.
- **Apache Cassandra**: No release inside the window. Latest remains **5.0.9** (2026-08-07). No new CVEs.
- **Elasticsearch**: No release inside the window. Latest is **9.5.3** (2026-09-03, at the window's edge and already covered last week) — routine bugfix, no CVEs, no breaking changes; 8.x line latest **8.19.20** (2026-08-11). No 9.5.4 yet. Carried-forward 9.5 behavior change: `search.batched_query_phase` on by default.
- **BigQuery** (managed, rolling): no headline in-window change confirmed. Carried-forward deprecation: core Graph processing will require **Enterprise / Enterprise Plus** reservations from **2027-04-26** (Standard edition + on-demand billing support removed). BigQuery Graph itself reached **GA on 2026-08-31**.
- **Google Cloud Storage**: **Within the window (early September):** **Storage Insights datasets** can now generate **activity data** (object mutations, errors, and activity trends across projects/buckets/regions); dataset content is queryable via **BigQuery `ObjectRef` functions** (analyze object content/metadata, e.g. sensitive-data detection or image description); **partial-success option** when listing buckets returns reachable buckets plus the names of any temporarily-unavailable ones instead of failing the whole call; **object contexts** (key-value pairs for categorize/track/search) are preserved by default across copy, rewrite, and compose. No CVEs, no breaking changes.

---

### OS and Platform

- **Docker**: **Within the window — Docker Engine 29.8.0 (2026-09-03).** **No CVEs, no breaking changes.** Adds a **`HostConfig.Umask` option and `--umask <octal>` flag** to `docker create` / `docker run`, setting the umask for the container's main process, execs, and healthchecks. Packaging bumps: **BuildKit v0.33.0**, **containerd (static binaries) v2.3.4**. Carried-forward breaking notes: `docker sbom` deprecated for `docker scout sbom`; QEMU backend removed on macOS.
- **Kubernetes**: No release inside the window. **v1.37.0 "Garhwal"** (2026-08-26) remains newest; **1.34 in maintenance mode since 2026-08-27** (EOL 2026-10-27). Next patch train (1.37.1 / 1.36.4 / 1.35.8 / 1.34.11) targeted **2026-09-15**. Carried-forward breaking items: containerd 2.0+ required since 1.35; static Pods can't reference Secrets/ConfigMaps.
- **GCP Compute Engine**: No confirmed in-window release. Standing item: bulletin **GCP-2026-054 / CVE-2026-6726** (TPM 2.0 reference-code OOB read on AMD-based Confidential/Shielded VMs).
- **GKE**: Weekly release channel rollout continued (~release **2026-R38** around 2026-09-09); **no new GKE security bulletin was confirmed for this window.** Carried-forward: bulletin **GCP-2026-058** (2026-09-02, Critical) — GKE Multi-Cloud APIs missing-authorization check, auto-patched server-side, no customer action, no exploitation observed; and the **2026-09-01 firewall-rule change** for GKE **1.35.1-gke.1031000+** (Service rule priority 1000 → 999, new deny rules — audit custom firewall configs before upgrading).
- **GCP Maps API**: No confirmed in-window release. Latest **v3.66.x** (August 2026).
- **GCP Translate API**: No confirmed in-window release. Adaptive translation is GA (added Portuguese, higher character limits, lower latency); romanization/transliteration in Preview. Standing note: migrate off `www.googleapis.com` to `translation.googleapis.com`.
- **GCP Cloud Storage**: see Database and Storage section (Storage Insights activity data, `ObjectRef`, partial-success bucket listing, object contexts).
- **Google Analytics (GA4)**: **Data-quality incident — 2026-09-01 data showed zero traffic** for many properties (widely reported bug, subsequently corrected). No feature release in-window. Carried-forward: since 2026-06-15, Consent Mode (in Google Ads) is the single control for Analytics ads-personalization data.
- **Amazon S3**: No confirmed in-window announcement.
- **Amazon EC2**: **Within the window — 2026-09-09: Graviton5 GA.** **R9g / R9gd** (memory-optimized) instances are generally available, ~25% better compute performance than R8g; **C9g / C9gd** (compute-optimized) also available. Ideal for databases, in-memory caches, and real-time analytics.
- **AWS Lambda**: **Within the window — 2026-09-09:** Lambda **Managed Instances** now support **Graviton5-powered C9g/C9gd/M9g/M9gd EC2 instances**, up to 25% better compute performance than Graviton4. (Managed Instances run Lambda functions on EC2 instances you own while Lambda manages lifecycle, patching, routing, load-balancing, and auto-scaling.)
- **Amazon EKS**: No confirmed in-window announcement. Carried-forward: Kubernetes **1.33 end of standard support** and **1.30 end of extended support** both hit **2026-08-31** — clusters on 1.33 now need an extended-support license token for continued CVE patches. The long-lived-cluster **CA rotation** action item also remains open.
- **Amazon CloudFront**: No confirmed in-window announcement.
- **Amazon Route 53**: No confirmed in-window announcement.
- **AWS (general)**: AWS Weekly Roundup **2026-09-07** — Claude **Fable 5.1** available on AWS (Bedrock); **Amazon Linux 2027** preview; new **AWS Certified AI Business Strategist** certification; SANS-Institute collaboration on securing agentic workloads.
- **Cloudflare**: **Breaking / operational — new AI-crawler defaults go live 2026-09-15.** Traffic is classified as **Search** (allowed), **Training** (blocked by default on ad-serving pages), or **Agent** (blocked by default on ad-serving pages); multi-purpose crawlers are allowed/blocked per the union of their behaviors. Applies automatically to **all new domains, new customers, and existing Free-tier customers** (opt-out window before 09-15); paid customers with existing configs are unchanged. "Pay Per Crawl" is expanding into a **"Pay Per Use"** model (compensation when content surfaces in an AI answer; launch partners Ceramic.ai and You.com). Action: if you run an AI agent/crawler, register and split your user-agents by purpose before the cutoff.
- **Firebase**: **Within the window.** **Admin Node.js SDK v14.3.0** released — new **Firebase SQL Connect** features plus dependency updates in Cloud Firestore and Cloud Storage. **Firebase AI Logic** now supports **US and EU location selection** for Gemini API model access (compliance / latency / availability control). **Imagen APIs removed** from the Firebase AI Logic SDKs and docs (all Imagen models shut down 2026-08-17 — migrate to a Gemini image model / "Nano Banana"). Carried-forward: Firebase Apple SDK stops publishing to CocoaPods after **October 2026**; Remote Config is on usage-based pricing (100,000 free daily fetches/project, then Blaze pay-as-you-go).
- **PayPal**: No confirmed in-window release. Last product changelog was 2026-07-01 (Enterprise Payouts direct-debit return reasons via webhook/API; post-funded payouts with/without FX; self-service REST credential management; Visa AFT payout rails; Twint/Bizum/PayU/Skrill via Braintree orchestration).
- **Apple Pay**: No confirmed in-window release. **iOS 27 GA expected September 2026** — validate PassKit / Apple Pay integrations against the developer beta ahead of GA. WWDC 2026 PassKit changes shipping with the OS: Poster Generic pass style, four additional barcode formats, Featured Actions across all pass types, and Apple's free Pass Designer tool.

---

## Caveats

- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the window" is judged against **2026-09-03 to 2026-09-10**. Items that landed just before the window (Spring's 08-20 wave, the Redis 08-17 train, Kubernetes 1.37.0 on 08-26, Elasticsearch 9.5.3 on 09-03) are reported as carry-forward where they remain the current operative guidance.
- Web-search sources disagreed on a few version numbers (Protobuf 34.x vs 36.0; Guava 33.6.0 vs 33.7.x; exact Netty patch level; exact GKE weekly release number). These are flagged inline as needing direct confirmation against the upstream release page.
- **Open items to carry into next week:** PostgreSQL 19.0 GA (imminent, not shipped as of 09-10); Kubernetes 09-15 patch train (1.37.1 etc.); Java 27 GA (~09-16); Oracle CSPU (~09-15) and next CPU (10-20); Cloudflare AI-crawler default cutover (09-15); iOS 27 / Apple Pay GA; EKS CA rotation for long-lived clusters; Firebase Apple SDK CocoaPods cutoff (October 2026).
