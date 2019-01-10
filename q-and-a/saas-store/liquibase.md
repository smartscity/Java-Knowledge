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
  * 提供变化应用的回滚功能,可按时间、数量或**标签\(tag\)**回滚已应用的变化。通过这种方式,开发人员可轻易的还原数据库在任何时间点的状态
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

### 第二步 配置`application.yml`配置文件

#### Configuration  `application.yml`

* 需要在 `application.yml`中，增加`liquibase`配置

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
liquibase:
  change-log: classpath:config/liquibase/db.changelog.xml
  drop-first: false      #优先drop整库之后，顺序执行 master.xml
  enabled: true          #开启Liquibase
  check-change-log-location: true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 第三步 配置Liquibase部署文件

#### Configuration `db.changelog.xml`

* 在`db.changelog.xml`中，配置初始化变化集`changeSet`、Tag标签、重大升级变化集`changeSet`等。

{% code-tabs %}
{% code-tabs-item title="db.changelog.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.5.xsd">
    <property name="now" value="now()" dbms="h2"/>
    <property name="now" value="now()" dbms="mysql"/>
    <property name="autoIncrement" value="true"/>
    <property name="floatType" value="float4" dbms="postgresql, h2"/>
    <property name="floatType" value="float" dbms="mysql, oracle, mssql"/>

    <!-- 初始化的 变化集-->
    <changeSet id="20180628072451-1" author="suixingpay"> 
        <!-- 详见 【Configuration ChangeSet】 -->
    </changeSet>
    <!-- Tag标签分割，用于rollback到此-->
    <changeSet author="suixingpay" id="tag_1">
        <tagDatabase tag="version_0.1.0" />
    </changeSet>
    <!-- 重大升级 变化集合 etd. -->
</databaseChangeLog>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 如何版本管理

### TAG管理

* **按照文件顺序**在`changeSet`中，打版本标签`<tagDatabase tag="version_0.7.0" />`
* 当`rollback`时，会回滚到指定`tag`之前的样子
* `tag`允许被**字符串命名**，建议与`release number`保持一致

```markup
    <changeSet author="suixingpay" id="1">
    <changeSet author="suixingpay" id="tag_version_0_7_0">
        <tagDatabase tag="version_0.7.0" />
    </changeSet>
    <changeSet author="suixingpay" id="2">
```

### 版本升级

准备好即将升级的`changeset`脚本，见【**制作数据升级包**】包含：

* **手动升级**
  * **Gradle commands 执行此命令可以达到增量滚动升级脚本的目的**

    ```bash
    gradle update
    ```

  * **Maven commands 执行此命令可以达到增量滚动升级脚本的目的**

    ```bash
    mvn update
    ```
* **自动升级**
  * 启动工程后，即可查看目标数据结构变化
    * **注意 此处 关闭 `drop-first` 避免业务销毁数据**

      {% code-tabs %}
      {% code-tabs-item title="application.yml" %}
      ```yaml
      drop-first: false      #优先drop整库之后，顺序执行 master.xml
      ```
      {% endcode-tabs-item %}
      {% endcode-tabs %}
* ~~**指定升级**~~

### 版本回滚

* **Gradle commands 回滚到指定历史版本tag**

  ```bash
  gradle rollback -PliquibaseCommandValue=version_0.5.0
  ```

* **Maven commands 回滚到指定历史版本tag**

  ```bash
  mvn liquibase:rollback -Dliquibase.rollbackTag=version_0.5.0
  ```

## Configuration ChangeSet

### ChangeSet

**ChangeSet 存储每个阶段变化的集合，即变更集；它有以下几个主要属性：**

* **id                      ：唯一标示，不得重复**
* **runOnChange :  当changeSet内容发生改变时，执行此**
* **runAlways      ：总是执行此changeSet**

### **ChangeSet Supported**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>ChangeSet 下的功能</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left"><b>e.g.</b>
      </th>
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
</table>### `Rollback`划重点

在changeSet下有一个非常重要的标签，rollback；它定义了回滚语句



### 我要创建表结构

{% code-tabs %}
{% code-tabs-item title="20180628072446\_added\_entity.xml" %}
```markup
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
        <!-- 此处不写 rollback ，因为它可以被  autoRollback -->
        <!-- 【insert】 【drop】 【modifyDataType】必须写rollback，才会被回滚 -->
    </changeSet>
    
    <!-- Tag标签分割，用于rollback到此-->
    <changeSet author="suixingpay" id="tag_version_0_1_0">
        <tagDatabase tag="version_0.1.0" />
    </changeSet>

```
{% endcode-tabs-item %}
{% endcode-tabs %}



### 我要建索引

{% code-tabs %}
{% code-tabs-item title="20180628072450\_added\_index.xml" %}
```markup
   <!--
        modify mobile of the entity custom   length = 11 + 2 = 13.
    -->
    <changeSet id="201806280724549-1" author="suixingpay">
        <createIndex tableName="custom" indexName="idx_custom_username_mobile_nickname_fullname_registerxtime" unique="false">
            <column name="username"/>
            <column name="mobile"/>
            <column name="nick_name"/>
            <column name="full_name"/>
            <column name="register_time"/>
        </createIndex>
    </changeSet>
    
    <!-- Tag标签分割，用于rollback到此-->
    <changeSet author="suixingpay" id="tag_version_0_2_0">
        <tagDatabase tag="version_0.2.0" />
    </changeSet>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 如何初始化数据

{% code-tabs %}
{% code-tabs-item title="20180628072449\_insert\_data.xml" %}
```markup
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
        <!-- 回滚语句 -->
        <rollback>
            <delete tableName="custom">
                <where>
                    username = 'liyunlong'
                </where>
            </delete>
        </rollback>
    </changeSet>
    
    <!-- Tag标签分割，用于rollback到此-->
    <changeSet author="suixingpay" id="tag_version_0_3_0">
        <tagDatabase tag="version_0.3.0" />
    </changeSet>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 制作数据库升级包

* addColumn 增加列
* modifyDataType 修改列的数据类型或长度等
* dropColumn 删除列定义

```markup
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
        <rollback>
            <modifyDataType tableName="custom" columnName="mobile" newDataType="varchar(11)"/>
        </rollback>
    </changeSet>

    <!--
        drop bbbb of the entity custom
    -->
    <changeSet id="20180628072451-1" author="suixingpay">
        <dropColumn tableName="custom" columnName="bbbb" />
        <rollback>
            <addColumn tableName="custom">
                <column name="bbbb" type="boolean" defaultValueBoolean="true" remarks="测试"></column>
            </addColumn>
        </rollback>
    </changeSet>
    
    <changeSet id="20180628072452-1" author="suixingpay">
        <dropColumn tableName="custom" columnName="active" />
        <rollback>
            <addColumn tableName="custom">
                <column name="active" type="boolean" defaultValueBoolean="true" remarks="激活1"></column>
            </addColumn>
        </rollback>
    </changeSet>
    
    <changeSet author="suixingpay" id="tag_version_0_4_0">
        <tagDatabase tag="version_0.4.0" />
    </changeSet>

```

### 完整的 `db.changelog.xml`

{% file src="../../.gitbook/assets/db.changelog\_now.xml" %}



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

![](../../.gitbook/assets/image%20%2840%29.png)

### 比较 `Diff`

* 比较两个数据库差异

```bash
./liquibase --driver=com.mysql.jdbc.Driver 
                --classpath=lib/ 
                --changeLogFile=config/liquibase/changelog/db.changelog.xml 
                --url="jdbc:mysql://172.16.60.247:3306/suixingpay-starter-demo?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&zeroDateTimeBehavior=convertToNull" 
                --username=fd --password=123456 
                diff 
                --referenceUrl="jdbc:mysql://172.16.60.247:3306/test?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&zeroDateTimeBehavior=convertToNull" 
                --referenceUsername=fd 
                --referencePassword=123456
```

* 结果如下图所示，红框内为变化量：

![](../../.gitbook/assets/image%20%2834%29.png)

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

### Configuration `Gradle build.xml`

```groovy
liquibase {
    activities {
        main {
            changeLogFile 'src/main/resources/config/liquibase/changelog/db.changelog_now.xml'
            username 'fd'
            url 'jdbc:mysql://172.16.60.247:3306/suixingpay-starter-demo?userSSL=false'
            password '123456'
        }
        diff {
            changeLogFile 'src/main/resources/config/liquibase/changelog/*'
            url 'jdbc:mysql://172.16.60.247:3306/test'
            username 'fd'
            password '123456'
        }
    }
    runList = 'main'
}
```

## Q&A

### CI&CD 如果与生产环境不同该如何处理？

1. 准备CI&CD环境配置，准备CI&CD数据库，以便与生产网络不通时，让CI&CD可以继续运行，不至于阻塞；
2. 而到了生产部署时，`liquibase.enabled: true`  只需开启，便会自动执行滚动升级

### TAG版本号如何管理？

1. 版本号是字符串，建议根据`release number`设定，`tagDatabase`是按文件顺序切割版本，所以`rollback`时，注意`changeset`顺序即可。

