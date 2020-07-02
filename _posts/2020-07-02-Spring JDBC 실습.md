---
layout: post
title: "Spring JDBC"
tags: [Spring JDBC]
comments: true
---
 
해당 Post는 실습을 통해 Spring JDBC을 정리한 파일입니다.

---

<a href="https://junghyun100.github.io/Spring-JDBC/">지난 포스트</a>

## DTO란?

* DTO란 Data Transfer Object의 약자입니다.
* 계층간 데이터 교환을 위한 자바빈즈입니다.
* 여기서의 계층이란 컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계층을 의미합니다.
* 일반적으로 DTO는 로직을 가지고 있지 않고, 순수한 데이터 객체입니다.
* 필드와 getter, setter를 가진다. 추가적으로 toString(), equals(), hashCode()등의 Object 메소드를 오버라이딩 할 수 있습니다.

### DTO의 예

```java
public class ActorDTO {
    private Long id;
    private String firstName;
    private String lastName;
    public String getFirstName() {
        return this.firstName;
    }
    public String getLastName() {
        return this.lastName;
    }
    public Long getId() {
        return this.id;
    }
    // ......
}
```

## DAO란?

* DAO란 Data Access Object의 약자로 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체입니다.
* 보통 데이터베이스를 조작하는 기능을 전담하는 목적으로 만들어집니다.


## ConnectionPool 이란?

* DB연결은 비용이 많이 듭니다.
* 커넥션 풀은 미리 커넥션을 여러 개 맺어 둡니다.
* 커넥션이 필요하면 커넥션 풀에게 빌려서 사용한 후 반납합니다.
* 커넥션을 반납하지 않으면 어떻게 될까요?
> 그렇지 않으면 ConnectionPool에서 사용 가능한 Connection이 없어서 프로그램이 늦어지거나 심할 경우에는 장애를 발생시킬 수도 있습니다.

<img src="https://cphinf.pstatic.net/mooc/20180208_14/15180684447693OANG_JPEG/3_8_2_ConnectionPool.jpg">

## DataSource란?

* DataSource는 커넥션 풀을 관리하는 목적으로 사용되는 객체입니다.
* DataSource를 이용해 커넥션을 얻어오고 반납하는 등의 작업을 수행합니다.
 
## Spring JDBC를 이용한 DAO작성 실습

<img src ="https://cphinf.pstatic.net/mooc/20180208_103/1518068520531pRbvK_PNG/3_8_2_Spring_JDBC__DAO_.png">

### 실습코드

### pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>kr.or.connect</groupId>
	<artifactId>daoexam</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>daoexam</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<spring.version>4.3.5.RELEASE</spring.version>
	</properties>

	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- basic data source -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-dbcp2</artifactId>
			<version>2.1.1</version>
		</dependency>


		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.45</version>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.6.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>
```
### ApplicationConfig.java
```java
package kr.or.connect.daoexam.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@Import({DBConfig.class})
public class ApplicationConfig {

}
```
### DBConfig.java
```java
package kr.or.connect.daoexam.config;

import javax.sql.DataSource;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@EnableTransactionManagement
public class DBConfig {
	private String driverClassName = "com.mysql.jdbc.Driver";
    private String url = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&characterEncoding=utf8";

    private String username = "connectuser";
    private String password = "connect123!@#";

    @Bean
    public DataSource dataSource() {
    	BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;

    }
}
```
### DataSourceTest.java
```java
package kr.or.connect.daoexam.main;

import java.sql.Connection;

import javax.sql.DataSource;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import kr.or.connect.daoexam.config.ApplicationConfig;

public class DataSourceTest {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		DataSource ds = ac.getBean(DataSource.class);
		Connection conn = null;
		try {
			conn = ds.getConnection();
			if(conn != null)
				System.out.println("접속 성공^^");
		}catch (Exception e) {
			e.printStackTrace();
		}finally {
			if(conn != null) {
				try {
					conn.close();
				}catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
	}

}
```
---
