# Weekly Tech Update Report — Week Ending October 1, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-09-25 to 2026-10-01**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, GitHub release pages, and vendor advisories.

## TL;DR

- **Released this week:** Hibernate ORM **7.4.11.Final** (09-27), Maven **3.10.0** (09-27, breaking change), Spring Boot **4.2.0-M2** / Spring AI **2.1.0-M1** (09-25), Kafka **4.2.2** (09-29), MongoDB **9.0** GA (09-29/30, breaking changes), Elasticsearch security patches (09-25), socket.io **4.8.4** (09-25), pg **8.23.1** (~09-30), GKE 512-pods-per-node GA (09-25), Amazon S3 Tables full Iceberg V3 support (09-30), Apple Pay launched in India (09-30), Cloudflare emergency WAF release (09-25).
- **Patch now / act — security:**
  - **Elasticsearch — three new DoS CVEs disclosed 2026-09-25, no workarounds published.** **CVE-2026-94399** (CVSS 6.5), **CVE-2026-94396** (CVSS 6.5), and **CVE-2026-94408** (CVSS 4.9) — all resource-exhaustion/DoS via excessive allocation. Fixed in **8.19.22 / 9.4.7 / 9.5.3–9.5.4**. Upgrade is the only mitigation.
  - **Cloudflare emergency WAF release (2026-09-25)** adds protection for a critical unauthenticated WordPress path-traversal/LFI (**CVE-2026-87902**) and reinforces coverage for the Adobe Commerce "StyleSmuggler" unauthenticated RCE (**CVE-2026-75650**). Confirm managed WAF rules are enabled if you front either stack.
  - **Amazon EKS — NetworkPolicy bypass, CVE-2026-86831** (disclosed just before window, still unresolved for many): pod-identifier collisions in `aws-network-policy-agent` (pre-1.4.0) can bypass namespace isolation. Upgrade agent to **1.4.0+** / VPC CNI managed add-on **v1.22.4+**.
  - **MongoDB — CVE-2026-82067 (CVSS 8.1, unauthenticated full admin access via case-sensitivity handling bug)**, fixed 2026-09-15 in **8.0.30/8.0.32**. Confirm patched — don't let the MongoDB 9.0 GA announcement this week distract from it.
  - **Jackson-databind — CVE-2026-91776 (High)**, disclosed 2026-09-23 (two days before window, still very fresh): unbounded type-ID cache growth in `TypeDeserializerBase` → memory-exhaustion DoS. Fixed in **2.22.3 / 2.21.7 / 2.18.11**.
  - **Docker Sandboxes — CVE-2026-77179** (critical macOS symlink escape) and **CVE-2026-79994** (TOCTOU race), advisory published 2026-09-15 for fixes that shipped in **0.42.0** (2026-09-07). Confirm you're on 0.42.0+ if running agent/AI sandboxed workloads.
  - **Carry-forward, still worth confirming patched:** Spring Framework **CVE-2026-59313** (9.8 Critical, SSE injection in WebMvc.fn) and Spring Security **CVE-2026-59270** (Critical, embedded LDAP admin DN exposed on all interfaces); Sequelize **CVE-2026-30951** (7.5 High, SQLi via JSON-column cast-type interpolation, fixed 6.37.8); express/body-parser **CVE-2026-12590** (Low, DoS via disabled size limit, fixed body-parser 1.20.6/2.3.0, bundled in express 4.22.3).
