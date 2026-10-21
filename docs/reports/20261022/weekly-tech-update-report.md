# Weekly Tech Update Report: 2026-10-22

Window requested: the last 7 days (2026-10-15 to 2026-10-22).
Method: WebSearch only, one or two queries per tool, mostly standard mode.

**Caveat.** For most tools the searches did not return anything dated inside the 7-day window. Where that is the case I list the *latest release the search found*, with its date, and mark it **(older than 7 days)**. Entries are search-result summaries and were not checked against primary sources. Treat version numbers as leads to confirm before acting on them.

## Highlights (backend / security)

| Priority | Item |
|---|---|
| High | **Oracle CPU October 2026** (released 2026-10-20): 1283 patches overall, 13 for Oracle Database (3 remotely exploitable without authentication, max CVSS 8.8). Affects 19.3-19.32, 21.3-21.23 and 23.4.0-23.26.3. |
| High | **Next.js / React** security release on 2026-10-14: two critical RCEs (SVG image processing, `use cache`) and one DoS (App Router Server Actions). Upgrade `next` to 15.5.28 or 16.4.1. |
| High | **Redis**: CVE-2026-25243 (RESTORE invalid memory access, possible RCE) reported as actively exploited as of October 2026. Fixed in 6.2.22 and 7.2.14. Further RCE CVEs are also listed (CVE-2026-66373, CVE-2026-81934). |
| Medium | **Log4j**: CVE-2026-34477 reported 2026-10-19 (incomplete fix in Log4j Core; TLS man-in-the-middle). Latest releases found are 2.26.1 and 2.25.5. |
| Medium | **Express 5.3.0** released 2026-10-09. |
| Medium | **Firebase Apple SDK 13.0.0** (2026-10-07): CocoaPods dropped, minimum iOS 15. |
| Medium | **Cloudflare**: on 2026-10-05, CIDR-encoded route endpoints were removed from the Zero Trust Networks and Tunnel APIs, and the `connections` field was removed from tunnel list/get responses. |
| Info | **Node.js**: the release model changes with Node 27 in October (every release becomes LTS, yearly cadence). |

## 1. Java

| Tool | Latest found | Date | Notes |
|---|---|---|---|
| JDK | 26.0.2.1 (feature line); 21.0.12.1 (LTS) | 2026-08-18 **(older)** | JDK 26 GA was 2026-03-17. JDK 27 is in release-candidate/early-access. |
| Netty | 4.2.10.Final / 4.1.131.Final | 2026-02-06 **(older)** | Search surfaced nothing newer. Check netty.io/news. |
| Vert.x | 5.1.6, 4.5.32 | 4.5.32 on 2026-08-07 **(older)** | Regular patch releases. |
| Spring | Boot 4.2.0 and Framework 7.1.0 at milestone 2 | late Sep 2026 **(older)** | Milestones only. No GA found. |
| Hibernate ORM | 7.4.7.Final | after 2026-07 **(older)**; exact date not found | 7.4.x line. |
| PostgreSQL JDBC | 42.7.10 | 2026-02-15 **(older)** | Fixes a proleptic date regression. |
| Protobuf | v36.0-rc1 | recent pre-release | Edition 2026 changes: STRICT `default_symbol_visibility`, proto limits enforced by default for Edition 2026. Deprecation warnings added for generic service options. Likely breaking for generated code once GA. |
| Dagger | 2.60 | ~2026-07 **(older)** | |
| Jackson | databind 2.22.2 (2026-08-23); 3.2.x also current | **(older)** | 2.22.2 reportedly closes 12 vulnerabilities (polymorphic type validation, `@JsonView`/`@JsonIgnore` bypass, deserialization). Worth upgrading. |
| Log4j | 2.26.1 (2026-06-29); 2.25.5 (2026-07-01) | **(older)** | See Highlights for CVE-2026-34477, CVE-2026-49844 and CVE-2026-34478. Versions below 2.25 are unmaintained. |
| Guava | 33.6.0-jre | 2026-04-14 **(older)** | |
| JUnit | 6.1.2 confirmed (2026-07-12); 6.1.3 mentioned but not confirmed | **(older)** | 6.2.0-M1 in development. |
| Maven | 3.9.16 (2026-07-07); 3.10.0-rc-1 (2026-07-13) | **(older)** | Resolver 2.0.20 (2026-07-03). |
| Gradle | 9.7.1 | 2026-08-19 **(older)** | 9.7.0 promoted Isolated Projects to incubating; IDE sync on a 300-subproject build improved from 84s to 47s. |

