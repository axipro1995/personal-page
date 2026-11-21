# Weekly Tech Update Report — Week Ending 2026-11-05

Window: 2026-10-29 → 2026-11-05. Compiled from web searches, which returned thin and mostly secondary results. "No confirmed release" means search did not surface a release in the window; it does not prove nothing shipped, so check upstream before acting. This report was generated late (on 2026-11-21), so some items below are dated after the window and are labelled as such.

## Highlights

1. **Gradle 9.9.0**: listed in InfoQ's 2026-11-02 Java roundup (incremental compilation of Kotlin DSL scripts). Exact release date not shown; treat as unconfirmed for the window. Gradle 9.9.1 followed on 2026-11-10 with 3 security fixes.
2. **Spring Boot 4.2.0 RC2**: reported in InfoQ's 2026-11-09 roundup (bug fixes, dependency upgrades). GA was not confirmed in the window.
3. **React Native 0.89.0-rc.0**: tracked as released 2026-11-04. rc.1 followed on 2026-11-09. No final 0.89.0 seen.
4. **PostgreSQL 19**: the October 29 GA target slipped. The official announcement is dated 2026-11-19 (after the window). PostgreSQL 14 reaches EOL on 2026-11-12.
5. **Node.js**: no new release in the window. The latest security release is still 2026-10-21 (v26.12.1, v24.22.1, v22.23.4). Node 18 and 20 are EOL.

## Java

| Tool | Finding |
|---|---|
| Java core | No confirmed release in window. Latest seen: JDK 21.0.13 (October CPU, 2026-10-24). JDK 28 JEPs (Lazy Constants, AOT code compilation) are being targeted. |
| Netty | No confirmed release in window. Netty 4.1 is EOL (announced 2026-09-09); move to 4.2. |
| Vert.x | A Baeldung weekly lists 5.2.2; date not confirmed. |
| Spring | Spring Boot 4.2.0-RC2 (reported 2026-11-09 roundup). No GA confirmed in window. |
| Hibernate | Hibernate ORM and Reactive betas listed in the 2026-10-26 roundup. No GA confirmed. |
| JDBC | No confirmed release in window. |
| Protobuf | No confirmed release in window. |
| Dagger | No confirmed release in window. |
| Jackson | No confirmed release in window. A jsr310 module release on 2026-10-19 was relocated to the `tools.jackson` group. |
| Log4j | No confirmed release in window. Dependency reports show 2.25.2; a 3.0.0 beta line exists. |
| Guava | No confirmed release in window. |
| JUnit | No confirmed release in window. |
| Maven | No confirmed release in window. Maven 4.0 RC7 was seen in September 2026. |
| Gradle | 9.9.0 (Kotlin DSL incremental compilation), per the 2026-11-02 roundup; exact date unconfirmed. |

## Node.js

| Tool | Finding |
|---|---|
| Node core | No new release in window. See the 2026-10-21 security release above. |
| axios | No confirmed release in window. Changelogs show 1.18.x/1.19.0 lines on unpkg, with no dates confirmed. |
| bcryptjs | No confirmed release in window. |
| express | No confirmed release in window. |
| moment | No confirmed release in window. |
| pg | No confirmed release in window. |
| sequelize | No confirmed release in window. |
| socket.io | No confirmed release in window. |
| jest | No confirmed release in window. |
| prettier | No confirmed release in window. |
| ReactJS | No confirmed release in window. |
| ReactNative | 0.89.0-rc.0 on 2026-11-04 (release candidate only). |

## Database and Storage

| Tool | Finding |
|---|---|
| Oracle | No new release in window. The October CPU (2026-10-20) is the latest. |
| PostgreSQL | 19 GA slipped past 2026-10-29; announced 2026-11-19. PostgreSQL 14 EOL 2026-11-12. |
| Redis | No confirmed release in window. |
| Kafka | No confirmed release in window. |
| MongoDB | No confirmed release in window. |
| Cassandra | No confirmed release in window. |
| ElasticSearch | No confirmed release in window. |
| BigQuery | No confirmed release in window. |

## OS and Platform

| Tool | Finding |
|---|---|
| Docker | No confirmed release in window. Latest seen in search: Engine 29 notes dated 2026-10-08 (earlier report noted 29.9.1 on 2026-10-14). |
| Kubernetes | No confirmed release in window. |
| GC Compute Engine | No confirmed update in window. |
| GKE | No confirmed update in window. Latest seen: September 2026 notes. Cloud Service Mesh in-cluster ISTIOD is deprecated (2026-09-28), support ends 2028-03-01. |
| GC Map API / Translate API / Storage / Analytics | No confirmed update in window. |
| Amazon S3 / EC2 / Lambda / EKS / CloudFront / Route53 | No confirmed update in window (search returned nothing). |
| Cloudflare / Firebase / PayPal / ApplePay | No confirmed update in window. |

## Recommended actions

- Confirm Node.js is on 26.12.1, 24.22.1 or 22.23.4, and move off Node 18/20.
- Plan the PostgreSQL 14 upgrade (EOL 2026-11-12) and evaluate 19.
- Plan the Netty 4.1 → 4.2 move.
- Check the official release pages for the gaps above, since search coverage was weak this week.