- **Breaking-change / operational watch:**
  - **MongoDB 9.0 GA'd 2026-09-29/30** with real breaking changes: null-comparison semantics changed for dotted-path queries, a new cap on concurrently open multi-document transactions for external clients, and a renamed metric. Review before upgrading.
  - **Maven 3.10.0 (2026-09-27)** flips dependency classpath resolution from **depth-first to breadth-first** ordering to align with Maven 4/Resolver 2.x — can silently change which version wins in diamond dependency conflicts. Re-verify builds after upgrading.
  - **PostgreSQL 19 has slipped again**: RC1 now targeted **2026-10-15**, GA **2026-10-29** (confirmed 2026-09-28). Beta 4 (09-24) dropped six planned features including SQL/PGQ graph queries.
  - **Kubernetes 1.34 reaches end of life 2026-10-27** — upgrade planning should start now. Current patch train (1.37.1/1.36.5/1.35.9/1.34.12, 09-15) also closed **CVE-2026-39821** (CVSS 9.6, Punycode-normalization bypass in `golang.org/x/net/idna`).
  - **Google Maps Places SDK for Android 6.0.2 (2026-09-30)** removes several methods (`findCurrentPlace`, `fetchPlace`, Autocomplete-related methods) — breaking for Android integrations; audit before upgrading.
  - **GKE's managed ISTIOD control plane for Cloud Service Mesh was deprecated 2026-09-28** — confirm the exact support-end date against Google's own docs before planning migration (sources disagreed on the year).
  - **React Native 0.88 is in RC** (rc.3 cut 09-28), **GA scheduled 2026-10-12** — carries forward 0.87's breaking changes (Strict TypeScript API default, deep-import removal, `InteractionManager` removed).
  - **Spring's next generation baselines on Java 27**: Spring Boot 4.2.0-M2, Spring AI 2.1.0-M1, and Spring Cloud 2026.0.0-M1 all landed 09-24/09-25 as the first milestones targeting JDK 27 (Boot 4.2 GA expected November 2026).
  - Carried forward: Oracle's next quarterly CPU is **2026-10-20**; Amazon EKS/GKE continue their Kubernetes 1.34 patch trains ahead of the 10-27 EOL; Firebase Apple SDK stops publishing to CocoaPods after **October 2026**.

---

### Java

**Window checked:** 2026-09-25 to 2026-10-01. **Only Hibernate ORM and Maven shipped inside this window** — most of the rest of the ecosystem released in the few days just before it (09-21 through 09-27 milestone wave) and is carried forward. No new CVEs were *disclosed* inside the window; the freshest security item (Jackson CVE-2026-91776) landed two days before it.

**Java (core / JDK)**
- No release inside the window. Latest GA remains **Java 27** (GA 2026-09-15, now carried forward two weeks running). Next scheduled activity is the **October 2026 CPU/CSPU on 2026-10-20** (JDK 8u512, 11.0.33, 21.0.13) — outside this window, watch for next week's report.

**Netty**
- No release this window. Latest remains **4.2.18.Final / 4.1.138.Final** (2026-09-09, well before window). Notable from that release still relevant: HTTP/2 header **name and value validation are now both opt-in by default** (behavior change, not a hard break), an `SslHandler` buffer-leak fix on OOME during allocation, and an `AsciiString.cached(String)` sanitization/perf fix.

**Vert.x**
- No release inside the window. **Vert.x 5.2.0 GA landed 2026-09-21** (4 days before window) — carried forward. Headline: the Vert.x 4 eventbus JavaScript client is now a standalone package usable outside the full framework.

