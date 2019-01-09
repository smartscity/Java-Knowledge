# Liquibase



## How to used

### Gradle import

```yaml
compile("org.liquibase:liquibase-core")
```

### Maven import

```markup
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
</dependency>
```

### Configuration  `application.yml`

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
liquibase:
  change-log: classpath:config/liquibase/master.xml
  drop-first: false
  enabled: true
  check-change-log-location: true
```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Configuration master.xml

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

### 我要创建表

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

### 如何升级数据库版本

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

### 如何管理版本

* 