## 2. Node.js

| Tool | Latest found | Date | Notes |
|---|---|---|---|
| Node.js | 26.x, 24.x and 22.x security releases | 2026-07-27 **(older)** | 11 CVEs (3 High, 5 Medium, 3 Low), including HTTP/2 use-after-free (CVE-2026-56848) and `--permission` sandbox escape (CVE-2026-58043). Node 18 and 20 are EOL. Node 27 and the new release model arrive in October. |
| axios | 1.15.1 (2026-04-19); 0.33.0 (2026-06-13) | **(older)** | 1.14.1 and 0.30.4 were compromised in March 2026 (RAT via malicious dependency; 1.14.0 is the safe version). CVE-2026-40175 (SSRF, CVSS 10, April). Three more high-severity CVEs on 2026-05-29. Check lockfiles. |
| bcryptjs | 3.0.2 | ~mid-2026 **(older)** | No known vulnerabilities. |
| express | 5.3.0 | **2026-10-09** | In window. 4.22.2 on 2026-05-11. Release notes not read. |
| moment | 2.31.0 | 2026-09-14 **(older)** | Closes CVE-2026-17495 (object prototype properties used as format tokens). |
| pg | last release 2026-06-19 | **(older)** | Version number not found. |
| sequelize | v7.0.0-alpha.48 | 2026-02-04 **(older)** | v7 still alpha. CVE-2026-30951 advisory exists (March). |
| socket.io | last JS release 2025-12-23; socket.io-adapter 2.5.8 (2026-06-16) | **(older)** | The adapter bumps `ws` to ~8.21.0 for CVE-2026-48779. |
| jest | 30.4 | 2026-05-09 **(older)** | |
| prettier | not found | n/a | Search returned nothing usable. |
| ReactJS | 19.2.x (19.2.7 tag seen) | 2026-10-14 security advisory | See Highlights. |
| React Native | 0.87.0-rc.2 (2026-07-20); 0.88 scheduled 2026-10-12 | 0.86.0 on 2026-06-09 | Whether 0.88 shipped was not confirmed. |

## 3. Database and Storage

| Tool | Latest found | Date | Notes |
|---|---|---|---|
| Oracle | CPU October 2026 | **2026-10-20** | See Highlights. |
| PostgreSQL | 18.6, 17.11, 16.15, 15.19, 14.24; 19 beta 3 | Aug 2026 **(older)** | That release fixed 28 security vulnerabilities and 110+ bugs. The May release (18.4, 17.10) fixed 11. |
| Redis | 8.4.2 (2026-02-23); 7.2.14; 6.2.22 (2026-05-05) | **(older)** | Actively exploited RESTORE CVE, see Highlights. |
| Kafka | 4.3.1 | 2026-06-25 **(older)** | Fixes a Kafka Streams RocksDB native memory leak (KAFKA-20616). 4.3.0 on 2026-05-22. KRaft only, no ZooKeeper. |
| MongoDB | 8.0.28 (2026-08-01); 8.2.12 (2026-07-28); 8.3.7 (2026-07-30) | **(older)** | 8.2 security support ended 2026-07-31. |
| Cassandra | 5.0.8 | 2026-04 **(older)** | |
| Elasticsearch | 9.4.8 / 9.5.5 security updates (ESA-2026-185, ESA-2026-190); 9.6.x line exists | 2026 **(older)** | The security updates fix uncontrolled recursion and resource consumption; earlier ones fixed HTTP request smuggling. |
| BigQuery | Release notes through Sept 2026 | **(older)** | GA of the Data Engineering Agent with BigQuery Graph integration. In August: Gemini 3.1 Flash Lite and 3.5 Flash GA for generative AI functions, cross-cloud connections, TabFM. |

## 4. OS and Platform

