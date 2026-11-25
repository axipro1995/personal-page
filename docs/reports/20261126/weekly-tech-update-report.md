# Weekly Technology Updates — Week Ending 2026-11-26

Window: 2026-11-19 → 2026-11-26. Compiled from web search; many results were secondary sources or cached pages. Verify against official release notes before acting. "No confirmed release" means nothing dated in the window was found, not that nothing shipped. Items dated before the window are marked "(prior)" and are kept only where still relevant.

## Highlights

- **PostgreSQL 19:** one release-notes page showed a banner "November 19, 2026: PostgreSQL 19 Released!". This is from a single source, so confirm it on postgresql.org. The Nov 12 minors (18.7, 17.12, 16.16, 15.20, 14.25) fixed 52 CVEs (prior). **PostgreSQL 14 is EOL** after 14.25.
- **Java "Java4Shell" (prior, Nov 17):** Microsoft's advisory for CVE-2026-114938 (memory corruption, possible unauthenticated RCE) lists fixed builds 25.0.5.1, 21.0.13.1 and 17.0.21.1. Oracle published 25.0.5.1 and 21.0.13.1 notes. Patch now if not done. This came from one search summary, so check the vendor advisory.
- **Gradle 8.14.8 (Nov 19):** includes security fixes. The 9.x line is at 9.9.1 (Nov 10, 3 security fixes), and 9.9.2 notes also cite 3 security vulnerabilities, with no date found.
- **Node.js (prior, Nov 11):** security releases 26.13.1, 24.22.3 and 22.23.5 fix about 16 flaws (7 high), mostly HTTP/2 and TLS. The count comes from secondary blogs. Node 20 and older are EOL and unpatched.
- **Oracle CSPU (prior, Nov 17):** 7 new Database patches, highest CVSS 9.9. Affects 19.3–19.33, 21.3–21.24, 23.4–23.26.4. Based on the pre-release announcement, not the final advisory.
- **Kubernetes:** ingress-nginx CVE-2026-116472 will not be fixed because the project was retired in March. Migrate or mitigate. A Kyverno flaw (CVSS 9.9, cross-tenant RoleBinding rewrite) was reported in early November.
- **AWS re:Invent** runs Nov 30 – Dec 4, so expect a large batch of announcements next week.

## Java

| Tool | Latest found | Notes |
|---|---|---|
| Java core | JDK 27.0.1.1, 25.0.5.1, 21.0.13.1, 11.0.33.1 (Oracle, Nov 17 CPU) | JDK 27 is out (Java SE 27, Sep 2026). Oracle says 25.0.5.0.2 should not be used after the CPU. Microsoft lists 17.0.21.1 as fixed. |
| Netty | 4.2.18.Final, 4.1.138.Final (Sep 9) | Many security fixes (HTTP, HTTP/3, STOMP, OCSP). **Netty 4.1 EOL: 2027-07-01.** No Nov release found. |
| Vert.x | 5.1.7, 4.5.34 | 4.5.34 ships Netty 4.1.138 for vulnerability fixes. 4.x is supported until Apr 2027. No Nov release found. |
| Spring | Boot 4.2.0-RC1, Framework 7.1.0-RC1 (Oct) | GA not confirmed. Framework 7.1 is expected to deprecate `RestTemplate` in favour of `RestClient`. |
| Hibernate ORM | 7.4.13.Final (Oct 11), 6.6.59 (Oct 18), 8.0.0.Beta3 (Sep 25) | No Nov ORM release found. Hibernate NoSQL 1.0.0.Alpha4 (Nov 3) is a separate project. |
| JDBC (pgjdbc) | 42.7.12 (security, Jun/Jul) | Upgrade for the GSS-encryption issue CVE-2026-107313 and the SCRAM channel-binding policy. A new from-scratch driver is in pre-release. No Nov release found. |
| Protobuf | 36.0 line (Editions 2026 min protoc 36.0, Aug 20) | No Nov release found. |
| Dagger | 2.60.1 (date unconfirmed); Hilt 1.4.0 (Jul 1) | No Nov release found. |
| Jackson | 3.2.3, 3.1.7 (Sep 22) | No Nov release found. The HeroDevs 2.13.13 fork (Oct 23) fixes CVE-2026-59889, which is not an upstream release. |
| Log4j | 2.26.1 (Jun 29), 2.25.5 (Jul 1) | Avoid 2.26.0 (CVE-2026-49844). Apr 2026 CVE-2026-34477..34481 are fixed from 2.25.4. No Nov release found. |
| Guava | 33.7.0 (Aug 17, single source) | No Nov release found. |
| JUnit | 6.x (6.1.x seen) | No Nov release found. Requires Java 17. |
| Maven | 3.9.16 (May 13), 3.10.0-rc-1 (Jul 9), 4.0.0-rc-6 | No Nov core release found. |
| Gradle | 8.14.8 (Nov 19), 9.9.1 (Nov 10), 9.9.0 (Nov 6) | Security fixes in 8.14.8 and 9.9.1. 9.9.0 adds incremental compilation for Kotlin DSL scripts. |

## Node.js

