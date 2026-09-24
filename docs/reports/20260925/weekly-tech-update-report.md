# Weekly Tech Update Report — Week Ending September 25, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-09-18 to 2026-09-25**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, GitHub release pages, and vendor advisories.

## TL;DR

- **Released this week:** Vert.x **5.2** (2026-09-21), Kubernetes **v1.37.1 / v1.36.4 / v1.35.8 / v1.34.11** patch train (~2026-09-23), PostgreSQL **19 Beta 4** (2026-09-24, GA now slipping to **end of October**), Prettier **3.9.9** (2026-09-23), plus rolling platform updates to GCP Compute Engine (Backup and DR API auto-enable), EKS Distro (`v1-37-eks-3`), and Google Cloud Translation. **Java's core (post-JDK 27), Node.js core, most of the Node/DB library stack, Redis, Kafka, Docker, and Cassandra had no release inside this window** — most landed just before it and are carried forward.
- **Patch now / act — security:**
  - **Jackson-databind — two new High-severity CVEs, disclosed 2026-09-23, inside this window.** **CVE-2026-91776** (CVSS 7.5, unbounded growth of the type-ID cache in `TypeDeserializerBase`, attacker-controlled raw type IDs retained indefinitely → memory-exhaustion DoS) and **CVE-2026-91777** (High, quadratic forward-reference completion in `@JsonIdentityInfo`-based Collection/Map deserializers → CPU-exhaustion DoS). **Fixed in jackson-databind 2.18.11, 2.21.7, 2.22.3, and tools.jackson (3.x) 3.1.7 / 3.2.3.** Upgrade if you deserialize untrusted JSON with polymorphic types or object-identity references.
  - **Carry-forward — Spring Framework SSE injection, CVE-2026-59313 (disclosed 2026-09-10, just before this window, still the operative new item).** A bare carriage return in attacker-influenced text streamed via `ServerResponse.sse(...)` (WebMvc.fn) is written verbatim, letting a browser reinterpret it as new SSE fields. Scanners flag it **9.8 Critical**; Spring itself rates it **Low**. **Only the Spring Framework 7.0.x branch gets a free open-source fix** (upgrade to a patched 7.0.x); 5.3.x/6.0.x/6.1.x/6.2.x fixes are Commercial-Support-Only. Confirm your branch before relying on scanner severity alone.
  - **MongoDB — CVE-2026-89099 (race condition in the document-value layer), out-of-band fix 2026-09-11 (just before window):** fixed in **8.3.11** and **8.0.32**. Patch if still below those.
  - **Redis** — no new CVE this window; latest is **8.10.2** (2026-09-17, just before window). The Aug-17 coordinated-train fix for **CVE-2026-81934** (TLS use-after-free, unauthenticated RCE) remains the operative patch floor — confirm applied.
  - **Netty** — no new CVEs this window. The **CVE-2026-75595** (CVSS 9.1, SNI/mTLS bypass via SslClientHelloHandler offset bug) and **CVE-2026-75596** (CVSS 8.7, quadratic-CPU ClientHello flood) set, publicized this cycle, is **already fixed in 4.1.137.Final / 4.2.17.Final** (early August) — confirm you're at or above those floors.
- **Breaking-change / operational watch:**
  - **PostgreSQL 19 GA has slipped out of September.** Beta 4 shipped 2026-09-24; GA is now targeted for **"by the end of October."** The `GRAPH_TABLE`/SQL-PGQ revert from two weeks ago stands — earliest return is PG20 (Sept 2027).
  - **Java 27 GA landed 2026-09-15** (just before window) — non-LTS, nine JEPs including **quantum-resistant hybrid TLS 1.3 key exchange**, **compact object headers on by default** (10–20% heap reduction, 5–10% throughput gain), and **G1 as the default GC everywhere** (including small machines that previously got Serial GC). Review GC behavior changes before adopting on memory-constrained services. Oracle's next **CPU is 2026-10-20**; the September **CSPU landed 2026-09-15**.
  - **Kubernetes 1.34 remains in maintenance mode** (critical-fixes-only) ahead of its **2026-10-27 EOL** — the 1.34.11 patch just released is likely one of its last.
  - **Netty 4.1 end-of-life announced: 2027-07-01.** Plan migration to the 4.2.x line ahead of that date.
  - **Google Cloud Translation API**: DOC/PPT/XLS document-translation support, native-PDF page limit raised to 300, and multi-regional US endpoint now **GA** — release notes refreshed 2026-09-18. Still-standing note: migrate off `www.googleapis.com` to `translation.googleapis.com`.
  - **GCP Compute Engine**: **Backup and DR API now auto-enables** (from 2026-09-21) whenever Compute Engine, Cloud SQL Admin, AlloyDB, or Filestore APIs are enabled in a project — review IAM/billing implications if you don't want it on. **NVIDIA P100 GPUs reached end of support 2026-09-15** (just before window) — migrate to G2/G4 machine series.
  - Carried forward: Cloudflare's AI-crawler default classification (Search/Training/Agent) went live **2026-09-15**; Firebase Apple SDK stops publishing to CocoaPods after **October 2026**; iOS 27 GA (2026-09-14/15) shipped a redesigned Apple Pay checkout with swipeable multi-card selection — validate PassKit integrations against it; EKS clusters still on Kubernetes 1.33 need an extended-support license token following the 2026-08-31 standard-support end.

