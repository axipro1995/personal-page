# Weekly Tech Update Report — Week Ending September 3, 2026

**Scope:** Latest releases and security fixes across core backend languages/frameworks (Java, Node.js), databases/storage, and cloud/OS platforms, checked against the window **2026-08-27 to 2026-09-03**. Where a tool had no release inside that exact window, its most recent stable release is reported instead and flagged as such. Compiled via web search of official release notes, changelogs, GitHub release pages, and vendor advisories.

## TL;DR

- **Released this week:** Elasticsearch 9.5.3 (2026-09-03), Docker Desktop 4.89.0 (2026-08-31), Spring Modulith 2.2.0-M1 / 2.1.1 / 2.0.8 / 1.4.13 (~2026-09-01), GKE release 2026-R37 (2026-09-02), and a rolling batch of BigQuery / Cloud Storage / Firebase / AWS Lambda platform updates detailed below. Java, Node.js, and most of the Node/DB library stack had **no release inside this window**.
- **Patch now / act — security:**
  - **GKE — bulletin GCP-2026-058 (2026-09-02, Critical, no CVE):** a missing authorization check in the GKE Multi-Cloud APIs let an attacker register attached clusters into an arbitrary target project's Workload Identity Federation for GKE, enabling unauthorized Workload Identity token creation and impersonation of Kubernetes Service Accounts (attacker needed the target project number plus one namespace/KSA pair with `roles/iam.workloadIdentityUser`). **Google confirmed no exploitation and auto-patched all clusters server-side — no customer action required**, but review attached-cluster and Workload Identity Federation bindings.
  - **Cloud Service Mesh — bulletin GCP-2026-057:** Envoy/Istio flaws **CVE-2026-31837 / CVE-2026-31838** (High). Fixed in managed proxy `csm_mesh_proxy.20260819_RC00` and in-cluster CSM 1.29.7-asm.2 / 1.28.10-asm.24 / 1.27.9-asm.34. Applies if you run Cloud Service Mesh / Anthos Service Mesh; managed rollout is automatic, in-cluster installs must upgrade.
  - **Cloudflare WAF release 2026-09-01** added a new Managed Ruleset SQL-injection detection targeting complex query syntax (WHERE-comparison patterns). Verify no false positives against legitimate traffic.
  - **Carry-forward (still the operative guidance):** the 2026-08-20 ~91-CVE Spring-portfolio wave (Spring Security CVE-2026-59270 CVSS 9.4, Spring for GraphQL CVE-2026-59285 CVSS 9.2, etc.), the 2026-08-17 Redis coordinated release (CVE-2026-62356 and siblings), MongoDB's July 27-CVE disclosure, and the Jackson PolymorphicTypeValidator-bypass set — all landed before this window but remain the current floors to confirm.
- **Breaking-change watch:**
  - **GKE firewall-rule change (announced 2026-09-01):** GKE versions **1.35.1-gke.1031000 and later** change the automatically-created firewall rules for Services — rule priority shifts from **1000 → 999** and new **deny** rules are added. Custom firewall configurations that assumed the old priority or all-allow behavior may break. Audit firewall rules before upgrading the control plane / node pools past that version.
  - **BigQuery Graph edition restriction (announced 2026-08-27):** core graph processing will require **Enterprise / Enterprise Plus** reservations starting **2027-04-26**; Standard edition and on-demand billing support is being deprecated. Ironically BigQuery Graph also hit **GA on 2026-08-31** — GA and the edition lock-in landed the same week. Plan a reservation/edition move if you use `GRAPH_TABLE` on Standard/on-demand.
  - **Firebase Remote Config usage-based pricing (effective 2026-09-01):** free tier of **100,000 daily fetch requests** per project, then pay-as-you-go on the Blaze plan. Review client `minimumFetchInterval` / fetch cadence in mobile apps to avoid a surprise bill.
  - **Amazon EKS:** Kubernetes **1.33 end of standard support** (and **1.30 end of extended support**) hit **2026-08-31**, at this window's edge. Clusters on 1.33 now need an extended-support license token to keep receiving CVE patches. Finalize upgrades.
  - **Kubernetes 1.34** has been in **maintenance mode since 2026-08-27** (critical-fixes-only, EOL 2026-10-27). No 1.37.x patch this window — 1.37.1 / 1.36.4 / 1.35.8 / 1.34.11 are targeted for **2026-09-15**.
  - Carried forward from the 4.0 baseline: Spring Boot 4 (Jackson 3 default, Spring Security 7 CSRF changes, Undertow dropped); React Native 0.87 (Strict TypeScript API default, Node 22 / AGP 9 / Kotlin 2.0 minimums); Gradle 9.7 (Isolated Projects incubating, embedded Kotlin 2.4).

