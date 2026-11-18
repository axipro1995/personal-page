# Weekly Technology Updates — Week Ending 2026-11-19

Window: 2026-11-12 → 2026-11-19. Compiled from web search; many results were secondary sources. Verify against official release notes before acting. "No confirmed release" means nothing dated in the window was found, not that nothing shipped.

## Highlights

- **PostgreSQL (Nov 12):** 18.7, 17.12, 16.16, 15.20 and 14.25 fix 52 CVEs. The worst is CVE-2026-6460 (CVSS 8.8, code execution as the OS account). **PostgreSQL 14 is now EOL.** Patch soon.
- **Node.js (Nov 11):** security releases v26.13.1, v24.22.3 and v22.23.5 fix about 16 flaws (7 high), mostly HTTP/2 and TLS. Node 20 and older are EOL and unpatched.
- **Oracle CPU (Nov 17):** the Critical Security Patch Update was scheduled, with 7 new Database patches (19.3–19.33, 21.3–21.24, 23.4–23.26.4). The final advisory was not confirmed. Oracle says JDK 25.0.5 should not be used after this CPU.
- **Spring (Nov 19, scheduled):** Boot 4.2.0, Framework 7.1.0 and Cloud 2026.0.0 were due on the release calendar. Not confirmed as shipped. Framework 7.1 deprecates `RestTemplate` in favour of `RestClient`, with removal in 8.
- **Gradle:** 9.9.0 came out Nov 6. 9.9.1 followed Nov 10 and fixes 3 security vulnerabilities, so upgrade to 9.9.1.

## Java

| Tool | Latest found | Notes |
|---|---|---|
| Java core | JDK 27.0.1.0.3 (Oracle), 25.0.5, 21.0.13, 17.0.21 | The Nov 17 CPU was scheduled. Oracle's 27.0.1.0.3 notes mention a security alert fix (CVE-2026-114820). The release date was not confirmed. |
| Netty | 4.2.17.Final (via Vert.x 5.1.6) | Includes a few vulnerability fixes. No November release date confirmed. |
| Vert.x | 5.1.x (5.1.6 / 5.1.8), 4.5.34 | 4.5.34 was published Sep 10. 4.x gets bug fixes until Apr 2027. No Nov release confirmed. |
| Spring | Boot 4.2.0 / Framework 7.1.0 scheduled Nov 19 | See Highlights. Boot 4.0.x support ends Dec 31, 2026. |
| Hibernate | ORM 7.4.1 (Jun 9), 8.0 in beta | No Nov release confirmed. A forum thread reports a regression since 7.1.28. |
| JDBC | No confirmed update | Driver updates track the database releases above. |
| Protobuf | No Nov release found | protobuf-java last seen Mar 2026. |
| Dagger | No Nov release found | Hilt 2.59.2 adds AGP 9 support. |
| Jackson | 2.21 LTS, 3.1 LTS, 3.2 | No Nov release confirmed. |
| Log4j | 2.25.2 (2.x), 3.0.0-beta3 | No Nov release confirmed. |
| Guava | 33.5.0 and 33.6.0-jre | No Nov release confirmed. |
| JUnit | 6.x (6.1.2 seen) | No Nov release confirmed. |
| Maven | No release found | |
| Gradle | 9.9.0 (Nov 6), 9.9.1 (Nov 10) | 9.9.1 fixes 3 security vulnerabilities. |

## Node.js

| Tool | Latest found | Notes |
|---|---|---|
| Node core | 26.13.1, 24.22.3, 22.23.5 (Nov 11) | Security release, see Highlights. |
| axios | last seen Aug 26 (v1.16.x line) | No Nov release found. |
| express | last seen May 11 | No Nov release found. |
| socket.io | last seen Dec 2025 (tracker may be stale) | No Nov release found. |
| bcryptjs, moment, pg, sequelize, jest, prettier | No information found | |
| React | No Nov release found | React Foundation (Linux Foundation) since Feb 2026. |
| React Native | 0.88 stable (Oct 19). 0.89.0-rc.0 (Nov 4) and rc.1 (Nov 9). | 0.89 stable is planned for Dec 7. 0.88 had no breaking changes. 0.85 moved the Jest preset to `@react-native/jest-preset`. |