**Spring (Framework / Boot / Cloud / Security / AI / Modulith)**
- **In window:** **Spring Boot 4.2.0-M2** and **Spring AI 2.1.0-M1** were both published **2026-09-25** (day one of the window). Boot 4.2.0-M2 adds SSL-bundle/LDAPS support for the embedded LDAP server and an OTLP/Micrometer semantic-convention switch; it's the first Boot line targeting **Java 27** (GA expected November 2026). Spring AI 2.1.0-M1 moves to a structured `MessagePart` content model (Text/Reasoning/ToolCall/ToolResult/Media/Unknown parts) and adds OpenAI Responses API support.
- **Just before window (not counted as in-window):** Spring Cloud 2026.0.0-M1 "Paddington" (09-24, adds untrusted-proxy request denial in `ForwardedHeadersFilter`/`XForwardedHeadersFilter`), Spring Framework 7.1.0-M2 (09-24), Spring Security 7.2.0-M2 (week of 09-21).
- **No new CVEs disclosed this window.** The most severe recent/unpatched items remain, all pre-window — flag if not yet remediated:
  - **CVE-2026-59313** — Spring MVC functional web framework SSE injection via bare carriage-return in response text, allows forging/truncating another user's event stream. **CVSS 9.8 Critical.** Affects 5.3.0–5.3.49, 6.0.0–6.0.30, 6.1.0–6.1.28, 6.2.0–6.2.19, 7.0.0–7.0.8. Disclosed 2026-08-27.
  - **CVE-2026-59270** — Spring Security's embedded UnboundID LDAP server binds the well-known admin DN (`uid=admin,ou=system`) on all network interfaces, letting any network-reachable attacker authenticate and modify the in-memory directory. Critical. Disclosed 2026-08-20.
  - **CVE-2026-59283** — SpEL compiler silently compiles expressions evaluated in a `SimpleEvaluationContext`, bypassing safety guards on subsequent evaluations (unbounded class-loading growth). Affects 5.2.25–7.0.8. Disclosed ~2026-09-09.
  - **CVE-2026-47892** — WebFlux functional endpoints header-predicate authorization bypass. Disclosed 2026-08-20/27.
- Spring Modulith: no window release; latest is **2.2.0-M1 / 2.1.1 / 2.0.8 / 1.4.13** (2026-08-26), carried forward.

**Hibernate**
- **In window.** **Hibernate ORM 7.4.11.Final** released **2026-09-27** — fixes a `StatelessSession` upsert/`upsertMultiple` concurrency bug (HHH-20894), a missing `Interceptor#onLoad` call in `StatelessSession` (HHH-20876), incorrect silent-drop of `MERGE ... ON CONFLICT DO NOTHING` on H2/Oracle/SQL Server (HHH-20826), and an enum-as-string binding fix (HHH-17020). Also **8.0.0.Beta3** (dev line, Jakarta Persistence 4.0) shipped **2026-09-25**. (7.2.25.Final 09-17 and 6.6.58.Final 09-20 are the limited-support lines, both just before window.)

**JDBC drivers (pgjdbc, MySQL Connector/J)**
- No release this window for either. **pgjdbc** latest is **42.7.13** (2026-07-07), carried forward. **MySQL Connector/J** latest is **26.7.0** (2026-07-29, new calendar-versioned line superseding the 9.x series), carried forward.

**Protobuf**
- No Java artifact release inside the window. Latest **protobuf-java 4.36.2 / 7.36.2** (~2026-09-17/18), carried forward. Worth flagging: a **breaking-changes notice for v38** was published 2026-09-22 (just before window) — drops Bazel 8 as minimum (raises to Bazel 9), bumps C++ major version 7.37.x → 8.38.x, and makes `[[nodiscard]]` permanent. Targeted for 2027 Q1; plan ahead but no action needed yet.

**Dagger (Google DI)**
- No release this window. Latest **2.60.1** (2026-07-07), carried forward.

**Jackson (databind / core)**
- No point release landed exactly inside the window, but a fresh High-severity CVE published **2026-09-23 (two days before window — flagged, not counted as in-window)**:
  - **CVE-2026-91776** — `TypeDeserializerBase._findDeserializer()` caches resolved deserializers under attacker-supplied raw type IDs, causing unbounded type-id cache growth (memory-exhaustion DoS). **High.** Fixed in **2.22.3, 2.21.7, 2.18.11** — patch promptly given proximity to this window.
  - Also still active/relevant from earlier in 2026: **CVE-2026-54515** (Medium, case-insensitive binding reopens `@JsonIgnoreProperties`-excluded fields, fixed 2.18.9/2.21.5/3.1.4, disclosed 06-23), **CVE-2026-54512/54513** (High 8.1, `BasicPolymorphicTypeValidator` bypass enabling gadget instantiation, disclosed early June), **CVE-2026-54514** (Medium, eager DNS resolution on `InetSocketAddress` deserialization, fixed 2.18.8).
  - Current lines: legacy `com.fasterxml.jackson.core` 2.x (2.22.x/2.21.x active, 2.20 branch closed at 2.20.2 in Jan 2026) and new `tools.jackson.core` 3.x (3.2.3, 2026-06-09).

