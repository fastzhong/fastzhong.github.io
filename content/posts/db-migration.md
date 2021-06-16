+++
title = "数据库变更管理：Flyway & Liquibase"
date = 2021-06-14T11:08:47+08:00
readingTime = true
categories = ["DevOps"]
tags = []
toc = true 
+++

和代码的版本管理一样，对数据库的变更做版本管理

<!--more-->

对于大项目或者多个团队的项目开发，由于数据库表比较多，特别是生产环境中已经部署，数据库的变更管理就比较重要。有一点和代码变更不同的是，数据库的变更不只会被记录下来，而且还会被顺次执行，例如如果插入了一个错误的字段，下次变更得把它删除。

目前能实现数据库变更管理的工具就是 <span class="kwd2">Flyway</span> 和 <span class="kwd2">Liquibase</span>，这里做简单的介绍。

## Flyway

数据库的变更可以用 sql 或者 java 来记录，Flyway 通过下面的步骤实现数据库变更：

1. Flyway 先在数据库中检查自己的元数据表（默认为<span class="kwd">SCHEMA_VERSION</span>）是否存在，如果没有，则创建一个；
2. 检查 classpath 中所有的变更；
3. 对比变更和自己的表，如果变更的版本低于或等于当前版本，不做任何变动；
4. 否则，变更会按从低到高排序，并依次执行；
5. 执行完，在<span class="kwd">SCHEMA_VERSION</span>做相应的记录

Flyway 可以通过 Maven/Gradle，程序（Spring），或者命令行来执行变更操作，下面以 Maven 方式为例。

1. Flyway Maven Plugin

首先在 Maven 的 <span class="kwd">pom.xml</span>中引入 Flyway 的 plugin：

```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
    <version>4.4.0</version>
</dependency>
...
<plugins>
    <plugin>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-maven-plugin</artifactId>
        <version>4.4.0</version>
    </plugin>
</plugins>
```

2. Configuration

然后配置 Flyway：

```xml
<project>
    ...
    <properties>
        <flyway.user>databaseUser</flyway.user>
        <flyway.password>databasePassword</flyway.password>
        <flyway.schemas>schemaName</flyway.schemas>
        ...
    </properties>
    ...
    <plugin>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-maven-plugin</artifactId>
        <version>4.0.3</version>
        <configuration>
            <user>${databaseUser}</user>
            <password>${databasePassword}</password>
            <schemas>
                <schema>${schemaName}</schema>
            </schemas>
            ...
        </configuration>
    </plugin>
    ...
</project>
```

参数当然也可以通过 <span class="kwd">flyway.properties</span> 或者 system properties 传入，其优先顺序：

```md
1. system properties
2. 配置文件，如 flyway.properties
3. maven properties
4. plugin configuration
```

以 H2 数据库为例，Maven 的 <span class="kwd">pom.xml</span>中添加：

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>1.4.200</version>
</dependency>
```

在 src/main/resources/ 里添加以下配置文件 <span class="kwd">flyway.properties</span>：

```txt
flyway.user=databaseUser
flyway.password=databasePassword
flyway.schemas=app-db
flyway.url=jdbc:h2:mem:DATABASE
flyway.locations=src/main/resources/db/migration
```

💬 如果是 Gradle，不需要 Maven plugin：

```txt
implementation('com.h2database:h2')
implementation('org.postgresql:postgresql')
implementation('org.springframework.boot:spring-boot-starter-jdbc')
implementation "org.flywaydb:flyway-core"
```

3. 变更脚本

变更的脚本目录指定在 <span class="kwd">filesystem:src/main/resources/db/migration</span>，变更脚本文件的命名遵循下面的格式：

<span class="kwd">&lt;Prefix&gt;&lt;Version&gt;\_\_&lt;Description&gt;.sql</span>

```md
-   Prefix：默认 V 打头，可以通过 flyway.sqlMigrationPrefix 参数修改
-   Version：版本号
-   Description：文件描述，用双下划线分割
```

例如创建一个变更脚本 <span class="kwd">V1.0\_\_create_company.sql</span>：

```sql
create sequence hibernate_sequence start with 1050;