---

### Java

**Window checked:** 2026-09-18 to 2026-09-25. **Only Vert.x and Kubernetes-adjacent Java tooling shipped inside this window** — most of the ecosystem released just before it (09-09 through 09-15) and is carried forward.

**Java (core / JDK)**
- No release inside the window. **Java 27 GA landed 2026-09-15** (just before window) — non-LTS, six-month support line, nine JEPs. Headliners: **quantum-resistant hybrid key exchange for TLS 1.3**, **compact object headers enabled by default** (96-bit → 64-bit headers, ~10–20% heap reduction, 5–10% throughput improvement), **G1 as the default garbage collector on all machine sizes** (previously small machines defaulted to Serial GC), and updated **ML-KEM/ML-DSA** private-key encodings with performance gains for ML-KEM, ML-DSA, X25519, and Ed25519. **Java 25 LTS** remains the current long-term line; next LTS is **Java 29** (Sept 2027). Oracle's September **CSPU shipped 2026-09-15**; next quarterly **CPU is 2026-10-20**.
- Amazon Corretto 27 (AWS's OpenJDK distribution) was called out in AWS's 2026-09-21 weekly roundup, tracking the JDK 27 GA.

**Netty**
- No release this window. Latest remains **4.2.18.Final / 4.1.138.Final** (2026-09-09, just before window). **Netty 4.1 end-of-life announced for 2027-07-01** — start planning the 4.2.x migration. Standing fix set (already patched, confirm you're current): **CVE-2026-75595** (CVSS 9.1, SNI/mTLS-bypass via ClientHello offset bug) and **CVE-2026-75596** (CVSS 8.7, quadratic-CPU ClientHello flood), both fixed in 4.1.137.Final/4.2.17.Final; older **CVE-2026-33870** (HTTP/1.1 chunked-extension request smuggling, fixed 4.1.132.Final/4.2.10.Final) and **CVE-2026-33871** (HTTP/2 CONTINUATION-flood DoS).

**Vert.x**
- **Within the window — Vert.x 5.2 released 2026-09-21** with new features (exact feature list needs direct confirmation against the release blog). Also just before window: **4.5.34 / 5.1.8** (2026-09-10, bug fixes plus vulnerability fixes) and **4.5.33 / 5.1.7** (2026-09-01, bug fixes). No CVEs disclosed against the 5.2 release itself at time of writing.

**Spring (Framework / Boot / Cloud / AI / Modulith)**
- No new release inside the window. Spring Boot remains at **4.1.1 / 4.0.8**; Spring Framework at **7.0.9**.
- **New action item — CVE-2026-59313 (Spring Framework, disclosed 2026-09-10, just before window): Server-Sent Events injection in WebMvc.fn.** A bare `\r` in attacker-influenced SSE payload text is written verbatim, letting a browser reparse the rest of the stream as new SSE fields. Affects **5.3.0–5.3.49, 6.0.0–6.0.30, 6.1.0–6.1.28, 6.2.0–6.2.19, 7.0.0–7.0.8**. **Only 7.0.x gets a free OSS fix** (patched 7.0.x release) — all earlier branches require Commercial Support. Scanner severity (9.8) diverges sharply from Spring's own (Low); triage based on your actual SSE usage.
- **Carry-forward:** the 2026-08-20 ~91-CVE portfolio wave, including **CVE-2026-59270** (Spring Security embedded UnboundID LDAP, CVSS 9.4, fixed in Spring Security 7.1.1/7.0.7) and **CVE-2026-59285** (Spring for GraphQL unsafe deserialization, CVSS 9.2), remains the current floor to confirm.

**Hibernate**
- No release this window. Latest remains **6.6.57.Final** (2026-09-13, just before window); **7.4.x** is current stable, **8.0** still in Beta. No CVEs.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06). No release/CVE this window.
- **MySQL Connector/J**: sources disagree on the current numbering — one tracker shows **9.7.0** (2026-04-22), another shows a renumbered **26.7.0** (2026-07-29) line. Treat the exact latest as needing direct confirmation against `dev.mysql.com/doc/relnotes/connector-j`. No new CVEs.

**Protobuf**
- No release inside the window. Sources disagree on the exact current line — Wikipedia cites **v35.0** (2026-05-19) as stable, while the GitHub releases page shows a newer **v34.1** tag referenced as "latest." Treat the exact current version as needing direct confirmation against `protocolbuffers/protobuf` releases. No CVEs.

**Dagger**
- No release this window. Latest remains **2.60.1** (2026-07-06). No CVEs or breaking changes.

**Jackson**
- **Within the window — two new High-severity CVEs disclosed 2026-09-23:**
  - **CVE-2026-91776** (CVSS 7.5): `TypeDeserializerBase._findDeserializer()` caches the resolved deserializer under the raw, attacker-supplied type ID, causing **unbounded growth of the type-ID cache** — a memory-exhaustion DoS vector for services deserializing untrusted polymorphic JSON.
  - **CVE-2026-91777** (High): forward-reference completion for `@JsonIdentityInfo` object IDs performs a **linear scan of the pending-reference accumulator per resolved ID**, giving quadratic CPU cost for Collection/Map deserializers under crafted input.
  - **Fixed in jackson-databind 2.18.11, 2.21.7, 2.22.3, and tools.jackson (Jackson 3.x) 3.1.7 / 3.2.3.** Upgrade if you deserialize untrusted JSON using polymorphic types or `@JsonIdentityInfo`.
- Standing set still open for laggards: **CVE-2026-54512/-54513/-54514** (PolymorphicTypeValidator bypasses, CVSS 8.1), **CVE-2026-54515** (case-insensitive deserialization reopens `@JsonIgnoreProperties`-protected fields), **CVE-2026-59889** (`@JsonView` bypass via `@JsonUnwrapped`).

**Log4j**
- No release this window. Latest remains **2.26.0** (2026-05-07). Standing items: **CVE-2026-34480** (`XmlLayout`/`Log4jXmlObjectMapper` silently drops non-finite-value log fields, Medium, fixed 2.25.4+) and **CVE-2026-34481** (`MapMessage.asJson()` emits invalid JSON tokens for non-finite IEEE-754 values, affects 2.13.1–2.25.4 and 2.26.0). Confirm you're on a fixed line if you rely on `XmlLayout` or JSON-formatted `MapMessage` output for log integrity.

**Guava**
- No release this window. Latest confirmed is **33.7.1** (2026-08-18) — a fix release removing the Multi-Release JAR manifest entry that broke Java 9/10 under 33.7.0. No CVEs.

**JUnit**
- No release this window. Latest remains **JUnit 6.1.1** (2026-06-28). No CVEs. Migration notes stand: Java 17 / Kotlin 2.2 minimums, unified Platform/Jupiter/Vintage versioning, JSpecify nullability annotations, `suspend fun` test methods, deterministic `@Nested` test ordering.

**Maven**
- No GA release this window. Latest stable **3.9.16**; the Maven release-notes page was refreshed 2026-09-19 to list **planned** (not yet shipped) September releases for `maven-shared-utils` 3.5.0, `maven-script-interpreter` 1.9, `maven-shared-jar` 3.2.1, `doxia` 2.2.0, `reporting-api` 4.0.1, and `sitetools` 2.1.1 — confirm actual ship dates directly. `4.0.0-rc-6` remains the latest Maven 4 pre-release (still not GA).

**Gradle**
- No stable release this window. Latest stable remains **9.7.1** (2026-08-19/20); a **9.8.0-RC2** preview shipped 2026-09-17 (just before window), still pre-release. No CVEs. 9.7-line changes carried forward: Isolated Projects incubating; embedded Kotlin 2.4.0.

---

### Node.js

**Window checked:** 2026-09-18 to 2026-09-25. **Prettier was the only library in this stack with a confirmed in-window release** — Node.js core and the rest of the library set were quiet inside the window (most recent activity lands 09-08 through 09-17, just before it).

- **Node.js core**: No release inside the window. Latest remains **v26.8.2 (Current)** and **v24.21.0 "Krypton" (LTS)**, both from ~2026-09-08 (OpenSSL 3.5.8, NSS root-cert refresh). **Node.js is changing its release cadence starting with v27**: from v27 onward, every major becomes LTS (no more odd/even split), with a new six-month Alpha channel (`27.0.0-alpha.1`-style prerelease tags) — v27 itself has not shipped yet, expected around October 2026.
- **axios**: No release this window. Latest **v1.20.0** (2026-09-13, just before window) — prototype-pollution hardening, new `ContentTooLarge`/`UnprocessableContent` status names, interceptor/socket memory-leak fixes. No open CVE.
- **bcryptjs**: No release. Latest remains **v3.0.3** (2025-11-02). No CVEs.
- **express**: No release this window. 5.x latest **v5.2.1** (2025-12-01); 4.x latest **v4.22.2** (2026-05-11). Standing item: **CVE-2026-12590** (body-parser, Low) — fixed in body-parser 1.20.6 / 2.3.0.
- **moment**: Latest **v2.30.1** (2023-12-27). Maintenance-mode; keep migrating off it.
- **pg (node-postgres)**: Latest **v8.23.0** (~August 2026). No release this window, no CVEs.
- **sequelize**: v6 latest **v6.37.8** (2026-03-07); v7 still alpha. No release this window. Standing item: **CVE-2026-30951** (SQL injection via unescaped JSON/JSONB cast type, fixed 6.37.8).
- **socket.io**: No release this window. Server latest **v4.8.3** (2026-03-18); parser latest **v4.2.7 / v3.3.6** (2026-07-16). Standing `ws`-dependency hardening from earlier CVEs (**CVE-2026-45736**, **CVE-2026-48779**) remains applied at current versions.
- **jest**: No release this window. Latest remains **v30.5.2** (~mid-September). No CVEs.
- **prettier**: **Within the window — v3.9.9 released 2026-09-23**, fixing text containing `$` being incorrectly parsed as math syntax in Markdown. **v3.9.8** (2026-09-17, just before window) fixed Liquid objects interrupting Markdown paragraphs. No CVEs.
- **ReactJS**: No release this window. Latest **v19.3.0** (2026-09-09, just before window) — **View Transitions** (`<ViewTransition>` component, animate elements on enter/exit/move/resize via the browser View Transition API) and **Fragment Refs** both graduated from experimental to stable; `onBrowserBailout` option added to `react-dom/server`; Trusted Types API integration enabled. No new CVEs; the RSC DoS CVEs (CVE-2026-23864/-23870) remain relevant only to RSC/Server Actions users.
- **React Native**: No release this window. Latest **v0.87.1** (2026-08-26, just before window). Standing 0.87 breaking notes: Strict TypeScript API is the default public JS API; Node 22 / AGP 9 / Kotlin 2.0+ minimums; Metro 0.87.

---

### Database and Storage

- **Oracle Database**: No release inside the window. Oracle AI Database **26ai** (release-update line 23.26.x; latest RU 23.26.3, July 2026) is current. September **CSPU shipped 2026-09-15**; next quarterly **CPU is 2026-10-20**.
- **PostgreSQL**: **Within the window — PostgreSQL 19 Beta 4 released 2026-09-24.** **GA has slipped past its original September target and is now expected "by the end of October."** The RC is expected in early October. Standing item from two weeks ago: the **SQL/PGQ property-graph query feature (`GRAPH_TABLE`) was reverted from PG19** on 2026-09-07 — earliest return is PG20 (Sept 2027). Remaining PG19 headline features unchanged: `pg_plan_advice`, native `REPACK`/`REPACK CONCURRENTLY`, parallel autovacuum, expanded monitoring views, sequences in logical replication, `GROUP BY ALL`.
- **Redis**: No release inside the window. Latest remains **8.10.2** (2026-09-17, just before window). No new CVEs this window; the **CVE-2026-81934** TLS use-after-free (unauthenticated RCE) fix from the 2026-08-17 coordinated train remains the operative patch floor for TLS-enabled deployments below it.
- **Apache Kafka**: No release inside the window. Latest remains **4.3.x** (2026-06-25 area; sources also show a 4.2.1 bugfix line from 2026-05-30). No new CVEs.
- **MongoDB**: No release inside the window. **Out-of-band security patch just before window — CVE-2026-89099 (race condition in the document-value layer, unsynchronized concurrent memory access), fixed 2026-09-11 in 8.3.11 and 8.0.32.** Patch if below those floors. Also carried forward: **CVE-2026-82053/-82054/-82056/-82067/-82076** cluster (authorization escapes, case-sensitivity handling, CVSS up to 8.1) fixed across the 7.0.41 / 8.0.x / 8.3.x patch lines.
- **Apache Cassandra**: No release inside the window. Latest remains **5.0.9** (2026-08-07). Cassandra 6.0 remains pre-GA (alpha). No new CVEs.
- **Elasticsearch**: No release confirmed inside the window. Latest confirmed stable points are **9.5.3** (2026-09-03) and **9.4.6** (2026-09-01), both just before this window's start and already covered previously. A broader update mentioning ES|QL, ML, vector-search and snapshot-reliability improvements plus a bundled-JDK bump to 26.0.2 and Bouncy Castle FIPS 1.0.2.7/2.0.2 was referenced around 2026-09-23 in secondary sources but the exact version tag needs direct confirmation against elastic.co/blog before acting on it as "in-window."
- **BigQuery** (managed, rolling): **Within the window** — the Query results pane in BigQuery Studio now shows a short run history (including multi-statement queries) without navigating to Job History, reaching **GA**; generative-AI SQL functions now support the **`gemini-3.8-flash`** model. No CVEs, no breaking changes. Carried-forward deprecation: core Graph processing requires Enterprise/Enterprise Plus reservations from 2027-04-26.
- **Google Cloud Storage**: No major new item confirmed strictly inside this window beyond continued rollout of **Storage Insights activity data / `ObjectRef` BigQuery functions / partial-success bucket listing / object contexts** (reported previously); **Storage Intelligence advisor** (fleet-wide monitoring across orgs/folders/projects) is now called out as **GA**. No CVEs, no breaking changes.

---

### OS and Platform

- **Docker**: No release inside the window. Latest remains **Docker Engine 29.8.1** (2026-09-16, just before window), which included a fix for **CVE-2026-17106** (via a `moby/go-archive` bump) plus daemon-panic fixes for network-interface cleanup and swarm ingress-port removal. No new CVEs this window.
- **Kubernetes**: **Within the window — the 1.37.1 / 1.36.4 / 1.35.8 / 1.34.11 patch train shipped, with 1.37.1 confirmed 2026-09-23.** **v1.34 remains in maintenance mode** (critical-fixes-only) ahead of **EOL 2026-10-27** — 1.34.11 is likely near the end of its patch life. Carried-forward breaking items: containerd 2.0+ required since 1.35; static Pods can't reference Secrets/ConfigMaps.
- **GCP Compute Engine**: **Within the window — the Backup and DR API (`backupdr.googleapis.com`) now auto-enables (from 2026-09-21)** whenever Compute Engine, Cloud SQL Admin, AlloyDB for PostgreSQL, or Filestore APIs are enabled in a project — review before your next project setup if you don't want it on by default. **NVIDIA P100 GPUs reached end of support 2026-09-15** (just before window) — migrate workloads to G2 (L4) or G4 (RTX PRO 6000).
- **GKE**: No new security bulletin confirmed strictly inside this window. Carried-forward: **GCP-2026-058** (2026-09-02, Critical) — GKE Multi-Cloud attached-clusters cross-project authorization flaw (`CreateAttachedCluster`/`CreateAwsCluster`/`CreateAzureCluster`), auto-patched server-side, no customer action, no evidence of exploitation; etcd bumped to **v3.5.33-0-gke.3** addressing **CVE-2026-46595** and **CVE-2026-39821**.
- **GCP Maps API**: No confirmed in-window release. Recent (late Q3) items carried forward: `MarkerElement` for custom HTML content on 3D Maps, `FlattenerElement` for 3D-mesh flattening, `Map3DElement` `fov`/`cameraPosition` properties, and a more generous baseline 2D Map Tiles quota.
- **GCP Translate API**: **Within the window — release notes refreshed 2026-09-18.** New: DOC/PPT/XLS support for document translation, native-PDF page limit raised to **300 pages**, **multi-regional US endpoint now GA**, and adaptive-translation requests can now include up to five reference sentence pairs. Standing note: migrate off `www.googleapis.com` to `translation.googleapis.com`.
- **GCP Cloud Storage**: see Database and Storage section.
- **Google Analytics (GA4)**: No confirmed in-window release. Carried-forward: **Dashboards** (drag-and-drop KPI visualization) launched 2026-09-09; custom conversion-window support for click-through/engaged-view attribution; hostname allowlist data filters.
- **Amazon S3**: No confirmed in-window announcement. Standing recent items: 30-day minimum removed for Standard-IA/One Zone-IA transitions (July); S3 Files shared-filesystem access (April).
- **Amazon EC2**: No confirmed in-window announcement. Standing recent item: low-cost burstable **T8i** instances (6th-gen Intel Xeon "Granite Rapids," up to 30% better price-performance than T3) went GA in September, date unconfirmed against this window.
- **AWS Lambda**: No confirmed in-window announcement. Standing recent items (both just before window): **SnapStart for container-image functions** (2026-09-02) and **90-minute function timeout on Lambda Managed Instances** for async/ESM invocations (2026-09-09, a 6x increase from the prior 15-minute limit).
- **Amazon EKS**: **Within the window — EKS Distro `v1-37-eks-3` released 2026-09-22.** Standing: Kubernetes 1.33 standard support and 1.30 extended support both ended 2026-08-31 — clusters still on 1.33 need an extended-support license token for continued CVE patches.
- **Amazon CloudFront**: No confirmed in-window announcement. Standing recent items (before window): API support for flat-rate pricing plans (2026-09-03) and four new Dynamic Image Transformation features including enhanced smart cropping and an interactive transformation playground (2026-09-08).
- **Amazon Route 53**: No confirmed in-window announcement.
- **Cloudflare**: No major new item confirmed strictly inside this window. Carried-forward: **AI-crawler default classification (Search/Training/Agent) went live 2026-09-15**; a WAF release (2026-09-01) added detection for SQLi exploiting complex query syntax; Worker Previews (per-branch URLs/config/observability) and native Python Workers continue rolling out.
- **Firebase**: Rolling September updates (exact in-window dates unconfirmed): **CLI v15.30.2** (Cloud Functions parameter/secret prompt formatting, Firebase SQL Connect local toolkit bumped to v3.4.20, 2nd-gen Auth-trigger emulator fixes); **Apple SDK v12.19.2**; **Unity SDK v13.16.0** (Firebase AI/FCM/Remote Config updates); **Admin Python SDK v7.6.0** (App Check replay protection, Firebase ML deprecations ahead of its 2027-06-15 turndown). Carried-forward: Remote Config usage-based pricing (100,000 free daily fetches, effective 2026-09-01); Apple SDK stops publishing to CocoaPods after **October 2026**.
- **PayPal**: No confirmed in-window release. Standing item: Pay Later JS SDK v5→v6 migration guide updated 2026-09-09 (modular component loading, smaller payloads); Orders API v1→v2 migration guidance updated ~2026-09-22 — confirm exact scope directly if you're on the legacy Orders API.
- **Apple Pay**: No confirmed in-window release. **iOS 27 GA shipped 2026-09-14/15** (just before window) with a **redesigned Apple Pay checkout** — swipeable multi-card selection and a tap-to-view card grid on the main checkout screen. Validate PassKit / Apple Pay integrations against the new flow if you haven't already.

---

## Caveats

- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the window" is judged against **2026-09-18 to 2026-09-25**. Several items landed in the few days just before the window (Java 27 GA and CSPU on 09-15, Netty/Hibernate/Redis/React/axios/React Native/CloudFront/Lambda items on 09-01–09-17) and are reported as carry-forward where they remain the current operative guidance.
- Web-search sources disagreed on a few version numbers this week: MySQL Connector/J (9.7.0 vs. a renumbered 26.7.0 line), Protobuf (v35.0 vs. v34.1 "latest" tag), and the exact Elasticsearch version/date behind the 09-23 ES|QL/ML/vector-search update. These are flagged inline as needing direct confirmation against the upstream release page.
- **Open items to carry into next week:** PostgreSQL 19 GA (now targeted "by end of October," RC expected early October); Kubernetes 1.34 nearing its 2026-10-27 EOL; Oracle's next quarterly CPU (2026-10-20); Gradle 9.8.0 (currently RC2); Maven's planned September component releases (Doxia 2.2.0 etc. — not yet confirmed shipped); Node.js v27's cadence-change launch (expected ~October); Netty 4.1's 2027-07-01 EOL migration planning; Firebase Apple SDK CocoaPods cutoff (October 2026).