**Log4j**
- No release or CVE this window. Most recent is **CVE-2026-49844** (JSON encoding vulnerability, affects ≤2.25.4, fixed in **2.25.5**, disclosed 2026-07-16), carried forward.

**Guava**
- No release this window. Latest **33.7.1** (2026-08-19) — fixed a Multi-Release jar manifest regression under Java 9/10 introduced in 33.7.0. Carried forward.

**JUnit**
- No release this window. Latest GA is **JUnit 6.1.3** (2026-08-07/08), carried forward.

**Maven**
- **In window.** **Maven 3.10.0** released **2026-09-27** — uses Resolver 2.x, allows user-wide/installation-wide extensions, enables Reproducible Builds by default, adds CLI update-policy controls. **Breaking change:** dependency **classpath ordering changed from pre-order (depth-first) to level-order (breadth-first)** to align with Maven 4/Resolver 2.x defaults — this can change which version "wins" in diamond dependency conflicts; re-verify builds after upgrading. **Maven 4.0.0-rc-7** shipped 2026-09-24, just before window.

**Gradle**
- No release this window. **Gradle 9.8.0** (2026-09-24, just before window) carried forward — adds **Java 27 support** (daemon + toolchains), Maven mirror-settings reuse, and a fix so the Maven Publish Plugin participates in up-to-date checks instead of regenerating unchanged POMs.

---

### Node.js

**Window checked:** 2026-09-25 to 2026-10-01. **Socket.IO and pg (node-postgres) were the only libraries in this stack with confirmed in-window releases** — everything else was quiet inside the window, with most recent activity clustering 09-09 through 09-23, just before it.

