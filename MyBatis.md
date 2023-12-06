# MyBatis 自动生成Dao和XML的用法（插件式）

### 配置文件

- 位置：src/main/resources/mybatis-generator-config.xml

- 注意：重复生成xml文件是追加形式，而不是覆盖

### 相关Blog

- [MyBatis学习笔记](https://zjxkenshine.github.io/2018/03/29/MyBatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%EF%BC%88%E4%BA%94%EF%BC%89%EF%BC%9A%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90%E5%99%A8/)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application.yml"/>

    <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
        property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
        javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
    -->
    <context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat">

        <!-- 这个插件给生成的Java模型对象增加了equals和hashCode方法 -->
        <!--<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>-->

        <!-- 注释 -->
        <commentGenerator>
            <!-- 是否不生成注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 不希望生成的注释中包含时间戳 -->
            <!--<property name="suppressDate" value="true"/>-->
            <!-- 添加 db 表中字段的注释，只有suppressAllComments为false时才生效-->
            <!--<property name="addRemarkComments" value="true"/>-->
        </commentGenerator>


        <!-- jdbc连接 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mall?useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"
                        userId="root"
                        password="123456789">
            <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!--是否使用bigDecimal，默认false。
                false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
                true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
            <property name="forceBigDecimals" value="true"/>
            <!--默认false
                false，将所有 JDBC 的时间类型解析为 java.util.Date
                true，将 JDBC 的时间类型按如下规则解析
                    DATE	                -> java.time.LocalDate
                    TIME	                -> java.time.LocalTime
                    TIMESTAMP               -> java.time.LocalDateTime
                    TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
                    TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
                -->
            <!--<property name="useJSR310Types" value="false"/>-->
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="com.example.demoweb.entity" targetProject="src/main/java">
            <!-- 是否让 schema 作为包的后缀，默认为false -->
            <!--<property name="enableSubPackages" value="false"/>-->
            <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成Mapper.xml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <!--<property name="enableSubPackages" value="false"/>-->
        </sqlMapGenerator>

        <!-- 生成 XxxMapper.java 接口-->
        <javaClientGenerator targetPackage="com.example.demoweb.dao" targetProject="src/main/java" type="XMLMAPPER">
            <!--<property name="enableSubPackages" value="false"/>-->
        </javaClientGenerator>


        <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
             tableName为对应的数据库表名
             domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
             enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
             -->
        <table schema="mall" tableName="users" domainObjectName="User"
               enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false" selectByExampleQueryId="false">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
    </context>
</generatorConfiguration>
```

### pom.xml 关于插件的位置

```xml
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <configuration>
        <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
        <overwrite>true</overwrite>
        <verbose>true</verbose>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>
    </dependencies>
</plugin>
```
