# spring-setting
**스프링 기본 셋팅**

**1. jdk**
  * bin 폴더 rombok.jar 넣고 
  * cmd 켜서 cd 맞추고 java -jar ./lombok.jar 실행


**2. Servers에 Tomcat 서버추가**
  * server.xml -> connector tag에 URIEncoding="UTF-8" 추가

**3. sts에 data source explorer 추가**
  *  help -> install new software
  * http://download.eclipse.org/releases/neon/ 
  *  data source explorer 체크후 설치

**4. pom.xml**
* java-version(쓰고있는 jdk버젼으로) and springframework-version(모르겠으면 최신)  수정
https://mvnrepository.com/artifact/org.springframework/spring-context


<h1><properties>
	<java-version>11</java-version>
	<org.springframework-version>4.2.4.RELEASE</org.springframework-version>
	<org.aspectj-version>1.6.10</org.aspectj-version>
	<org.slf4j-version>1.6.6</org.slf4j-version>
</properties><h1>



* dependency 
lombok, spring jdbc, jdbc(mariadb), spring-mybtis, mybatis 추가

-------------------추가한 부분-------------------
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.18.18</version>
	<scope>provided</scope>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-jdbc</artifactId>
	<version>4.2.6.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client -->
<dependency>
	<groupId>org.mariadb.jdbc</groupId>
	<artifactId>mariadb-java-client</artifactId>
	<version>2.7.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis-spring</artifactId>
	<version>1.3.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis</artifactId>
	<version>3.4.1</version>
</dependency>
----------------------------------------------------

* plugin 수정
1.6 -> 1.8로

------수정 부분------
<source>1.8</source>
<target>1.8</target>
---------------------

* Project Facets 자신 버젼에 맞게 수정

**5. mybatis 연결**
* root-context.xml
하단에 name space(없으면 마켓플레이스에서 spring tool 설치후 xml open with) 
-> context, jdbc, mybatis-spring 체크
-> value(url, username,password 맞게 기입

-------------------------------추가 부분---------------------------
<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
	<property name="driverClass" value="org.mariadb.jdbc.Driver"></property>
	<property name="url" value="jdbc:mariadb://localhost:3306/comstudy21"></property>
	<property name="username" value="root"></property>
	<property name="password" value="12345"></property>
</bean>
	
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource"></property>
	<property name="configLocation" value="classpath:sql-map-config.xml"></property>
</bean>
	
<bean id="mybatis" class="org.mybatis.spring.SqlSessionTemplate">
	<constructor-arg ref="sqlSessionFactory"></constructor-arg>
</bean>
------------------------------------------------------------------------


**6. mapping 생성**
 * src/main/resources -> mappings 폴더 생성 -> (user-mapping) xml 추가
 * <mapper namespace = "자신이 하고싶은 명칭">

-------------------------------추가 부분----------------------------------
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="UserDao">
</mapper>
------------------------------------------------------------------------------

 * src/main/resources -> sql-map-config.xml  

-------------------------------추가 부분-----------------------------------
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration> 
------------------------------------------------------------------------------


**7. run하고 테스트**


**8. 객체 만드는 xml파일 추가**


* src/main/resources -> pring Bean Configration File생성 ->
applicationContext.xml -> beans, context, p 체크( 자기 버젼에 맞게끔)

-------------------------------추가 부분-----------------------------------
<beans:beans
	xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<annotation-driven />
	<context:component-scan
		base-package="org.comstudy21.myweb" />

</beans:beans>
-----------------------------------------------------------------------------------------