- **Node.js core**: No release inside the window. Latest remains **v26.10.0 (Current)**, 2026-09-22 — adds `crypto.parsePKCS12()`, VFS-mounted library loading for FFI, `net.BoundSocket` transfer to threads/child processes, and `SlidingWindowHistogram`/qrde analysis in `perf_hooks`. LTS lines: **v24.21.0 "Krypton"** (2026-09-09, OpenSSL 3.5.8 + NSS root-cert refresh) and **v22.23.3** (2026-09-23, maintenance LTS). No new CVEs disclosed this window; most recent security batch was the July 2026 release (11 vulnerabilities, 3 High across the 26.x/24.x/22.x lines) — confirm that's applied if not already.
- **axios**: No release this window. Latest **v1.20.0** (2026-08-19) — hardens runtime option handling, adds RFC 9110 status-code aliases, fixes Node/XHR reliability issues. (A legacy-branch patch, **v0.34.0**, landed 2026-09-13 — also before window.) No new CVEs; the March 2026 malicious-package supply-chain incident (fake 1.14.1/0.30.4) remains fully resolved and is not relevant to this window.
- **bcryptjs**: No release this window — stale carry-forward. Latest remains **v3.0.3** (2025-11-02, ~11 months old). No CVEs against bcryptjs itself; be aware of the adjacent pattern of consumers not enforcing the 72-byte truncation limit bcryptjs silently applies, if auditing password-hashing call sites.
- **express**: No release this window. Latest **v4.22.3** (2026-09-14) bundles `body-parser@^1.20.6`/`^2.3.0`, which fixes **CVE-2026-12590** (CVSS 3.1 Low): an invalid `limit` option (unparseable string/NaN) silently disabled body-size enforcement, enabling DoS via oversized payloads. Affected: body-parser &lt;1.20.6 (1.x) / &lt;2.3.0 (2.x). Fixed: 1.20.6 / 2.3.0. Disclosed 2026-07-09 — well before window, but flag as carried-forward if not yet patched, since it's a quiet one to miss.
- **moment**: No release this window. Latest **v2.31.0** (2026-09-15) — routine maintenance; project remains in legacy/maintenance mode, no new features accepted. (moment-timezone v0.6.3, 2026-07-19, updated IANA TZDB to 2026c.)
- **pg (node-postgres)**: **In-window release** — **v8.23.1**, published ~2026-09-30. Follow-up patch to v8.23.0's opt-in query-pipelining feature (2026-08-08), fixing error handling for exceptions during value parsing. No CVE associated; low-risk, recommended bump.
- **sequelize**: No release this window. Latest stable remains **v6.37.8** (2026-03-07), which fixed **CVE-2026-30951** (SQL injection via JSON column cast-type interpolation in `_traverseJSON()`, CVSS 3.1: 7.5 **High**). Affected: 6.0.0-beta.1 through &lt;6.37.8. Fixed: 6.37.8. v7 (`@sequelize/core`, currently alpha.48) is not affected. Old news by date, but severe enough to re-flag if any service is still pinned below 6.37.8.
- **socket.io**: **In-window release** — **v4.8.4** (server + client), published 2026-09-25. Bug fixes include memory-leak prevention and type corrections; `ws` dependency bumped to `~8.21.0` following **CVE-2026-48779**. Companion releases: `engine.io-client@6.6.7` (09-25, in window) and `engine.io@6.6.11` (09-24, just outside). Also worth noting: `@socket.io/cluster-engine@0.1.1` (2026-09-08, before window) guarded client lookups against prototype pollution — carried-forward security fix worth confirming is picked up.
- **jest**: No release this window. Latest **v30.5.2** (2026-09-18) — TypeScript type stripping for Node, package.json bundling fixes, source-map resolution fixes, `jest-each` escaping improvements.
- **prettier**: No release this window. Latest **v3.9.9** (2026-09-23, two days before window) — fixed markdown text containing `$` being misparsed as math syntax. (v3.9.8 09-17, v3.9.7 09-16 added Angular 22.2 support.)
- **ReactJS**: No release this window. Latest **v19.3.0** (2026-09-09) — ViewTransition and Fragment refs now stable, new `browser()` react-dom API, Trusted Types integration, transitions now render independently instead of being entangled into one render. No breaking changes called out; minor/non-breaking release.
- **React Native**: No *stable* release this window. Latest stable remains **v0.87** (2026-08-11) with breaking changes: Strict TypeScript API now default, deep imports into `react-native/Libraries/*` now a type error, `InteractionManager` removed, `@react-native/assets-registry` deprecated, `ImageBackground` deprecated, raised minimum toolchain (Node 22, AGP 9, Kotlin 2.0+). **0.88 is mid-RC inside the window** (rc.3 cut 2026-09-28, branch cut 2026-09-07), with stable GA scheduled for 2026-10-12 — outside this window but worth pre-flagging for next week's upgrade planning.

---

### Database and Storage