## Database and Storage

| Tool | Latest found | Notes |
|---|---|---|
| PostgreSQL | 18.7, 17.12, 16.16, 15.20, 14.25 (Nov 12) | 52 CVEs. 14 is EOL. PG 19 RC2 is out and GA is planned Nov 19. |
| Oracle | CPU Nov 17 (scheduled) | 7 Database patches. The final advisory was not confirmed. |
| Redis | Open Source 8.12 (RC, Oct) | Redis Software 7.4 reaches EOL Nov 30. |
| Kafka, MongoDB, Cassandra, Elasticsearch | No Nov release found | The Percona Search for MongoDB GA is not an official MongoDB release. |
| BigQuery | Table-level lineage for pipelines is GA. Gemini model support was noted, undated. | From the Google Cloud release notes. |

## OS and Platform

| Tool | Notes |
|---|---|
| Docker | Engine 29.9.3 (Nov 3), Desktop 4.95.0 (Oct 14). No release in the window found. |
| Kubernetes | v1.37 is the latest minor found (Aug). KubeCon NA news only in the window. |
| GCP (Compute Engine, GKE, Map, Translate, Storage, Analytics) | No dated November items confirmed. Other notes: API Keys remote MCP server GA (Nov 12). TLS 1.3 for API Gateway backends (Nov 10). App Engine direct VPC egress GA. |
| AWS (S3, EC2, Lambda, EKS, CloudFront, Route 53) | No service-specific November announcements found. See the weekly roundups of Nov 2 and Nov 9. Security Hub SBOM inventory covers EC2, ECR and Lambda. |
| Cloudflare | Billing checkout now supports Apple Pay, Google Pay, Link and PayPal (undated). Budget alerts on by default for eligible pay-as-you-go accounts. |
| Firebase, PayPal, Apple Pay | No relevant November updates found. |

## Recommended actions

1. Patch PostgreSQL to the latest minor release and plan the move off 14.
2. Upgrade Node to 22.23.5, 24.22.3 or 26.13.1. Move off Node 20.
3. Apply the Oracle Nov CPU to Database and JDK.
4. Upgrade Gradle to 9.9.1.
5. Prepare for Spring Boot 4.2 / Framework 7.1. Start migrating `RestTemplate` to `RestClient`.

## Sources

- [Oracle JDK 25.0.5 release notes](https://www.oracle.com/java/technologies/javase/25-0-5-0-2-relnotes.html)
- [Oracle JDK 27.0.1.0.3 release notes](https://www.oracle.com/java/technologies/javase/27-0-1-0-3-relnotes.html)
- [Vert.x blog](https://vertx.io/blog/eclipse-vert-x-5-1-6)
- [InfoQ Spring news roundup (Oct 19, 2026)](https://infoq.com/news/2026/10/spring-news-roundup-oct19-2026)
- [Spring Boot 4.1 support calendar](https://ecorpit.com/spring-boot-4-1-end-of-support-2027/)
- [Hibernate ORM on endoflife.date](https://endoflife.date/hibernate-orm)
- [Gradle 9.9.1 release notes](https://docs.gradle.org/9.9.1/release-notes.html)
- [Node.js Nov 2026 security releases (secondary)](https://braindetox.kr/en/posts/nodejs_security_release_2026_11.html)
- [oss-security: Node.js release (Nov 11)](https://www.openwall.com/lists/oss-security/2026/11/11/25)
- [React Native 0.88](https://reactnative.dev/blog/2026/10/19/react-native-0.88)
- [React Native releases](https://reactnative.dev/releases/overview)
- [PostgreSQL 52 CVEs (Phoronix)](https://phoronix.com/news/PostgreSQL-52-CVEs)
- [Oracle CSPU Nov 2026 pre-release](https://www.oracle.com/security-alerts/cspunov2026.html)
- [Redis 8.12 release notes](https://redis.io/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/redisos-8.12-release-notes/index.html.md)
- [Docker Engine on endoflife.date](https://endoflife.date/docker-engine)
- [Google Cloud release notes](https://cloud.google.com/release-notes?hl=en)
- [Cloudflare billing changelog](https://developers.cloudflare.com/changelog/product/billing/)
