# Codegen

## 方案一 （Domain-Driven）

### Domain Language（推荐）（领域语言驱动代码生成）

* 勾选依赖 架构部组件
* RabbitMQ、Redis【单点、集群、哨兵】
* application 一个微服务工程
* applicationType gateway、microservice、monolith\[all in one\]
* prodDatabaseType mysql、postgresql、mongodb
* buildTool gradle、maven
* serverPort 服务端口号
* **Blob \(byte\[\]\)**
  * `AnyBlob` or just `Blob` to create a field of the “any” binary type;
  * `ImageBlob` to create a field meant to be an image.
  * `TextBlob` to create a field for a CLOB \(long text\).
* \`\`\`text

  application {

  config {

    baseName mygateway

    applicationType gateway

    prodDatabaseType mysql

    serverPort 10000

    buildTool gradle

  }

  }

application { config { baseName myapp applicationType microservice prodDatabaseType mysql serverPort 8080 buildTool gradle } entities Custom except C, D }

DEFAULT\_MIN\_LENGTH = 1 DEFAULT\_MAX\_LENGTH = 32 MAX\_LENGTH\_MOBILE = 11 PATTERN\_MOBILE = 888 MAX\_LENGTH\_LS = 64 MAX\_LENGTH\_YLES = 1024 MAX\_LENGTH\_ELSB = 2048 DEFAULT\_MIN\_BYTES = 20 DEFAULT\_MAX\_BYTES = 4096 DEFAULT\_MIN = 0 DEFAULT\_MAX = 100

/\*\*

* Class comments.
* @author The Suixingpay team.

  \*/

  entity Custom {

   /_\* A required attribute , 这个注解会生成swagger notes里_ /

   username String required minlength\(MAX\_LENGTH\_MOBILE\) maxlength\(MAX\_LENGTH\_MOBILE\) pattern\(/^1\[1,2,3,4,5,6,7,8,9\]\d{9}$/\)

   password String required minlength\(8\) maxlength\(MAX\_LENGTH\_LS\)

   mobile String required minlength\(MAX\_LENGTH\_MOBILE\) maxlength\(MAX\_LENGTH\_MOBILE\) pattern\(/^1\[1,2,3,4,5,6,7,8,9\]\d{9}$/\)

   passwordStrength Integer required min\(DEFAULT\_MIN\) max\(DEFAULT\_MAX\)

   nickName String minlength\(4\) maxlength\(DEFAULT\_MAX\_LENGTH\)

   fullName String minlength\(2\) maxlength\(DEFAULT\_MAX\_LENGTH\)

   email String minlength\(8\) maxlength\(MAX\_LENGTH\_LS\)

   idType Integer

  }

enum Language { FRENCH, ENGLISH, SPANISH } entity Book { title String required, description String, language Language } entity Author { name String required }

relationship OneToMany { Author{book} to Book{writer\(name\) required} }

relationship \(OneToMany \| ManyToOne \| OneToOne \| ManyToMany\) { \[{\[\(\)\]}\] to \[{\[\(\)\]}\]

## }

dto Book,Author with mapstruct except Custom service \* with serviceClass except Custom paginate Book,Custom with pager

\`\`\`

### 解析器

* 解析 Domain Language
* 解析 applicationType
* 解析 applicationType 与 Entity 关系
* 解析 Entity 与 Entity 关系  `(OneToMany | ManyToOne | OneToOne | ManyToMany)` 。
* 解析 dto
* 解析 service
* 解析 paginate

### 适配器

* 生成基础 starter-demo
* 生成 swagger controller代码
* 生成 service
* 生成 domain
* 生成UI 

## 方案二

### 数据库设计反向生成代码

类似 金昊 的实践方案

