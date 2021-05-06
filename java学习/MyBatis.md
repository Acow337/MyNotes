### 第一个MyBatis

（jdbc连mysql常用url：jdbc:mysql://localhost:3306/mybatistest?useSSL=false&amp;serverTimezone=UTC&amp;autoReconnect=true&amp;useUnicode=true&amp;characterEncoding=UTF-8）



maven项目注入mybatis依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.12</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
            <scope>test</scope>
        </dependency>
</dependencies>
```

![QQ截图20210312190418](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312190418.png)

![QQ截图20210312191216](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312191216.png)

![QQ截图20210312191333](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312191333.png)

![QQ截图20210312192910](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312192910.png)

![QQ截图20210312193014](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312193014.png)

1.写工具类

2.写实体类（含有无参，有参构造，有get&set函数）

3.写接口

4.写Mapper.xml  (实现接口)

5.在核心配置文件注册Mapper.xml

6.测试





（工具类）

```java
package com.acow.utils;


import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;
    
    static{
        try {
            String resources="mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resources);
            sqlSessionFactory=new SqlSessionFactoryBuilder().build(inputStream);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
    
}
```



（核心配置文件）

```html
<configuration>

<environments default="development">
<environment id="development">
<transactionManager type="JDBC"/>
<dataSource type="POOLED">
    <property name="driver" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mybatistest?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</dataSource>
</environment>
</environments>

</configuration>
```

```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<mapper namespace="com.acow.dao.UserMapper">

    <select id="getAllUser" resultType="com.acow.pojo.User">
        select * from mybatis.user
    </select>

</mapper>
```



```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">


<configuration>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatistest?useSSL=false&amp;serverTimezone=UTC&amp;autoReconnect=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="com/acow/dao/UserMapper.xml"/>
    </mappers>

</configuration>
```





(注：增删改需要提交事务)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312231008.png)







MAP：

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312233152.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210312233217.png)





多环境：



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313003250.png)





引入外部资源环境：

![image-20210313005141717](C:\Users\jinyu\AppData\Roaming\Typora\typora-user-images\image-20210313005141717.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313005747.png)



 

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313020317.png)







resultmap:

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313092955.png)







<settings>标签



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313095736.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313100340.png)





### 日志

LOG4J：

```html
       <!--日志 start-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.25</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.25</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.25</version>
            <scope>test</scope>
        </dependency>
        <!--日志end-->
```





```
 \### set log levels ###

log4j.rootLogger = debug , stdout , D , E

\### 输出到控制台 ###

log4j.appender.stdout = org.apache.log4j.ConsoleAppender

log4j.appender.stdout.Target = System.out

log4j.appender.stdout.layout = org.apache.log4j.PatternLayout

log4j.appender.stdout.layout.ConversionPattern = %d{ABSOLUTE} %5p %c{ 1 }:%L - %m%n

\### 输出到日志文件 ###

log4j.appender.D = org.apache.log4j.DailyRollingFileAppender

log4j.appender.D.File = logs/log.log

log4j.appender.D.Append = true

log4j.appender.D.Threshold = DEBUG ## 输出DEBUG级别以上的日志

log4j.appender.D.layout = org.apache.log4j.PatternLayout

log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n

\### 保存异常信息到单独文件 ###

log4j.appender.D = org.apache.log4j.DailyRollingFileAppender

log4j.appender.D.File = logs/error.log ## 异常日志文件名

log4j.appender.D.Append = true

log4j.appender.D.Threshold = ERROR ## 只输出ERROR级别以上的日志!!!

log4j.appender.D.layout = org.apache.log4j.PatternLayout

log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n


```





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313104504.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313104609.png)







### 分页查询

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313140454.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313140703.png)







注解写sql: (本质：反射机制实现)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313144950.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313145115.png)







通过注解写增删改查

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313191414.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313191831.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313192128.png)

### LOMBOK



```html
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.18</version>
    <scope>provided</scope>
</dependency>
```



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313193240.png)





复杂查询环境搭建：

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313203703.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210313204015.png)





### 多对一处理



按照查询嵌套处理：



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314140640.png)



按照结果嵌套处理：

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314143930.png)



### 一对多处理

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314152633.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314154343.png)    







获得随机ID

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314162053.png)





### 动态SQL（if choose ...）



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314173449.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314175304.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314181450.png)





![QQ截图20210314191004](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314191004.png)



foreach：



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314200435.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210314201354.png)