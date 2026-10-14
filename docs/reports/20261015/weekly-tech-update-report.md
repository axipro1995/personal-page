# Weekly Technology Updates — Week Ending 2026-10-15

Scope: releases, breaking changes, and security fixes for the core tools/libraries in the watch list, for the week 2026-10-08 → 2026-10-15. Compiled from web searches; the search index often lagged official release pages, so several items are marked **no data found**. Treat those as "unverified", not "no release".

## Highlights

- **Oracle CPU and OpenJDK quarterly CPU both land on 2026-10-20**: plan patch windows for Java and Oracle Database.
- **React Native 0.88** was released on 2026-10-12.
- **Protobuf v36.0-rc1** (Edition 2026) is out; the final release is expected soon. Edition 2026 changes some defaults (naming style, strict symbol visibility), so test your builds.
- **Node.js 27** is due this month. Every release goes LTS, versions align to the calendar year, and the number of concurrent release lines shrinks. Node 18 and 20 are EOL.
- **Kafka 4.2.2** was announced 2026-09-29.
- **Elasticsearch**: vulnerabilities were flagged on 2026-10-06 for 8.19.23, 9.4.8 and 9.5.5. Check the Elastic security advisory and upgrade.
- **PostgreSQL 14 reaches EOL on 2026-11-12**, with its final minor release that day.

## Java

| Tool | Latest found | Notes |
|---|---|---|
| Java (OpenJDK) | July 2026 CPU/PSU current | Next quarterly CPU on **2026-10-20**. |
| Netty | 4.1.131.Final / 4.2.10.Final (Feb 2026) | No newer release found. |
| Vert.x | 5.1.6 (2026-08-05), 4.5.32 (2026-08-07) | Nothing newer found. |
| Spring | Boot 4.2.0-M2 and Framework 7.1.0-M2 (week of 2026-09-21) | Milestones only. Check the spring.io release pages for GA patches. |
| Hibernate ORM | 7.4.7.Final | 7.3.x reached EOL (7.3.13, 2026-08-02). |
| JDBC (pgjdbc) | 42.7.10 (2026-02-15) | A later release is reported on 2026-07-07, but no version was found. |
| Protobuf | v36.0-rc1 | Edition 2026: `enforce_naming_style` defaults to STYLE2026 and `default_symbol_visibility` to STRICT. New JSON custom string option for enum values. |
| Dagger / Jackson / Log4j / Guava | no data found | |
| JUnit | 6.1.3 on Maven Central | Requires Java 17. The 5.13.x line is the last supporting Java 8. |
| Maven / Gradle | no data found | |

## Node.js

| Tool | Latest found | Notes |
|---|---|---|
| Node.js | July 2026 security release (2026-07-29) fixed 11 CVEs on 22.x/24.x/26.x | Two HTTP/2 highs include a heap use-after-free (CVE-2026-56848). One `--permission` sandbox escape (CVE-2026-58043). Node 27 is due in October. |
| axios | 1.20.0 (2026-08-26), 0.34.0 (2026-09-16) | Move off the 0.x line where possible. |
| express | last release 2026-05-11 | |
| bcryptjs | 3.0.3 | 3.x generates `2b` hashes by default. |
| moment, pg, sequelize, socket.io, jest, prettier | no data found | |
| React | 19.2 line | No new release found. |
| React Native | **0.88** (2026-10-12) | New Architecture only, Hermes V1 default. Review the upgrade helper for breaking changes. |

## Database and Storage

| Tool | Latest found | Notes |
|---|---|---|
| Oracle | July 2026 CPU: Database 19.3–19.31 and 23.4.0–23.26.2 affected | Next CPU **2026-10-20**. |
| PostgreSQL | 18.3/17.9/16.13/15.17/14.22 (Feb 2026) | PG 19 expected around now. PG 14 EOL 2026-11-12. |
| Redis | 8.6 GA on Redis Cloud Pro (July 2026) | Redis 8.0 EOL 2026-12-01. |
| Kafka | 4.2.2 (2026-09-29), 4.3.1 (2026-06-25) | |
| MongoDB, Cassandra | no data found | |
| Elasticsearch | 9.5.1 (2026-08-11) | See highlights for the 2026-10-06 vulnerability notice. |
| BigQuery | Sept 2026: Data Engineering Agent with BigQuery Graph (GA), `run_bq_command` MCP tool (Preview) | |

## OS and Platform

| Tool | Latest found | Notes |
|---|---|---|
| Docker Engine | 29.6.2 (2026-07-16) | containerd 2.2.6 and security fixes. 29.x uses the containerd image store by default for new installs. |
| Kubernetes | 1.36 "Haru" (2026-04-22) | User Namespaces and Mutating Admission Policies are stable. |
| Compute Engine / GKE | Sept 2026: c3-standard-*-lssd as Confidential GKE Nodes (Intel TDX) GA | |
| GC Maps / Translate / Storage / Analytics | no data found | |
| Amazon S3 / EC2 / Lambda / EKS / CloudFront / Route 53 | S3 access logs to CloudWatch (Jun), EKS Auto Mode zonal shift (Jul), Route 53 accelerated recovery (60-min RTO) | Nothing dated this week found. |
| Cloudflare | Billing profile adds Apple Pay, Google Pay, Link (May 2026) | |
| Firebase / PayPal / Apple Pay | no data found | |

## Suggested actions

1. Schedule patching for the 2026-10-20 Oracle and OpenJDK CPUs.
2. Check Elasticsearch deployments against the 2026-10-06 advisory.
3. Plan the Node 27 / React Native 0.88 evaluation and the PostgreSQL 14 upgrade before 2026-11-12.
4. For the "no data found" rows, check the official release pages directly.
