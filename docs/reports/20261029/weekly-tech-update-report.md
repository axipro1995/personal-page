# Weekly Tech Update Report — Week Ending 2026-10-29

Window: 2026-10-22 → 2026-10-29. Compiled from web searches. Items marked "no confirmed release" had no release surfaced in the window by search; it does not prove nothing shipped, so check upstream before acting.

## Highlights

1. **Node.js security release (2026-10-21)**: v26.12.1, v24.22.1, v22.23.4 fix 10 CVEs, 4 High. These include a Brotli decoder out-of-bounds write (CVE-2026-111497) and DNS resolve truncating names at NUL (CVE-2026-105482). Patch now.
2. **Oracle Critical Patch Update (2026-10-20)**: 1,295 patches, 1,209 unique CVEs, six at CVSS 10.0. Database Server max CVSS is 8.8, affecting 19.3–19.32, 21.3–21.23 and 23.4.0–23.26.3.
3. **Java CPU**: JDK 21.0.13.0.3 was released on 2026-10-24. Oracle JDK updates from this CPU move to the Java SE OTN license and need a Java SE Universal Subscription. OpenJDK builds (Temurin, Zulu, Liberica) are not affected by the license change.
4. **PostgreSQL 19**: RC1 shipped on 2026-10-15. GA was scheduled for 2026-10-29. PostgreSQL 14 reaches EOL on 2026-11-12.

## Java

| Tool | Finding |
|---|---|
| Java core | JDK 21.0.13.0.3 (October CPU, released 2026-10-24) with security fixes. Oracle licensing change as above. Quarterly CPUs continue, plus the new monthly update release introduced in August. |
| Netty | No confirmed release in window. Netty 4.1 EOL was announced on 2026-09-09, so plan a move to 4.2. Several 2026 Netty CVEs were fixed earlier (e.g. SUSE advisory in July). |
| Vert.x | No confirmed release in window. |
| Spring | No confirmed release in window. Spring Boot 4.1 (released 2026-06-10), 4.0, 3.5 and 3.4 are supported as of 2026-10-16. Framework 7.x is current. |
| Hibernate | No confirmed ORM release in window. Latest seen: Hibernate Reactive 3.4.2.Final (2026-06-16, ORM 7.4 / Vert.x 5.1). |
| JDBC | No confirmed release in window. Oracle JDBC is covered by the October CPU. |
| Protobuf | No confirmed release in window (last seen 2026-03-26). |
| Dagger | No confirmed release in window. |
| Jackson | No confirmed release in window. |
| Log4j | No confirmed release in window (last seen 2.26.0, 2026-05-07). |
| Guava | No confirmed release in window (last seen 2026-04-29). |
| JUnit | No confirmed release in window. JUnit 6.x line is current (Jupiter up to 6.1.2 seen). |
| Maven | No confirmed release in window. |
| Gradle | No confirmed release in window. |

## Node.js

| Tool | Finding |
|---|---|
| Node core | 2026-10-21 coordinated security release: v26.12.1 (Lithium), v24.22.1 (Krypton), v22.23.4 (Jod). 10 CVEs, 4 High: Brotli decoder OOB write, DNS NUL truncation, TLS certificate validation issue. Node 18 and 20 are EOL and received no fix. From 2027 the project moves to one release a year (Node 27 LTS). |
| axios | No confirmed release in window. Latest seen: 0.33.0 (2026-06-13). A supply-chain compromise of axios occurred in March 2026, so pin versions and verify lockfiles. |
| bcryptjs | No confirmed release in window. |
| express | No confirmed release in window. |
| moment | No confirmed release in window. |
| pg | No confirmed release in window. |
| sequelize | No confirmed release in window. CVE-2026-30951 advisory exists from earlier in the year. |
| socket.io | No confirmed release in window. |
| jest | No confirmed release in window. |
| prettier | No confirmed release in window. |
| ReactJS | No confirmed release in window. React 19.2 is current. |
| React Native | No release in window. 0.86 is the latest seen (June 2026). 0.89 has branch cut on 2026-11-03 and release on 2026-12-07. Jest preset is now `@react-native/jest-preset` (since 0.85). |

## Database and Storage

| Tool | Finding |
|---|---|
| Oracle | October CPU (2026-10-20), details in Highlights. Also covers GoldenGate Stream Analytics, NoSQL Database and REST Data Services. Apply the patch to supported releases (19c, 21c, 23ai). |
| PostgreSQL | 19 RC1 (2026-10-15). GA scheduled for 2026-10-29, so confirm on postgresql.org. PG 14 EOL on 2026-11-12: upgrade to 18 or 19. |
| Redis | Redis Software for Kubernetes maintenance releases in October: 7.8.6-22, 7.22.2-47, 8.0.20-28, 8.2.0-19. Exact dates not confirmed. |
| Kafka | No confirmed release in window. Last seen 4.2.1 (June 2026). |
| MongoDB | No confirmed release in window. |
| Cassandra | No confirmed release in window. |
| ElasticSearch | No confirmed release in window. |
| BigQuery | Conversational analytics in BigQuery available. Cross-region replication and managed DR metrics in Cloud Monitoring are GA. AlloyDB can query BigQuery live via `bigquery_fdw` (GA). |