| Tool | Latest found | Date | Notes |
|---|---|---|---|
| Docker Engine | 29.9.1 (one result says 29.8.0) | 2026-10-14 | In window. Conflicting sources. 29.6.2 (mid-July) was security-focused. |
| Kubernetes | 1.36.4 (2026-08-11); 1.36.5 targeted 2026-09-15; 1.37.0-beta.0 | **(older)** | 1.37 beta: StatefulSet Recreate strategy, ClusterTrustBundle stable, `metrics.k8s.io` v1. |
| GCP Compute Engine | Z4D GA; C4N preview | Sept 2026 **(older)** | |
| GKE | Confidential nodes on c3-*-lssd (Intel TDX) GA; C3 bare metal GA; Gateway session affinity GA | Sept 2026 **(older)** | |
| GCP Maps API | none found | n/a | Only 2025 pages surfaced. |
| GCP Translate API | Translation LLM LoRA full fine-tuning | 2026-02-24 **(older)** | |
| GCP Storage | Storage Intelligence advisor GA | 2026-09-10 **(older)** | Rapid Cache prefix-level ingest-on-write filtering (2026-08-21). |
| Google Analytics | none found | n/a | |
| Amazon S3 | S3 Files, Account Regional Namespaces, S3 Vectors GA, S3 Tables, S3 Annotations | 2026 **(older)** | Exact dates not found. |
| Amazon EC2 | M8in/R8in/C8ine/M8ine (Apr); M8idn/R8idn (May) | **(older)** | |
| AWS Lambda | Lambda MicroVMs | Q2 2026 **(older)** | |
| Amazon EKS | Kubernetes 1.36 support | 2026-06-02 **(older)** | |
| CloudFront | mutual TLS for viewer connections, Functions capabilities | Nov 2025 **(older)** | |
| Route 53 | Global Resolver GA (Mar); view sharing via AWS RAM (Jun) | **(older)** | |
| Cloudflare | API removals (2026-10-05); Logpush `websocket_analytics` dataset | Oct 2026 | See Highlights. |
| Firebase | Apple SDK 13.0.0 | **2026-10-07** | See Highlights. Removes FirebaseMLModelDownloader; GoogleSignIn is no longer bundled in the zip and Carthage distributions. |
| PayPal | none found | n/a | Nothing newer than March 2026. |
| Apple Pay | no developer-facing update found | n/a | News is about India rollout and in-store installments. |

## Suggested actions

1. Plan the Oracle CPU October 2026 patching (19c, 21c, 23ai).
2. Check Redis versions against CVE-2026-25243, especially if RESTORE is reachable.
3. Check Next.js/React apps against the 2026-10-14 advisory.
4. Audit axios versions and lockfiles (compromised 1.14.1 and 0.30.4; the 2026 CVEs).
5. Review Log4j (CVE-2026-34477) and bump Jackson to 2.22.2 or later.
6. Update anything calling the removed Cloudflare tunnel/route API fields.
7. If using Firebase on iOS, plan the 13.0.0 migration (Swift Package Manager only).

## Sources