---

### Java

**Window checked:** 2026-08-27 to 2026-09-03. No Java-ecosystem tool shipped a release inside this exact window; the Spring Modulith milestone wave (~09-01) is the only movement, and it carries no CVEs.

**Java (core / JDK)**
- No release inside the window. Oracle's monthly CSPU is due **2026-09-15** (next quarterly CPU 2026-10-20). The last patch train was the **2026-08-18 CSPU** (19 Java SE patches, 17 remotely exploitable without auth, highest Java-SE CVSS 7.5) — already covered two reports ago. No new Java-core security news 08-27 → 09-03.

**Netty**
- No release this window. Latest remains **4.1.137.Final / 4.2.17.Final** (early August). Standing fix set unchanged: CVE-2026-62243 (TLS hostname-verification bypass on the OpenSSL client path), CVE-2026-59903 (Vary-header CORS cache poisoning), CVE-2026-62380 (SOCKS4/5 encoder injection). No breaking changes.

**Vert.x**
- No release this window. Latest remains **5.1.6 / 4.5.32** (2026-08-07/08). No new CVEs.

**Spring (Framework / Boot / Cloud / AI / Modulith)**
- **Within the window:** **Spring Modulith 2.2.0-M1, 2.1.1, 2.0.8, and 1.4.13** (~2026-09-01) — routine milestone/patch releases, **no CVEs, no breaking changes**.
- "This Week in Spring" (2026-09-01) reported **no new security advisories** since the 2026-08-20 ~91-CVE portfolio wave. Spring Boot remains at **4.1.1 / 4.0.8** (2026-08-20/21) and **4.2.0-M1** (2026-08-20); Spring Cloud at **2025.1.3 "Oakwood"**; Spring AI at **2.0.1**.
- **Action still open:** if you have not yet absorbed the 08-20 wave, the critical named CVEs are **CVE-2026-59270** (Spring Security embedded UnboundID LDAP admin-DN exposure, CVSS 9.4) and **CVE-2026-59285** (Spring for GraphQL unsafe deserialization → possible RCE, CVSS 9.2), plus a dozen-plus High-severity CVEs across Spring Security, Spring Data REST, Spring Integration, Reactor, Spring AMQP, Spring Batch, and Spring AI.

**Hibernate**
- No release this window. Latest remains **7.4.6.Final / 6.6.56.Final** (2026-08-23). 8.0 still in Beta. No CVEs.

**JDBC drivers**
- **pgjdbc**: latest **42.7.13** (2026-07-06). No new release/CVE. Standing history: CVE-2026-54291 (SCRAM channel-binding downgrade, fixed 42.7.12), CVE-2026-42198 (SCRAM-response DoS, fixed 42.7.11).
- **MySQL Connector/J**: latest **9.7.0** (2026-04-22). No new release/CVE.
- Note: BigQuery shipped an updated **Simba JDBC driver for BigQuery** on 2026-09-02 (see Database section).