- **Oracle Database**: No release inside the window. Oracle AI Database **26ai** (release-update line 23.26.x; latest RU **23.26.3**, July 2026) remains current — no new RU shipped this window. September **CSPU shipped 2026-09-15** (carried forward, outside window): 673 patches covering 672 CVEs, 100+ critical, 240+ remotely exploitable without authentication; heaviest in E-Business Suite (159), Fusion Middleware (153), Hyperion (102). Next quarterly **CPU is 2026-10-20**.
- **PostgreSQL**: No GA/point release inside the window, but a schedule update landed **within the window on 2026-09-28**: the Release Management team confirmed **RC1 is now targeted for 2026-10-15** and **GA for 2026-10-29** — PG19 has slipped again past the earlier "end of October" estimate (though it's still landing in October). Beta 4 itself shipped 2026-09-24, just before the window, and dropped six previously-planned features including the long-anticipated SQL/PGQ graph query support. Most recent security fix (carried forward, outside window): the **2026-08-13** quarterly update (18.6/17.11/16.15/15.19/14.24) patched **CVE-2026-6471 "PostGREShell"** (CVSS 7.2 — arbitrary code execution via logical decoding, present since PG 9.4/2014, requires REPLICATION role + `wal_level=logical`), plus CVE-2026-6472/6473/6474 (search_path type hijack, integer-wraparound OOB write, format-string info leak in `timeofday()`). Next quarterly update expected November 2026.
- **Redis**: No release inside the window. Latest is **8.10.2** (2026-09-17, carried forward) — a security release fixing **CVE-2026-62356** (miscalculated buffer size in CMSketch RDB loading → heap out-of-bounds write) plus an OOB read in TopK heap cleanup, a use-after-free in the TLS pending-data list, a malicious-RDB `SLOT_INFO` path that can lead to RCE, and use-after-free issues in Vector Sets (`VREM` mutating the HNSW graph). All fixed in 8.10.2; no newer build shipped by 2026-10-01.
- **Apache Kafka**: **Within the window — Kafka 4.2.2 released 2026-09-29.** Bugfix release, ~33 issues resolved since 4.2.1, no CVEs. Notable fixes: `ListDeserializer` silently deserializing truncated/corrupted entries (KAFKA-20769), an exactly-once-semantics checkpoint bug that could skip state-store wipe after an unclean crash (KAFKA-20685), a deadlock in `KafkaStreams.removeStreamThread` racing a `REPLACE_THREAD` handler (KAFKA-20873), consumer-group downgrades leaving on-disk coordinator state permanently unloadable (KAFKA-20845), and a high-CPU loop after failed re-authentication (KAFKA-20253).
- **MongoDB**: **Within the window — MongoDB 9.0 launched 2026-09-29/30** at MongoDB's developer conference, alongside Atlas Agent Engine (GA) and Atlas Infinite (public preview). Claims up to 2x throughput, 35% faster reads, 30% faster updates. **Breaking changes to review before upgrading**: null-comparison semantics changed for dotted-path queries, a new cap on concurrently open multi-document transactions for external clients, a renamed metric (`metrics.changeStreams.showExpandedEvents` → `...option.showExpandedEvents`), and Enterprise Advanced now receives only an annual Feature Update between major releases (no more sequential minor-version hopping requirement). Security (carried forward, just outside window): **CVE-2026-82067** (CVSS **8.1** — improper case-sensitivity handling enabling unauthenticated full admin access; affects Server 7.0 &lt;7.0.41 and 8.3 &lt;8.3.9) was fixed 2026-09-15 via 8.0.30/8.0.32 — patch now if still unpatched.
- **Apache Cassandra**: No release inside the window — **5.0.9 (2026-08-07)** remains latest GA, carried forward. Cassandra 6.0-alpha2 (also 2026-08-07) continues pre-GA with Accord transactions, Transactional Cluster Metadata, automated repair, constraints framework, Zstd dictionary compression, and cursor-based compaction; GA still targeted for H2 2026.
- **Elasticsearch**: **Within the window — three DoS advisories published 2026-09-25 (first day of window).** ESA-2026-180 / **CVE-2026-94399** (CVSS 6.5, uncontrolled resource consumption via excessive allocation; affects 8.0.0–8.19.21 and 9.0.0–9.4.6/9.5.0–9.5.3; fixed in **8.19.22 / 9.4.7 / 9.5.4**). ESA-2026-182 / **CVE-2026-94396** (CVSS 6.5, same CWE-400 class; affects 9.2.0–9.4.6 and 9.5.0–9.5.3; fixed in **9.4.7 / 9.5.4**). ESA-2026-184 / **CVE-2026-94408** (CVSS 4.9, DoS via excessive allocation; affects 8.0.0–8.19.21 and 9.0.0–9.4.6/9.5.0–9.5.2; fixed in **8.19.22 / 9.4.7 / 9.5.3**). No workarounds published for any of the three — upgrade is the only mitigation.
- **BigQuery**: **Within the window — several rolling GA updates, 2026-09-28 through 2026-09-30.** BigQuery **Security Center reached GA on 2026-09-30** (in-console data security profile analysis, row-/column-level security policy management, data governance/policy tags). **BigQuery Data Transfer Service MCP server reached GA on 2026-09-28.** Continuous queries can now write directly into **Apache Iceberg managed tables via `INSERT` DML** (2026-09-28). `bq update --connection` gained the ability to clear `serviceDirectoryService` on AWS/Azure cross-cloud connections (2026-09-29).

---

### OS and Platform

- **Docker**: No release inside the window. Latest remains **Docker Engine 29.8.1** (2026-09-16, before window). Worth flagging regardless: on 2026-09-15 Docker published an advisory for **Docker Sandboxes** covering a critical macOS symlink-escape (**CVE-2026-77179**) and a high-severity Unix-socket-relay TOCTOU race (**CVE-2026-79994**); both were actually fixed in the 0.42.0 release that shipped 2026-09-07, just the CVE records/advisory landed later. Confirm you're on 0.42.0+ if you use Docker Sandboxes for agent/AI workloads.
- **Kubernetes**: No release inside the window. Latest patch train is **1.37.1 / 1.36.5 / 1.35.9 / 1.34.12** (shipped 2026-09-15), which incorporated the fix for **CVE-2026-39821** (Punycode-normalization bypass in `golang.org/x/net/idna`, CVSS 9.6, via Go 1.26.6+). Next patch train (1.37.2 etc.) is targeted for 2026-10-13. **Approaching EOL: Kubernetes 1.34 reaches end of life on 2026-10-27** — upgrade planning should start now.
- **GCP Compute Engine**: No GA/preview feature confirmed strictly inside the window. Most recent prior item: a preview (2026-09-24, just before window) to protect standard snapshots from accidental/malicious deletion via a 3-day recycle bin. Carried forward.
- **GKE**: **Within the window.** GKE Standard now supports up to **512 max Pods per node** (up from 256), GA 2026-09-25. The storage-optimized **Z4D machine series** became available for clusters on 1.36.3-gke.1244000+ (2026-09-28). **Breaking/deprecation**: Cloud Service Mesh support for the **managed ISTIOD control plane** was deprecated 2026-09-28 — confirm the exact support-end date against Google's own docs (sources disagreed) before planning migration.
- **GCP Maps API**: **Within the window.** Places SDK for Android **6.0.2** released 2026-09-30 with **breaking changes**: several methods/fragments removed, including `PlacesClient.findCurrentPlace`, `PlacesClient.fetchPlace`, and Autocomplete-related methods. Audit Android integrations before upgrading.
- **GCP Translate API**: No release inside the window. Latest remains `google-cloud-translate` v3.27.0 (2026-06-22) and Translation LLM full fine-tuning with LoRA (2026-02-24). Carried forward.
- **GCP Cloud Storage**: **Within the window.** 2026-09-29: the `gcloud storage buckets anywhere-caches pause` command was **removed** (operational/breaking for anyone scripting against it). 2026-09-30: client libraries now provide automated end-to-end checksumming by default on read/write for data-integrity.
- **Google Analytics (GA4)**: **Within the window.** 2026-09-29: app conversions are now fully supported in cross-channel conversion reporting (performance, attribution analysis, attribution models), with independently adjustable attribution settings for app vs. web conversions.
- **Amazon S3**: **Within the window.** 2026-09-30: S3 Tables now support the **full Apache Iceberg V3 spec** — variant type, nanosecond timestamps, geometry/geography, deletion vectors, and row lineage — with in-place V2→V3 upgrade and built-in compaction/maintenance.
- **Amazon EC2**: No release inside the window. Most recent: **T8i** burstable instances (Intel Xeon 6, up to 30% better price-performance vs. T3) reached GA on 2026-09-17, just before window. Carried forward.
- **AWS Lambda**: No release inside the window. Most recent: **90-minute function timeout** for async/event-source-mapping invocations on Lambda Managed Instances (6x increase from 15 min), announced 2026-09-09. Synchronous invocations are still capped at 15 minutes. Carried forward.
- **Amazon EKS**: **Within the window.** EKS Distro `v1-34-eks-23` released 2026-09-25; Kubernetes 1.34 platform version `1.34.11-20260930` released 2026-09-30. **Security note (just before window, still actionable)**: **CVE-2026-86831** — a NetworkPolicy enforcement bypass in the `aws-network-policy-agent` (pre-1.4.0) allowing pod-identifier collisions to bypass namespace isolation — published 2026-09-17. Upgrade to agent 1.4.0+ / VPC CNI managed add-on v1.22.4+.
- **Amazon CloudFront**: No release inside the window. Most recent: four new Dynamic Image Transformation features (smart cropping, multi-tier device-aware auto-optimization), announced 2026-09-08. Carried forward.
- **Amazon Route 53**: No release inside the window. Most recent material items: Accelerated Recovery for public DNS records (60-min RTO during us-east-1 disruptions) and Route 53 Global Resolver GA (2026-03-09). Carried forward.
- **Cloudflare**: **Within the window.** Emergency WAF release on **2026-09-25** added protection against a critical WordPress path-traversal/LFI flaw (**CVE-2026-87902**, unauthenticated arbitrary file read) and reinforced coverage for the Adobe Commerce "StyleSmuggler" unauthenticated RCE (**CVE-2026-75650**) from earlier in September. If you run WordPress or Adobe Commerce behind Cloudflare, confirm the relevant managed WAF rules are enabled.
- **Firebase**: **Within the window.** Firebase CLI **v15.32.0** released 2026-09-28: introduces function kits, support for overriding Secret Manager secret IDs/versions in Cloud Functions `.env` files, and migration tooling for Firebase Extensions. Standing note: Apple SDK stops publishing to CocoaPods after **October 2026**.
- **PayPal**: No developer/API release inside the window — late-September PayPal news was PR/marketing only. Most recent developer-facing changes remain Self-Managed API Credentials (2026-03-24) and the OpenAPI 3.0 spec publication. Carried forward.
- **Apple Pay**: **Within the window.** Apple Pay launched in **India** on 2026-09-30, available on iPhone/iPad/Apple Watch (Mac "coming soon"), initially limited to Axis Bank-issued Visa/Mastercard cards — RuPay and Amex are not yet supported at launch.

---

## Caveats

- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the window" is judged against **2026-09-25 to 2026-10-01**. Several items landed in the few days just before the window (Vert.x 5.2, Spring Cloud/Framework/Security milestones, Gradle 9.8.0, Jackson CVE-2026-91776, Kubernetes 1.37.1 patch train, Docker Sandboxes advisory) and are reported as carry-forward where they remain the current operative guidance.
- Sources disagreed on the exact support-end date for GKE's managed ISTIOD deprecation — confirm directly against Google's Cloud Service Mesh docs before relying on it for migration planning.
- **Open items to carry into next week:** PostgreSQL 19 RC1 (now 2026-10-15) and GA (2026-10-29); Kubernetes 1.34 EOL (2026-10-27); Oracle's next quarterly CPU (2026-10-20); React Native 0.88 GA (2026-10-12); Kubernetes next patch train (~2026-10-13); Spring Boot 4.2 GA (expected November 2026); Firebase Apple SDK CocoaPods cutoff (October 2026).