| Tool | Latest found | Notes |
|---|---|---|
| Node core | 26.13.1, 24.22.3, 22.23.5 (Nov 11) | Security release, see Highlights. Updates llhttp 9.4.4, undici, sqlite 3.54.1. |
| axios | 1.21.1 (Nov 9), 0.35.0 (Nov 2) | From the Versio table. Publishing now requires a maintainer approval step (staged publishing). |
| bcryptjs | 3.0.2 | No new release found. |
| express | 5.3.0 (Oct 9) | Express 4 EOL is targeted no sooner than Oct 1, 2026. Plan the move to 5.x. |
| moment | 2.31.0 (Sep 15) | Fixes CVE-2026-17495 (affects 2.29.2–2.30.1). The project is in maintenance mode. |
| pg | 8.23.1 | Release date not confirmed. |
| sequelize | 6.37.8 (Mar 7), v7 alpha.48 | v7 is still pre-release, with the Postgres dialect in `@sequelize/postgres`. |
| socket.io | 4.8.3 (Dec 2025) | The tracker may be stale. |
| jest | 30.x (30.2.0 seen) | No release date found. |
| prettier | 3.8.0 seen | No release date found. |
| ReactJS | No release found | |
| React Native | 0.89.0-rc.1 (Nov 9) | Stable 0.89 is due Dec 7. 0.88 (Oct) is current, with 0.88.1 on Oct 27. |

## Database and Storage

| Tool | Latest found | Notes |
|---|---|---|
| Oracle | Nov 2026 CSPU (Nov 17) | See Highlights. |
| PostgreSQL | 18.7, 17.12, 16.16, 15.20, 14.25 (Nov 12); 19 reported Nov 19 | Nov 12 fixes include tzdata 2026f and a GIN index issue (reindex suspect indexes). 19 is unconfirmed. |
| Redis | Open Source 8.12 (RC, Oct); Software 8.2.6-25 (Oct) | No Nov release found. Redis Software 7.4 EOL is in Nov 2026. |
| Kafka | 4.4.0 (Oct 22), 4.3.2 (Oct 23) | No Nov release. 4.5.0 is targeted for around Mar 2027. |
| MongoDB | 9.0.2 (Sep 28), 8.3.13 (Oct 13) | No Nov server release found. |
| Cassandra | 5.0.9, 4.1.12, 4.0.21 (Aug 7); 6.0 alpha | No Nov release found. |
| Elasticsearch | 9.5.6 (Nov 3) | Contains security fixes (see Elastic's advisory). Bundled JDK is 27.0.1.0.2. |
| BigQuery | Lakehouse caching for cross-cloud data GA (Nov 2) | Also gemini-4-argon in generative AI functions. Nothing later found. |

## OS and Platform

| Tool | Latest found | Notes |
|---|---|---|
| Docker | Desktop 4.99.0 (Nov 9), 4.98.0 (Nov 3) | 4.98.0 fixes CVE-2026-98432 and a WSL 2 AF_VSOCK bypass (CVE-2026-112917, needs WSL 3.0.3+). Engine 29.9.2 (Oct). Mostly from the Releasebot aggregator. |
| Kubernetes | 1.34.12, 1.35.9, 1.36.5, 1.37.1 (Sep patches) | 1.38 is in its enhancements freeze. See Highlights for ingress-nginx. No Nov patch found. |
| GC Compute Engine / GKE | GKE 2026-R45 (Oct 26) | Versions 1.34–1.37 available. Running without a release channel is deprecated and ends 2027-06-14. No Nov GKE entry found. |
| GC Maps API | No information found | |
| GC Translate API | Last notes Feb 24, 2026 | Translation LLM supports LoRA full finetuning. |
| GC Storage | No information found | |
| Google Analytics | Data Manager (beta), Nov 6 | Single source. Official page snippets showed no Nov entries. |
| Amazon S3 | S3 Tables managed Hive to Iceberg migration | Date unconfirmed. |
| Amazon EC2 | No information found | |
| Amazon Lambda | GPU (G-series) for Event Functions on Managed Instances | Date unconfirmed. Lambda Web Functions came in Oct. |
| Amazon EKS | Kubernetes 1.36 support, customer-routed control plane egress (Jun) | No Nov entry found. |
| Amazon CloudFront | Flat-rate plans gained Lambda@Edge, CAPTCHA, mTLS | Date unconfirmed. |
| Amazon Route 53 | Accelerated recovery for public DNS (60-min RTO, us-east-1); Global Resolver preview | Date unconfirmed. |
| Cloudflare | No Nov entry found | Service Key API auth stopped working on Sep 30, 2026. Check for leftover use. |
| Firebase | CLI 15.35.0 (Nov 4); Admin SDK fixes (Nov 11) | Gemini 2.5 Pro/Flash via the Gemini Developer API are affected from **Dec 1, 2026**. Releasebot shows more entries on Nov 12–18, with no content seen. |
| Paypal | No Nov entry found | |
| ApplePay | No developer change found | Apple named a new head of Pay and Wallet (reported Nov 18). A US connectivity issue was reported with no details. |

## Suggested actions

1. Patch JDK to the Nov 17 builds (25.0.5.1 / 21.0.13.1 / 17.0.21.1).
2. Roll out the PostgreSQL Nov 12 minors. Plan to move off 14.
3. Upgrade Gradle to 9.9.1 or later (8.14.8 on the 8.x line) and Node to the Nov 11 releases.
4. Review Netty 4.1 (EOL Jul 2027), Express 4 and ingress-nginx for migration.
5. Check Firebase Gemini 2.5 usage before Dec 1.