## OS and Platform

| Tool | Finding |
|---|---|
| Docker | Docker Engine 29.9.1 (2026-10-14); 29.9.0 on 2026-10-08. Patch releases on the 29 line. |
| Kubernetes | Supported lines are 1.36, 1.35 and 1.34 (latest 1.36.2). October patch releases were targeted for 2026-10-13. v1.38 enhancements freeze began on 2026-09-30. |
| GC Compute Engine | `EXTERNAL_PASSTHROUGH` load balancing scheme is GA in backend-service and forwarding-rule import/export. `instance-templates export/import` added to all gcloud tracks. |
| GKE | GKE Standard CPU node pools can upgrade nodes in place on the existing VM, instead of recreating them. Cloud Storage FUSE CSI driver is supported on Google Cloud Dedicated clusters (1.36.0-gke.1266000+). |
| GC Map API | No confirmed update in window. |
| GC Translate API | No confirmed update in window. |
| GC Storage | A Cloud Storage FUSE CSI driver issue published on 2026-10-02 is resolved. |
| Google Analytics | No confirmed update in window. |
| Amazon S3 | S3 server access logs can go to CloudWatch Logs and S3 Tables. S3 Tables support all Apache Iceberg V3 data types. |
| Amazon EC2 | EC2 Auto Scaling supports batch instance termination. |
| Amazon Lambda | Self-managed S3 buckets for function code (removes the 75 GB code storage limit). Durable functions integrate with Pydantic AI. |
| Amazon EKS | Kubernetes 1.37 available on EKS. |
| Amazon CloudFront | CloudFront Functions logging to access logs. OAC for S3 Multi-Region Access Points. |
| Amazon Route53 | Route 53 Global Resolver GA (anycast resolver for public and private-hosted-zone domains), with DNS analytics and DNS Firewall options. |
| Cloudflare | No confirmed update in window (only older billing and payment changes seen). |
| Firebase | No confirmed update in window. |
| Paypal | No confirmed update in window. |
| ApplePay | No confirmed update in window. |

## Recommended actions

1. Patch Node.js to 26.12.1, 24.22.1 or 22.23.4. Move off Node 18/20, which are EOL and unpatched.
2. Apply the Oracle October CPU. Review the Java licensing impact if you use Oracle JDK, or switch to OpenJDK builds.
3. Plan Netty 4.1 → 4.2 and PostgreSQL 14 → 18/19 before 2026-11-12.
4. Pin and audit npm dependencies, given the 2026 axios supply-chain incident.

## Sources

- [Oracle JDK 21.0.13.0.3 release notes](https://www.oracle.com/java/technologies/javase/21-0-13-0-3-relnotes.html)
- [Netty 4.1 EOL announcement](https://netty.io/news/2026/09/09/4-1-EOL-announcement.html)
- [Spring news roundup, Sep 21 2026 (InfoQ)](https://infoq.com/news/2026/09/spring-news-roundup-sep21-2026)
- [Hibernate Reactive releases](https://hibernate.org/reactive/releases)
- [Node.js October 2026 security releases](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases?assessment=20261021)
- [Node.js October 2026 security release (Astra)](https://www.getastra.com/blog/security-advisory/node-js-security-release/)
- [axios 0.33.0](https://newreleases.io/project/npm/axios/release/0.33.0)
- [React Native versions](https://reactnative.dev/versions)
- [Oracle CPU October 2026](https://www.oracle.com/security-alerts/cpuoct2026.html)
- [Oracle October 2026 CPU coverage](https://securityonline.info/oracle-critical-patch-update-october-2026/)
- [PostgreSQL 14 EOL guide](https://wz-it.com/en/blog/postgresql-14-end-of-life-upgrade-guide/)
- [PostgreSQL news archive 2026-10-18](https://www.postgresql.org/about/newsarchive/-/20261018/)
- [Redis Software for Kubernetes 8.2.0-19](https://redis.io/docs/latest/operate/kubernetes/release-notes/8-2-0-releases/8-2-0-19-october2026/)
- [Docker Engine releases](https://releases.sh/docker/docker-engine)
- [Last Week in Kubernetes Development](https://buttondown.com/LastWeekInKubernetes/archive/lwkd-week-ending-october-04-2026/)
- [Google Cloud release notes](https://docs.cloud.google.com/release-notes)
- [AWS What's New roundup](https://aws.amazon.com/blogs/aws/category/post-types/announcements)