create table company
(
    id                            uuid         not null,
    name                          varchar(255) not null,
    web_page_address              varchar(255),
    billing_contact_email_address varchar(255),
    primary_contact_email_address varchar(255),
    constraint pk_company
        primary key (id)
);
```

运行：

```bash
$ mvn clean flyway:migrate -Dflyway.configFile=src/main/resources/flyway.properties
```

表 company 就会被创建，下次变更可能为插入数据，例如 <span class="kwd">V2.0\_\_add_company.sql</span>：

```sql
INSERT INTO PUBLIC.COMPANY (ID, NAME, WEB_PAGE_ADDRESS, BILLING_CONTACT_EMAIL_ADDRESS, PRIMARY_CONTACT_EMAIL_ADDRESS)
VALUES ('1a689e52-f35b-4bda-934c-ea4f076bdc2c', 'Blue Fish Software Inc', 'bluefish.io', 'bills@bluefish.io',
        'hi@bluefish.io');
```

再次运行：

```bash
$ mvn clean flyway:migrate -Dflyway.configFile=src/main/resources/db/migration
```

数据就会被写入表中。

## Liquibase

工作方式与 Flyway 非常类似，但是 Liquibase 稍微复杂点，下面也以 Maven 方式来实现 Liquibase 的数据库变更。

1. 和 Flyway 类似，引入 Liquibase plugin：

```xml
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-maven-plugin</artifactId>
    <version>3.4.1</version>
</dependency>
...
<plugins>
    <plugin>
        <groupId>org.liquibase</groupId>
        <artifactId>liquibase-maven-plugin</artifactId>
        <version>3.4.1</version>
        <configuration>
            <propertyFile>src/main/resources/liquibase.properties</propertyFile>
        </configuration>
        <dependencies>
            <dependency>
                <groupId>org.liquibase.ext</groupId>
                <artifactId>liquibase-hibernate4</artifactId>
                <version>3.5</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-beans</artifactId>
                <version>4.1.7.RELEASE</version>
                <version>4.1.7.RELEASE</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.data</groupId>
                <artifactId>spring-data-jpa</artifactId>
                <version>1.7.3.RELEASE</version>
            </dependency>
        </dependencies>
    </plugin>
</plugins>
```

💬 如果是 Gradle，同样简洁很多：

```txt
dependencies{
    liquibaseRuntime "org.liquibase:liquibase-core"
    liquibaseRuntime "mysql:mysql-connector-java"
    liquibaseRuntime "com.h2database:h2"
    liquibaseRuntime "org.liquibase:liquibase-groovy-dsl:2.1."
    liquibaseRuntime "org.liquibase.ext:liquibase-hibernate5:3.6"
    liquibaseRuntime sourceSets.main.output
}
```

2. Configuration

src/main/resources/liquibase.properties:

```txt
changeLogFile=src/main/resources/db/migration/changelog-master.xml
url=jdbc:mysql://localhost:3306/oauth_reddit
username=tutorialuser
password=tutorialmy5ql
driver=com.mysql.jdbc.Driver
referenceUrl=jdbc:h2:mem:oauth_reddit
diffChangeLogFile=src/main/resources/changelog-diff.xml
referenceDriver=org.h2.Driver
referenceUsername=sa
referencePassword=
```

3. 变更脚本

changeLogFile 指明了变更脚本：src/main/resources/db/migration/changelog-master.xml:

```xml
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.6.xsd">

    <property name="now" value="now()" dbms="h2"/>
    <property name="now" value="current_timestamp()" dbms="mysql"/>

    <property name="floatType" value="float4" dbms="h2"/>
    <property name="floatType" value="float" dbms="mysql"/>
    <property name="clobType" value="longvarchar" dbms="h2"/>
    <property name="clobType" value="longtext" dbms="mysql"/>
    <property name="uuidType" value="uuid" dbms="h2"/>
    <property name="uuidType" value="uuid" dbms="mysql"/>

    <changeSet id="00000000000000" author="fastzhong">
        <createSequence sequenceName="sequence_generator" startValue="1050" incrementBy="1"/>
    </changeSet>

    <include file="2021061401_add_entity_Company.xml" relativeToChangelogFile="true"/>
    <include file="2021061402_add_entity_Team.xml" relativeToChangelogFile="true"/>
    <include file="2021061403_add_entity_Project.xml" relativeToChangelogFile="true"/>