- [JDK versions](https://www.versio.io/en/product-release-end-of-life-eol-oracle-openjdk.html)
- [Netty news](https://netty.io/news)
- [Vert.x 4.5.32](https://vertx.io/blog/eclipse-vert-x-4-5-32)
- [InfoQ Spring news roundup, Sept 21 2026](https://infoq.com/news/2026/09/spring-news-roundup-sep21-2026)
- [Hibernate ORM 7.4.7](https://newreleases.io/project/github/hibernate/hibernate-orm/release/7.4.7)
- [pgjdbc 42.7.10](https://www.postgresql.org/about/news/postgresql-jdbc-42710-released-3234/)
- [Protobuf v36.0-rc1](https://newreleases.io/project/github/protocolbuffers/protobuf/release/v36.0-rc1)
- [Dagger](https://dagger.dev/)
- [Jackson Databind](https://www.versio.io/en/product-release-end-of-life-eol-fasterxml-jackson-databind.html)
- [Log4j versions (HeroDevs)](https://www.herodevs.com/blog-posts/apache-log4j-versions-eol-dates-and-latest-releases-august-2026)
- [Guava](https://badges.mvnrepository.com/artifact/com.google.guava/guava)
- [JUnit release notes](https://docs.junit.org/6.1.2/release-notes/)
- [Maven 3.9.16](https://tooling-vm-ec2-de.apache.org/api/cle/release/maven/3.9.16)
- [Gradle newsletter, Aug 2026](https://newsletter.gradle.org/html/2026-08-27-August2026/index.html)
- [Node.js July 2026 security releases](https://nodejs.org/uk/blog/vulnerability/july-2026-security-releases)
- [axios versions and CVEs (HeroDevs)](https://www.herodevs.com/blog-posts/axios-versions-cves-and-safe-upgrade-path-updated-april-2026)
- [axios 0.33.0](https://newreleases.io/project/npm/axios/release/0.33.0)
- [bcryptjs](https://npmjs.com/package/bcryptjs)
- [Express releases](https://www.versio.io/de/product-release-end-of-life-eol-openjs-foundation-express.html)
- [moment.js 2.31.0](https://tproger.ru/news/moment-js-2-31-0-zakryl-cve-2026-17495-i-ispravil-rabotu-s-proto)
- [pg releases](https://releasealert.dev/npmjs/_/pg/links)
- [Sequelize v7](https://sequelize.org/v7/)
- [socket.io](https://releasealert.dev/npmjs/_/socket.io/links)
- [Jest versions](https://jestjs.io/versions)
- [Next.js/React security, Netlify](https://www.netlify.com/changelog/2026-10-14-nextjs-react-security-vulnerabilities.md)
- [React Native releases](https://reactnative.dev/docs/releases)
- [Oracle CPU October 2026](https://www.oracle.com/security-alerts/cpuoct2026.html)
- [PostgreSQL 18.6 / 19 beta 3](https://www.postgresql.org/about/news/postgresql-186-1711-1615-1519-1424-and-19-beta-3-released-3365/)
- [Redis 2026 errata (AlmaLinux)](https://errata.almalinux.org/9/ALSA-2026-25219.html)
- [Kafka 4.2.1](https://kafka.apache.org/blog/2026/05/30/apache-kafka-4.2.1-release-announcement/)
- [MongoDB EOL](https://endoflife.date/mongodb)
- [Cassandra release notes](https://axonops.com/docs/data-platforms/cassandra/release-notes/)
- [Elasticsearch ESA-2026-190](https://discuss.elastic.co/t/390863)
- [BigQuery release notes](https://docs.cloud.google.com/bigquery/docs/release-notes)
- [Docker Engine EOL](https://endoflife.date/docker-engine%20)
- [Kubernetes 1.36](https://kubernetes.io/releases/1.36/)
- [Compute Engine release notes](https://docs.cloud.google.com/compute/docs/release-notes)
- [GKE release notes](https://docs.cloud.google.com/kubernetes-engine/docs/release-notes-new-features)
- [Cloud Translation release notes](https://docs.cloud.google.com/translate/release-notes)
- [Cloud Storage release notes](https://docs.cloud.google.com/storage/docs/release-notes)
- [AWS News Blog, Storage](https://aws.amazon.com/blogs/aws/category/storage)
- [EC2 instance types doc history](https://docs.amazonaws.cn/en_us/ec2/latest/instancetypes/doc-history.html)
- [AWS Lambda MicroVMs, ICYMI Q2 2026](https://aws.amazon.com/pt/blogs/aws-brasil/serverless-icymi-q2-2026/)
- [EKS 1.36](https://aws.amazon.com/pt/about-aws/whats-new/2026/06/amazon-eks-distro-kubernetes-version-1-36/)
- [CloudFront updates](https://releasebot.io/updates/amazon/amazon-cloudfront)
- [Route 53 Global Resolver](https://aws.amazon.com/about-aws/whats-new/2026/06/amazon-route-53-global-resolver/)
- [Cloudflare changelog](https://developers.cloudflare.com/changelog/product/cloudflare-tunnel-sase/)
- [Firebase Apple SDK release notes](https://firebase.google.com/support/release-notes/ios?hl=en)
- [PayPal open release notes](https://paypal.com/us/brc/article/paypal-open-release-notes)
- [Apple Pay 2026 features](https://www.digitalcitizen.life/apple-pays-newest-features-in-2026-what-has-actually-changed/)
