# mybatis学习

## 1.创建maven工程

## 2.导入依赖

```xml
<!--    导入依赖-->
<dependencies>
    <!--    mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.48</version>
    </dependency>
    <!--            mybatis-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.4</version>
    </dependency>
    <!--        junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

## 3.配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

## 4.MybatisUtils文件

```java
package com.mykang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {

        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = null;
            inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    public static SqlSession getSqlSession(){
         return sqlSessionFactory.openSession();

    }
}
```

## 5.注意：

error：```org.apache.ibatis.binding.BindingException: Type interface com.mykang.dao.UserDao is not known to the MapperRegistry.```

配置文件添加如下：

```xml
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
```

error:

mapper配置文件不在resources目录下

在pom.xml添加如下：

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

## 6.测试：

```java
@Test
public void test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserDao mapper = sqlSession.getMapper(UserDao.class);
    List<user> userList = mapper.getUserList();
    for (user user : userList) {
        System.out.println(user);
    }
    sqlSession.close();

}
```

### 可以遇到的问题：

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. maven导出资源

## 7.增、删、改

​	**必须提交事务:**

```java
sqlSession.commit();
```



## 8.sql语法

```xml
<!--命名空间，绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.mykang.dao.UserDao">
<!--        查-->
    <select id="getUserList" resultType="com.mykang.pojo.user">
        select * from mybatis.user
    </select>
<!--    查-->
    <select id="getUserById" resultType="com.mykang.pojo.user" parameterType="int">
        select * from mybatis.user where id = #{id};
    </select>
<!-- 增-->
    <insert id="addUser" parameterType="com.mykang.pojo.user">
        insert into mybatis.user(id,name,pwd) values (#{id},#{name},#{pwd});
    </insert>
<!--改-->
    <update id="updateuser" parameterType="com.mykang.pojo.user">
        update mybatis.user set name = #{name},pwd=#{pwd} where id=#{id};
    </update>
<!--删-->
    <delete id="deleteuser" parameterType="int" >
        delete from mybatis.user where id=#{id};
    </delete>
</mapper>
```

## 9.万能的Map

**实体类，或数据库中表、字段或参数过多，可以考虑使用map!**

```java
int updateuser2(Map<String, Object> map);
```

```xml
<!--改,使用Map-->
<update id="updateuser2" parameterType="map">
    update user set pwd=#{password} where id=#{userid};
</update>
```

```java
@Test
public void updateuser2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserDao mapper = sqlSession.getMapper(UserDao.class);

    Map<String, Object> map = new HashMap<String, Object>();

    map.put("userid",2);
    map.put("password","123123");

    mapper.updateuser2(map);

    sqlSession.commit();
    sqlSession.close();
}
```

Map传递参数，【parameterType="map"】

对象传递参数，【parameterType="Object"】

基本类型，可以直接在sql中取。【parameterType可以省略】

多个参数Map,或**注解**

## 10.IDEA连接MySQL数据库问题

![](https://img2020.cnblogs.com/blog/1916409/202007/1916409-20200707130154288-320204431.png)

## 11.模糊查询

**java代码执行传递通配符：**

```java
List<user> userList = mapper.getUserLike("%y%");
```

**sql拼接使用通配符：**

```xml
<select id="getUserLike" resultType="com.mykang.pojo.user">
    select * from mybatis.user where name like "%"#{value}"%";
</select>
```

## 12.配置解析

### 	1.核心配置文件

- ​		mybatis-config.xml

### 	2.properties配置文件

```properties
driver =com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=123456
```

**引入外部配置文件：**

```xml
<properties resource="db.properties"/>
```

### 3.别名（typeAliases）

1.类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写

```xml
<typeAliases>
    <typeAlias type="com.mykang.pojo.user" alias="user"/>
</typeAliases>
```

2.也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

```xml
<typeAliases>
  <package name="com.mykang.pojo"/>
</typeAliases>
```

3.若有注解，则别名为其注解值

```java
@Alias("author")
public class Author {
    ...
}
```

## 13.解决属性名和字段名不一致的问题

### 1.起别名

```sql
select id,username,pwd as password from mybatis.user where id = #{id};
```

### 2.resultMap

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。

- ```xml
  <resultMap id="userResultMap" type="User">
    <id property="id" column="user_id" />
    <result property="username" column="user_name"/>
    <result property="password" column="hashed_password"/>
  </resultMap>
  ```

## 14.日志

- SLF4J 
- LOG4J  【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING   【掌握】
- NO_LOGGING

### 1.STDOUT_LOGGING标准日志输出

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

### 2.log4j

#### 导依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

#### 配置log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

#### 配置：

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

#### 使用

```java
import org.apache.log4j.Logger;
```

```java
static Logger logger = Logger.getLogger(UserDaoTest.class);
```

```java
logger.info("info:进入了testLog4j");
logger.debug("debug:进入了testLog4j");
logger.error("error:进入了testLog4j");
```