</databaseChangeLog>
```

<span class="kwd">Vchangelog-master.xml</span> 是个主文件，具体的变更记录在 3 个文件里，例如 <span class="kwd">V2021061401_add_entity_Company.xml</span>：

```xml
<?xml version="1.0" encoding="utf-8"?>
<databaseChangeLog
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.6.xsd">

    <changeSet id="2021061401-1" author="fastzhong">
        <createTable tableName="company">
            <column name="id" type="bigint" autoIncrement="${autoIncrement}">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="name" type="varchar(255)">
                <constraints nullable="false"/>
            </column>
            <column name="web_page_address" type="varchar(255)">
                <constraints nullable="true"/>
            </column>
            <column name="billing_contact_email_address" type="varchar(255)">
                <constraints nullable="true"/>
            </column>
            <column name="primary_contact_email_address" type="varchar(255)">
                <constraints nullable="true"/>
            </column>
        </createTable>
        <rollback>
            <dropTable tableName="company"/>
        </rollback>
    </changeSet>

    <changeSet id="2021061401-1-data" author="fastzhong" context="faker">
        <loadData
                file="data/company.csv"
                separator=";"
                tableName="company">
            <column name="id" type="numeric"/>
            <column name="name" type="string"/>
            <column name="web_page_address" type="string"/>
            <column name="billing_contact_email_address" type="string"/>
            <column name="primary_contact_email_address" type="string"/>
        </loadData>
    </changeSet>
</databaseChangeLog>
```

每个变更记录在 <span class="kwd">changeSet</span> 里（id 为主键）。Liqudbase 的 XML 如 <span class="kwd">createTable</span>，<span class="kwd">loadData</span>，等，好处是可对任何数据库有效，当然也可以直接上 sql：

```xml
<changeSet id="2021061401-1-data" author="fastzhong" context="faker">
        <sql dbms="mysql">
            CREATE TABLE company (
                id BIGINT NOT NULL AUTO_INCREMENT,
                name CHAR(255) NOT NULL,
                web_page_address CHAR(255) NOT NULL,
                billing_contact_email_address CHAR(255) NOT NULL,
                primary_contact_email_address CHAR(255) NOT NULL,
                PRIMARY KEY (id)
            );
        </sql>
    </changeSet>
```

注意到 changeSet 可以包括 <span class="kwd">rollback</span>，当出错执行的回滚语句或者可以指定 rollback 时执行。

执行变更操作：

```bash
$ mvn liquibase:update
```

在项目开始时，也可以从数据库已有的表结构反向生成 changeLogFile：

```bash
$ mvn liquibase:generateChangeLog
```

还可以比较两个数据库有什么不同，记录在 diffChangeLogFile 里：

```bash
$ mvn liquibase:diff
```

💡 这个还蛮有用的，例如可以通过 Hibernate Entities 在本地数据库自动生成表结构，然后和生产环境中的数据库比较

如果觉得 XML 繁琐的话，Liquibase 也支持 Json，YAML 格式。

## 对比

两者的基本功能其实都差不多：

-   都是 Java 开发的开源数据库变更管理工具
-   支持大部分的数据库
-   和 Maven/Gradle 无缝集成
-   和 Spring 无缝集成
-   非常类似的变更实现方式
-   复杂变更如果 SQL 不能满足的话，都可以用 Java 代码实现

较大区别是 Flyway 的变更以纯 SQL 为脚本，简单直接；Liquibase 比较厚重，当然花样也比较多，包括：

-   可指定不同的 profile
-   具有通用的变更操作支持不同的数据库，如 createTable
-   Liquibase 开源版本支持 diff 模式，而此特性 Flyway 必须用商业版
-   Liquibase 开源版本支持回滚 rollback，而此特性 Flyway 必须用商业版
-   两者指定变更执行顺序的方法不同，Flyway 通过固定的文件名格式来确定顺序，而 Liquibase 就是通过给定文件的顺序来执行，所以开发人员还要遵守好文件命名规则，例如按照日期/时间顺序命名

两者都提供开源和商业版本，具体参见：

-   [Flyway 商业版](https://flywaydb.org/download)

-   [Liquibase 商业版](https://www.liquibase.com/product)

<i class="fas fa-map-marker-alt"></i> 小结：两者差别不大，因功能多，个人“稍微”倾向 Liquibase
