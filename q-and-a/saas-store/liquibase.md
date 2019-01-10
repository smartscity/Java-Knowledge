# Liquibase

## About Liquibase

Liquibase是一个用于跟踪、管理和应用数据库变化的开源的数据库重构工具；它将所有数据库的变化\(包括结构和数据\)都保存在文件中\(见附录2\)，便于版本控制。

### `Liquibase` 具备如下特性: 

* **不依赖于特定的数据库**
  * supported databases （见附录1）
* **提供数据库比较功能**
  * 比较结果保存在XML中，基于该XML你可用Liquibase轻松部署或升级数据库
* **多人协作**
  * 以XML存储数据库变化,其中以作者和ID唯一标识一个变化\(ChangSet\),支持数据库变化的合并,因此支持多开发人员同时工作
* **历史版本留痕**
  * 在数据库中保存数据库修改历史\(DatabaseChangeHistory\),在数据库升级时自动跳过已应用的变化 \(ChangSet\)
* **回滚**
  * 提供变化应用的回滚功能,可按时间、数量或标签\(tag\)回滚已应用的变化。通过这种方式,开发人员可轻易的还原数据库在任何时间点的状态
* **生成变化文档**
  * 可生成数据库修改文档\(HTML格式\)   （见`Liquibase Command 章节`）



## How to used?

### 第一步 导入依赖

#### `build.gradle` Gradle import

{% code-tabs %}
{% code-tabs-item title="build.gradle" %}
```yaml
compile("org.liquibase:liquibase-core")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### `pom.xml` Maven import

{% code-tabs %}
{% code-tabs-item title="pom.xml" %}
```markup
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
</dependency>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 第二步 配置Springboot配置文件

#### Configuration  `application.yml`

* 需要在 `application.yml`中，增加`liquibase`配置

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
liquibase:
  change-log: classpath:config/liquibase/master.xml
  drop-first: false      #优先drop整库之后，顺序执行 master.xml
  enabled: true          #开启Liquibase
  check-change-log-location: true
```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```

```
{% endcode-tabs-item %}
{% endcode-tabs %}

* **图例**

![](../../.gitbook/assets/image%20%2816%29.png)

### 第三步 配置Liquibase部署文件

#### Configuration master.xml

* 在`master.xml`中，引入 【表结构】、【索引】、【数据库脚本升级包】

{% code-tabs %}
{% code-tabs-item title="master.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd">

    <!--  初始化 - we will add liquibase init changelogs here -->
    <include file="config/liquibase/changelog/20180628072446_added_entity.xml" relativeToChangelogFile="false"/>

    <!-- 变化 - we will add liquibase changelogs here -->
    <include file="config/liquibase/changelog/20180628072447_added_column.xml" relativeToChangelogFile="false"/>
    <include file="config/liquibase/changelog/20180628072448_modify_column.xml" relativeToChangelogFile="false"/>
    <include file="config/liquibase/changelog/20180628072449_insert_data.xml" relativeToChangelogFile="false"/>
    <include file="config/liquibase/changelog/20180628072451_drop_column.xml" relativeToChangelogFile="false"/>
    <include file="config/liquibase/changelog/20180628072452_drop_column.xml" relativeToChangelogFile="false"/>

    <!-- 索引 - we will add liquibase changelogs here -->
    <include file="config/liquibase/changelog/20180628072450_added_index.xml" relativeToChangelogFile="false"/>

    <!-- 约束 - we will add liquibase constraints changelogs here -->
</databaseChangeLog>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Configuration Liquibase

### 介绍

* **ChangeSet 变化的集合，必须唯一**
* **ChangeSet Supported**

<table>
  <thead>
    <tr>
      <th style="text-align:left">ChangeSet 下的功能</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">e.g.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">createTable</td>
      <td style="text-align:left">创建表</td>
      <td style="text-align:left">
        <createTable tableName="custom">
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dropTable</td>
      <td style="text-align:left">销毁表</td>
      <td style="text-align:left">
        <dropTable tableName="custom">
      </td>
    </tr>
    <tr>
      <td style="text-align:left">addColumn</td>
      <td style="text-align:left">加一列</td>
      <td style="text-align:left">
        <p>
          <addColumn tableName="custom">
        </p>
        <p>
          <column name="username"></column>
        </p>
        <p>
          </addColumn>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">modifyDataType</td>
      <td style="text-align:left">修改列</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">mergeColumns</td>
      <td style="text-align:left">合并列</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dropColumn</td>
      <td style="text-align:left">销毁列</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">insert</td>
      <td style="text-align:left">插入数据</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">delete</td>
      <td style="text-align:left">删除数据</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">createIndex</td>
      <td style="text-align:left">创建索引</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dropIndex</td>
      <td style="text-align:left">销毁索引</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

### 我要创建表结构

{% code-tabs %}
{% code-tabs-item title="20180628072446\_added\_entity.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd
                        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">

    <property name="now" value="now()" dbms="h2"/>
    <property name="now" value="now()" dbms="mysql"/>
    <property name="autoIncrement" value="true"/>
    <property name="floatType" value="float4" dbms="postgresql, h2"/>
    <property name="floatType" value="float" dbms="mysql, oracle, mssql"/>

    <!--
        Added the entity custom.
    -->
    <changeSet id="20180628072446-1" author="suixingpay">
        <createTable tableName="custom">
            <column name="id" type="bigint" autoIncrement="${autoIncrement}">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="username" type="varchar(11)" remarks="用户姓名">
                <constraints nullable="false" />
            </column>

            <column name="password" type="varchar(64)"  remarks="用户密码">
                <constraints nullable="false" />
            </column>

            <column name="mobile" type="varchar(11)"    remarks="手机号">
                <constraints nullable="false" />
            </column>

            <column name="password_strength" type="integer" remarks="密码强壮程度">
                <constraints nullable="false" />
            </column>

            <column name="nick_name" type="varchar(32)" remarks="昵称">
                <constraints nullable="true" />
            </column>

            <column name="full_name" type="varchar(32)" remarks="姓名">
                <constraints nullable="true" />
            </column>

            <column name="email" type="varchar(64)" remarks="邮箱">
                <constraints nullable="true" />
            </column>

            <column name="id_type" type="integer" remarks="证件类型">
                <constraints nullable="true" />
            </column>

            <column name="id_card" type="varchar(255)" remarks="证件号码">
                <constraints nullable="true" />
            </column>
            <column name="active" type="boolean" defaultValueBoolean="true" remarks="激活"/>
            <column name="bbbb" type="boolean" defaultValueBoolean="true" remarks="激活1"/>
            <column name="register_time" type="datetime" defaultValueComputed="${now}">
                <constraints nullable="false" />
            </column>

            <!-- needle-liquibase-add-column - we will add columns here, do not remove-->
        </createTable>



    </changeSet>
    <!-- needle-liquibase-add-changeset - we will add changesets here, do not remove-->
</databaseChangeLog>


```
{% endcode-tabs-item %}
{% endcode-tabs %}