**Protobuf**
- No release inside the window. Latest stable is the **v36.0 GA line** from 2026-08-20 (one source instead shows the 35.x line as current — treat the exact latest as needing direct confirmation against protocolbuffers/protobuf releases). No CVEs. Carried-forward breaking notes: `sun.misc.Unsafe` removal from Java codegen, Bazel 7 support dropped for Bazel 9+.

**Dagger**
- No release this window. Latest remains **2.60.1** (2026-07-06). No CVEs or breaking changes.

**Jackson**
- No release this window. Latest lines: **2.20.0** (2.x), **3.2.0** (3.x, 2026-06-08). Standing critical set still the top outstanding Java CVE risk for laggards: **CVE-2026-54512** (CVSS 8.1, PolymorphicTypeValidator bypass), **CVE-2026-54513/-54514** (sibling PTV/DNS bypasses), **CVE-2026-54515** (case-insensitive deserialization reopens `@JsonIgnoreProperties`-protected fields), **CVE-2026-59889** (`@JsonView` bypass via `@JsonUnwrapped`).

**Log4j**
- No release this window. Latest remains **2.26.1** (2026-07-02). No new CVEs.

**Guava**
- No release this window. Latest stable **33.6.0** (2026-04-14). No CVEs. (The 33.7.x versions cited in an earlier report remain unconfirmed against google/guava's release page.)

**JUnit**
- No release this window. Latest remains **JUnit 6.1.3** (2026-08-07). No CVEs. JUnit 5→6 migration notes stand: Java 17 minimum, Kotlin 2.2+, removed modules/APIs, FastCSV engine.

**Maven**
- No GA release this window. Latest stable **3.9.16** (2026-05-13); pre-release **4.0.0-rc-6** (2026-08-04, still not GA, requires Java 17). No CVEs.

**Gradle**
- No release this window. Latest remains **9.7.1** (2026-08-19/20). No CVEs. 9.7-line changes carried forward: Isolated Projects incubating, embedded Kotlin 2.4.0 (drops Kotlin 1.9 language-version support), relocated `org.gradle.internal.impldep.*` no longer importable from the Kotlin DSL.

---

### Node.js

**Window checked:** 2026-08-27 to 2026-09-03. **No Node.js-ecosystem release inside this window** — the entire stack was quiet. Node.js core, axios, and React Native all shipped in the *previous* window (2026-08-26) and are unchanged.

- **Node.js core**: No release this window. Current **v26.8.0** and LTS **v24.20.0 "Krypton"** both from **2026-08-26** (feature-only, not security). No v26.9.x / v24.21.x yet. Node.js 26 enters LTS in **October 2026**. Security baseline remains the **2026-07-29** release (Permission Model bypass, HTTPS Agent mTLS/session-reuse, DNS/SQLite/zlib) — confirm applied.
- **axios**: No release this window. Latest **v1.20.0** (2026-08-24/26) — prototype-pollution hardening, `ContentTooLarge`/`UnprocessableContent` status names added (old aliases retained), interceptor/socket memory-leak fixes. No open CVE.
- **bcryptjs**: Latest **v3.0.3** (2025-11-02). No release, no CVEs.
- **express**: 5.x latest **v5.2.1** (2025-12-01); 4.x latest **v4.22.2** (2026-05-11). No release this window. Standing item: **CVE-2026-12590** (body-parser, Low) — invalid `limit` silently disabled body-size enforcement; fixed in body-parser 1.20.6 / 2.3.0. Confirm your floor.
- **moment**: Latest **v2.30.1** (2023-12-27). Maintenance-mode; keep migrating off it.
- **pg (node-postgres)**: Latest **v8.23.0** (2026-08-08). No release this window, no CVEs.
- **sequelize**: v6 latest **v6.37.8** (2026-03-07); v7 still alpha. No release this window. Standing item: **CVE-2026-30951** (SQL injection via unescaped JSON/JSONB cast type, fixed 6.37.8, v7 unaffected).
- **socket.io**: Server latest **v4.8.3** (2026-03-18); parser latest **v4.2.7 / v3.3.6** (2026-07-16). No release this window. Standing item: **CVE-2026-69185** (CVSS 7.5, zero-attachment memory-exhaustion DoS) — confirm `socket.io-parser` ≥ 4.2.7.
- **jest**: Latest **v30.4.2** (2026-05-09). No release this window, no CVEs.
- **prettier**: Latest **v3.9.x** (3.9 line, from 2026-06-27; latest patch ~3.9.6 in July). No release this window, no CVEs.
- **ReactJS**: Latest **v19.2.8** (2026-07-21). No release this window, no new CVEs. RSC DoS CVEs (CVE-2026-23864/-23870 in `react-server-dom-*`) remain relevant only for RSC/Server Actions users.
- **React Native**: No release this window. Latest **v0.87.1** (2026-08-26). 0.87 breaking notes carried forward: Strict TypeScript API is now the default public JS API; Node 22 / Android Gradle Plugin 9 / Kotlin 2.0+ minimums; Metro 0.87.

---

### Database and Storage

- **Oracle Database**: No release inside the window. Next monthly CSPU **2026-09-15**; next quarterly CPU **2026-10-20**. The 2026-08-18 CSPU remains the operative patch train.
- **PostgreSQL**: No release inside the window. Latest stable remains **18.6 / 17.11 / 16.15 / 15.19 / 14.24** (2026-08-13). **PostgreSQL 19 is still in Beta 3** (2026-08-13) — GA is expected imminently (Sept/Oct cadence) but had **not** landed by 2026-09-03. Watch next window for the 19.0 GA (headline features: `REPACK`/`REPACK CONCURRENTLY`, SQL/PGQ property-graph queries, parallel autovacuum, vacuum prioritization scoring).
- **Redis**: No release inside the window. Latest remains the **2026-08-17 coordinated train** (8.10.1 / 8.8.2 / 8.6.6 / 8.4.6 / 8.2.9 / 7.4.11 / 7.2.16 / 6.2.24), which fixed CVE-2026-62356 (CMSketch heap OOB write) plus use-after-free / RESTORE / ACL-bypass / TLS CVEs. Confirm you are on those floors.
- **Apache Kafka**: No release inside the window. Latest remains **4.3.1** (2026-06-25). No new CVEs.
- **MongoDB**: No release inside the window. The July 27-vulnerability disclosure (led by query-level RBAC bypass CVE-2026-13059, CVSS 8.6, and Compass command-injection CVE-2026-14881, CVSS 8.4) remains the operative advisory. No follow-up bulletin 08-27 → 09-03.
- **Apache Cassandra**: No release inside the window. Latest remains **5.0.9** (2026-08-07). No new CVEs.
- **Elasticsearch**: **Within the window — 9.5.3 (2026-09-03).** Routine bugfix release: **no CVEs, no breaking changes.** Notable items: audit events now include `request.raw_body` for protobuf request bodies, and oversized request bodies are rejected when request-body auditing is enabled; faster vector-quantization calibration during index merges; fixes across analysis (min_hash overflow), field-level security filtering, ES|QL (`BYTE_LENGTH`, `MV_MIN`/`MV_PERCENTILE`), and vector-search endianness. Carried-forward 9.5 behavior change: `search.batched_query_phase` on by default. 8.x line latest remains **8.19.20** (2026-08-11).
- **Google BigQuery** (managed, rolling):
  - **2026-08-27**: deprecation announcement — core Graph processing will require Enterprise / Enterprise Plus reservations from **2027-04-26** (Standard edition + on-demand billing support removed); Data Engineering Agent reaches HIPAA compliance; XGBoost 2.1 training GA.
  - **2026-08-31**: **BigQuery Graph GA**; folder-based pipeline management GA; identity / auto-incrementing columns for primary-key management (Preview); TabFM tabular foundation model supports zero-shot regression/classification via `AI.PREDICT` / `AI.EVALUATE` (Preview); daily-token-quota config for generative-AI functions restored.
  - **2026-09-01**: Rust SDK for BigQuery (Preview).
  - **2026-09-02**: updated **Simba JDBC driver for BigQuery** available.
  - **2026-09-03**: Conversational analytics supports market-basket-analysis questions (GA).

---

### OS and Platform

- **Docker**: **Within the window — Docker Desktop 4.89.0 (2026-08-31).** **No CVEs, no breaking changes.** Bundles **Compose v5.5.0**, **NVIDIA Container Toolkit v1.20.0**, Docker Agent v1.127.0. Fixes: background container polling that woke the engine unnecessarily under Resource Saver mode; start failure after an ungraceful shutdown left a stuck socket file; a dashboard crash. Behavior note: with `CONTAINERD_BUILDX_FOR_WINDOWS` enabled, Windows Containers now use dockerd's embedded containerd and in-process BuildKit instead of standalone services. Carried-forward breaking notes: `docker sbom` deprecated for `docker scout sbom`; QEMU backend removed on macOS.
- **Kubernetes**: No release inside the window. **v1.37.0 "Garhwal"** (2026-08-26) remains newest; **1.34 in maintenance mode since 2026-08-27** (EOL 2026-10-27). Next patch train (1.37.1 / 1.36.4 / 1.35.8 / 1.34.11) targeted **2026-09-15**. Carried-forward breaking items: containerd 2.0+ required since 1.35; static Pods can't reference Secrets/ConfigMaps.
- **GCP Compute Engine**: No release inside the window (notes jump from 2026-08-26 to 2026-09 gap). Standing item: bulletin **GCP-2026-054 / CVE-2026-6726** (TPM 2.0 reference-code OOB read on AMD-based Confidential/Shielded VMs).
- **GKE**: **Within the window.**
  - **2026-09-02 (release 2026-R37):** default versions — Rapid **1.36.3-gke.1767000**, Regular/Extended **1.35.7-gke.1150000**, Stable **1.34.9-gke.1655001**; multiple versions on 90-day deprecation clocks; COS images refreshed with cumulative security fixes.
  - **2026-09-02: bulletin GCP-2026-058 (Critical)** — GKE Multi-Cloud APIs missing-authorization check (attached-cluster registration → Workload Identity token creation / KSA impersonation). Auto-patched server-side, no customer action, no exploitation observed.
  - **2026-09-01 (breaking):** GKE **1.35.1-gke.1031000+** changes auto-created firewall rules for Services — priority 1000 → 999 and new deny rules; audit custom firewall configs before upgrading.
  - **2026-09-03:** Confidential GKE Nodes with Intel TDX on `c3-standard-*-lssd` GA. **2026-09-01:** C3 bare-metal (`c3-standard-192-metal`) GA on Standard clusters. **2026-08-31:** session affinity via `GCPTrafficDistributionPolicy` for GKE Gateway GA, incl. `STRONG_COOKIE_AFFINITY` (needs 1.36.3-gke.1767000+). **2026-08-27:** NEG pre-provisioning (Preview).
- **GCP Cloud Service Mesh**: **Within the window — bulletin GCP-2026-057** (Envoy/Istio **CVE-2026-31837 / CVE-2026-31838**, High). Managed proxy fixed in `csm_mesh_proxy.20260819_RC00`; in-cluster fixed in 1.29.7-asm.2 / 1.28.10-asm.24 / 1.27.9-asm.34.
- **GCP Maps API**: No release inside the window. Latest **v3.66.1a** (2026-08-17).
- **GCP Translate API**: No release inside the window. Standing note: migrate off `www.googleapis.com` to `translation.googleapis.com`.
- **GCP Cloud Storage**: **Within the window — 2026-09-03:** Storage Batch Operations now support **CEL metadata filters** sourced from Storage Insights datasets (no CSV manifest needed), targeting up to **1,000 buckets per job**, bulk storage-class transitions, bulk ACL patching, and multi-bucket dry-run validation. No CVEs, no breaking changes. (The HNS lifecycle empty-folder-deletion change flagged in prior reports occurred 2026-08-26 and is now closed.)
- **Google Analytics (GA4)**: No release inside the window.
- **Amazon S3**: No confirmed in-window announcement. AWS Lambda gained configurable control over S3 direct-access reads (see Lambda).
- **Amazon EC2**: No confirmed in-window announcement (routine regional expansions only).
- **AWS Lambda**: **Within the window (around the 2026-08-31 AWS Weekly Roundup):** public-preview runtimes introduced, starting with **Node.js 26** and **Python 3.15** (auto-graduate at GA); **configurable control over S3 direct access** (explicitly enable/disable functions streaming file reads directly from S3); new **`DependencyError`** value in `StateReasonCode` / `LastUpdateStatusReasonCode` for clearer failed-state diagnostics. No CVEs.
- **Amazon EKS**: **Within the window (edge):** Kubernetes **1.33 end of standard support** and **1.30 end of extended support** both **2026-08-31**. Clusters past standard support need an extended-support license token to keep receiving CVE patches. The 2018-vintage-cluster **CA rotation** action item (replace worker nodes + update external clients to trust the successor CA) also remains open.
- **Amazon CloudFront**: No confirmed in-window announcement.
- **Amazon Route 53**: No announcement found for this window.
- **Cloudflare**: **Within the window — WAF release 2026-09-01:** new Managed Ruleset SQL-injection detection for complex query syntax (WHERE-comparison patterns). Verify against legitimate traffic before broad enforcement.
- **Firebase**: **Within the window.** **Remote Config** moves to **usage-based pricing effective 2026-09-01** — free tier 100,000 daily fetch requests/project, then pay-as-you-go on Blaze (review client fetch cadence). SDK wave across **Apple / Android / Admin Node.js / JavaScript**: Firebase AI Logic adds `RealtimeInputConfig` in `LiveGenerationConfig` and `sendStartActivityRealtime` / `sendStopActivityRealtime` to `LiveSession`; App Check, Cloud Firestore, Cloud Messaging, Data Connect / SQL Connect, Crashlytics Gradle plugin, and the Android BoM updated. Imagen models shut down 2026-08-17 (migrate to Gemini "Nano Banana"); Firebase Apple SDK stops publishing to CocoaPods after October 2026.
- **PayPal**: No confirmed in-window release.
- **Apple Pay**: No confirmed in-window release. iOS 27 GA still expected September 2026 — validate PassKit / Apple Pay integrations against the developer beta ahead of GA.

---

## Caveats

- Compiled from public vendor release notes, changelogs, GitHub release pages, and security advisories via web search rather than direct verification of every CVE — confirm exact CVSS scores, affected versions, and patch availability against vendor advisories before acting.
- "Within the window" is judged against **2026-08-27 to 2026-09-03**. Items that landed just before the window (Spring's 08-20 wave, Redis's 08-17 train, Node.js 08-26, Kubernetes 1.37.0 on 08-26) are reported as carry-forward because they remain the current operative guidance.
- Web-search sources disagreed on a few version numbers this week (Protobuf 35.x vs 36.0; Netty patch level; Guava 33.6.0 vs 33.7.x). These are flagged inline as needing direct confirmation against the upstream release page.
- **Open items to carry into next week:** PostgreSQL 19.0 GA (imminent, not yet shipped as of 09-03); Kubernetes 09-15 patch train (1.37.1 etc.); Oracle CSPU 09-15; BigQuery Graph edition restriction (2027-04-26); EKS CA rotation for long-lived clusters; Firebase Apple SDK CocoaPods cutoff (October 2026).
