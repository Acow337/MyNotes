### 依赖

```xml
<properties>
    <org.springframework.version>4.3.19.RELEASE</org.springframework.version>
    <commons-logging.version>1.2</commons-logging.version> 
    <junit.version>4.12</junit.version>
</properties>

<dependencies>
    <!-- Spring核心依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring beans包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring 容器包 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring容器依赖包,将第三方库整合进Spring应用上下文,提供支持 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring aop依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring aspects依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- aspectj依赖 -->
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjrt</artifactId>
        <version>1.9.4</version>
    </dependency>


    <!-- commons-logging依赖 -->
    <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>${commons-logging.version}</version>
    </dependency>

    <!-- Spring jdbc依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!--Spring事物依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>

    <!-- Spring web依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!--Spring webmvc依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>
    <!-- Spring test依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${org.springframework.version}</version>
    </dependency>

    <!-- junit 单元测试依耐 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210315232136.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210315232507.png)





```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!--扫描service包-->
    <context:component-scan base-package="ssm.shiro.service.impl">
    </context:component-scan>
    <!--创建dbcp数据源连接池-->
    <context:property-placeholder location="classpath:jdbc.properties">
    </context:property-placeholder><bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">		<property name="driverClassName" value="${driver}">
    </property><property name="url" value="${url}">
    </property><property name="username" value="${user}">
    </property><property name="password" value="${password}">
    </property></bean>
    <!--创建mybatis的SqlSessionFactory工厂类对象-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource">
    </property><!----><!--<property name="configLocation" value="classpath:mybatis.xml">
	</property>--><!--指定mapper文件的地址 此处可以使用*号同配置，表示加载包下所有的xml结尾的mapper文件--><property name="mapperLocations" value="classpath:/mapper/*.xml"></property></bean><!--配置扫描mybatis的dao接口 ，会为dao接口创建myabtis的dao接口实现类对象，放置到session工厂中--><bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"><property name="basePackage" value="ssm.shiro.dao"></property></bean><!--声明spring 的事物管理器对象--><bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"><property name="dataSource" ref="dataSource"></property></bean><!--声明以注解的方式配置spring 的事物--><tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven><!--引入spring 和shiro集成的主配置文件--><import resource="classpath:spring-shiro.xml"></import></beans>
```





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316001640.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316001817.png)







![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316003710.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316004211.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316005714.png)



### IOC创建对象的方式

（有参构造）

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316013045.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316013216.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316013846.png)



（取别名）

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316021356.png)



import:

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316021917.png)





### DI依赖注入环境

（复杂类型）

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316024959.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316025118.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316140706.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316140939.png)





![]()![QQ截图20210316141810](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316141810.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316150614.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316152407.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316154713.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316154939.png)





### 使用注解实现自动装配

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316155749.png)



（set方法可以省略）

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316160627.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316160923.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316161638.png)



![ ](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316161858.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316162241.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316170023.png)



### Spring注解开发

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316171331.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316171655.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316172945.png)





### 用JavaConfig实现配置

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316192814.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316193027.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316193212.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316194612.png)



### 代理模式

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316204910.png)



### 动态代理

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316224129.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316224733.png)





=========================================================

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316230426.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210316231816.png)



### AOP

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317163055.png)



#### 直接实现spring的api

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317165743.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317165751.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317180533.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317181256.png)





#### 自定义切面

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317182140.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317183233.png)



#### 用注解方式实现AOP



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317185700.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317192220.png)

​	



### Spring整合MyBatis

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317221653.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317222143.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318002447.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318011252.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210317233654.png)



![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318001310.png)

















![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318011944.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318012351.png)

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318012542.png)





### 事务

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318014117.png)





![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210318014803.png)







### 查询书籍

（前端用bootstrap）

![](C:\Users\jinyu\Desktop\图片资源\QQ截图20210321122616.png)