### 我要建索引

{% code-tabs %}
{% code-tabs-item title="20180628072450\_added\_index.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd
                        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">
    <!--
        modify mobile of the entity custom   length = 11 + 2 = 13.
    -->
    <changeSet id="20180628072450-1" author="suixingpay">

        <createIndex tableName="custom" indexName="idx_custom_username_mobile_nickname_fullname_registerxtime" unique="false">
            <column name="username"/>
            <column name="mobile"/>
            <column name="nick_name"/>
            <column name="full_name"/>
            <column name="register_time"/>
        </createIndex>
    </changeSet>

</databaseChangeLog>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 如何初始化数据

{% code-tabs %}
{% code-tabs-item title="20180628072449\_insert\_data.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd
                        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">
    <changeSet id="20180628072450-1" author="suixingpay" >
        <insert tableName="custom">
            <column name="username" value="liyunlong"/>
            <column name="password" value="123456"/>
            <column name="mobile" value="18651698213"/>
            <column name="password_strength" value="1"/>
            <column name="nick_name" value="昵称"/>
            <column name="full_name" value="姓名"/>
            <!-- <column name="status" value="1"/> -->
        </insert>
    </changeSet>
</databaseChangeLog>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 制作数据库升级包

* addColumn 增加列
* modifyDataType 修改列的数据类型或长度等
* dropColumn 删除列定义

```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd
                        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">
    <!--
        Added column of the entity custom .
    -->
    <changeSet id="20180628072447-1" author="suixingpay">
        <addColumn tableName="custom">
            <column name="status" type="tinyint">
                <constraints nullable="false"></constraints>
            </column>
        </addColumn>
    </changeSet>
    
    <!--
        modify mobile of the entity custom   length = 11 + 2 = 13.
    -->
    <changeSet id="20180628072448-1" author="suixingpay">
        <modifyDataType tableName="custom" columnName="mobile" newDataType="varchar(13)"/>
    </changeSet>

    <!--
        drop bbbb of the entity custom
    -->
    <changeSet id="20180628072451-1" author="suixingpay">
        <dropColumn tableName="custom" columnName="bbbb" />
    </changeSet>
</databaseChangeLog>

```

### 



## Advanced Usage

### Preconditions 条件判断 <a id="sample-with-preconditions"></a>

* 简单判断

```markup
    <changeSet id="1" author="bob">
        <preConditions onFail="WARN">
            <sqlCheck expectedResult="0">select count(*) from oldtable</sqlCheck>
        </preConditions>
        <comment>Comments should go after preCondition. If they are before then liquibase usually gives error.</comment>
        <dropTable tableName="oldtable"/>
    </changeSet>
```

* Will require running on Oracle OR MySQL which makes more sense than the above example.

```markup
 <preConditions>
     <or>
         <and>
            <dbms type="oracle" />
            <runningAs username="SYSTEM" />
         </and>
         <and>
            <dbms type="mysql" />
            <runningAs username="root" />
         </and>
     </or>
 </preConditions>
```

### Parameters 应用

* oracle 和 mysql 在一些 type 命名是不一样的，可以通过此种方式做通用适配

| Attribute | Description |
| :--- | :--- |
| name | Name of the table's schema **required** |
| value | Name of the column's table **required** |
| context | Contexts given as comma separated list. |
| dbms | Database types given as comma separated list. |

```markup
    <property name="clob.type" value="clob" dbms="oracle"/>
    <property name="clob.type" value="longtext" dbms="mysql"/>

    <changeSet id="1" author="joe">
         <createTable tableName="${table.name}">
             <column name="id" type="int"/>
             <column name="${column1.name}" type="${clob.type}"/>
             <column name="${column2.name}" type="int"/>
         </createTable>
    </changeSet>
```

### Database “Diff”

> [http://www.liquibase.org/documentation/diff.html](http://www.liquibase.org/documentation/diff.html)

## Liquibase Command <a id="liquibase-command-line"></a>

### 反向生成 `Liquibase` from database

* [download liquibase.jar](http://download.liquibase.org/download)
* view `liquibase`

![](../../.gitbook/assets/image%20%2819%29.png)

* **将已有数据库结构生成**`liquibase.xml`

{% code-tabs %}
{% code-tabs-item title="generate.sh" %}
```bash
./liquibase --driver=com.mysql.jdbc.Driver \  
                --classpath=lib/ \
                --changeLogFile=config/liquibase/changelog/db.changelog.xml \
                --url="jdbc:mysql://172.16.60.247:3306/suixingpay-starter-demo?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&zeroDateTimeBehavior=convertToNull" \
                --username=fd \
                --password=123456 \
                generateChangeLog

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 生成变更文档

* 将变更记录生成文档

```text
./liquibase --driver=com.mysql.jdbc.Driver \  
                --classpath=lib/ \
                --changeLogFile=config/liquibase/changelog/db.changelog.xml \
                --url="jdbc:mysql://172.16.60.247:3306/suixingpay-starter-demo?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&zeroDateTimeBehavior=convertToNull" \
                --username=fd \
                --password=123456 \
                dbDoc \
                docs/dbdoc/
```

* 图示

![](../../.gitbook/assets/image%20%2839%29.png)

### More Command

> [http://www.liquibase.org/documentation/command\_line.html](http://www.liquibase.org/documentation/command_line.html)

## 附录

### 1.Supported Databases

| Database | Type Name | Notes |
| :--- | :--- | :--- |
| MySQL | mysql | No Issues |
| PostgreSQL | postgresql | 8.2+ is required to use the "drop all database objects" functionality. |
| Oracle | oracle | 11g driver is required when using the diff tool on databases running with AL32UTF8 or AL16UTF16 |
| Sql Server | mssql | No Issues |
| Sybase\_Enterprise | sybase | ASE 12.0+ required. "select into" database option needs to be set. Best driver is JTDS. Sybase does not support transactions for DDL so rollbacks will not work on failures. Foreign keys can not be dropped which can break the rollback or dropAll functionality. |
| Sybase\_Anywhere | asany | **Since 1.9** |
| DB2 | db2 | No Issues. Will auto-call REORG when necessary. |
| [Apache\_Derby](http://www.liquibase.org/apache_derby.html) | derby | No Issues |
| HSQL | hsqldb | No Issues |
| H2 | h2 | No Issues |
| [Informix](http://www.liquibase.org/informix.html) | informix | No Issues |
| Firebird | firebird | No Issues |
| [SQLite](http://www.liquibase.org/sqlite.html) | sqlite | No Issues |



### 2.Works with You

* Supports code branching and merging
* Supports multiple developers
* Supports [multiple database types](http://www.liquibase.org/databases.html)
* Supports [XML](http://www.liquibase.org/documentation/xml_format.html), [YAML](http://www.liquibase.org/documentation/yaml_format.html), [JSON](http://www.liquibase.org/documentation/json_format.html) and [SQL](http://www.liquibase.org/documentation/sql_format.html) formats
* Supports [context-dependent logic](http://www.liquibase.org/documentation/contexts.html)
* Cluster-safe database upgrades
* Generate [Database change documentation](http://www.liquibase.org/documentation/dbdoc.html)
* Generate Database "[diffs](http://www.liquibase.org/documentation/diff.html)"
* [Run through your](http://www.liquibase.org/documentation/running.html) build process, embedded in your application or on demand
* Automatically [generate SQL scripts](http://www.liquibase.org/documentation/sql_output.html) for DBA code review
* Does not require a [live database connection](http://www.liquibase.org/documentation/offline.html)

