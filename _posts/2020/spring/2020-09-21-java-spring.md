---
title: SPRING( SPRING 공부 )
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA SPRING
toc: true
toc_sticky: true
toc_label: 목차
description: JAVA를 공부하는 데 있어 Spring을 공부하여 기초를 잡는 게시물
article_tag1: 메이븐 프로젝트 생성
article_tag2: 메이븐 의존 설정
article_tag3: 그레이들 프로젝트 생성
article_section: Spring 공부하기
meta_keywords: pom, gradle, spring, 
last_modified_at: '2020-09-21 23:00:00 +0800'
---

제임스 고슬링(James Gosling)이 개발한 JAVA 언어를 깊이 이해하기 위해서는 Spring의 공부가 필요하게 생각하여 공부하는 내용을 정리하겠습니다.

## Step 1: 메이븐 프로젝트 생성
메이븐 프로젝트 폴더를 생성 후 
```java
//C:\spring5fs\sp5-chap02\pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>sp5</groupId>
	<artifactId>sp5-chap02</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.0.2.RELEASE</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.7.0</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>utf-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>
```
**pom.xml의 주요코드** <br>
08행 : 프로젝트의 식별자를 지정한다. 여기서는 프로젝트 폴더와 동일한 이름인 sp5-chap02를 사용한다.<br>
12~16행 : 프로젝트에서 5.0.2.RELEASE 버전의 spring-context모듈을 사용한다고 설정한다.<br>
21~29행 : 1.8버전을 기준으로 자바 소스를 컴파일하고 결과 클래스를 생성한다. 자바 컴파일러가 소스 코드를 읽을 때 사용할 인코딩은 UTF-8로 설정한다.<br>
{: .notice--info}

## Step 2: 메이븐 의존 설정 
```java
<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.0.2.RELEASE</version>
		</dependency>
	</dependencies>
```
메이븐은 한 개의 모듈을 아티팩트라는 단위로 관리한다.<br>
spring-context라는 식별자를 가진 5.0.2.RELEASE 버전의 아티팩트에 대한 의존을 추가한 것이다.<br>
의존을 추가한다는 것은 일반적인 자바 어플리케이션에서 클래스 패스에 spring-context 모듈을 추가한다는 것을 뜻한다.<br>
위 설정은 메이븐 프로젝트의 소스 코드를 컴파일하고 실행할 때 사용할 클래스 패스에 spring-context-5.0.2.RELEASE.jar파일을 추가한다는 것을 의미한다.<br><br>

## Step 3: 그레이들 프로젝트 생성
```java 
//C:\spring5fs\sp5-chap02\build.gradle
['apply plugin: 'java'

sourceCompatibility = 1.8
targetCompatibility = 1.8
compileJava.options.encoding = "UTF-8"

repositories {
    mavenCentral()
}

dependencies {
    compile 'org.springframework:spring-context:5.0.2.RELEASE'
}

task wrapper(type: Wrapper) {
    gradleVersion = '4.4'
}
``` 
**build.gradle의 주요코드** <br>
01행 : 그레이들 java 플러그인을 적용한다.<br>
03~04행 : 소스와 컴파일 결과를 1.8버전에 맞춘다.<br>
05행 : 소스코드 인코딩으로 UTF-8을 사용한다.<br>
07~09행 : 의존 모듈을 메이븐 중앙 리포지토리에서 다운로드한다.<br>
12행 : spring-context 모듈에 대한 의존을 설정한다.<br>
15~17행 : 그래이들 래퍼 설정이다. 소스를 공유할 때 그레이들 설치 없이 그레이들 명령어를 실행할 수 있는 래퍼를 생성해준다.<br>
{: .notice--info}

## Step 4: 예제 코드 작성
```java 
//sp5-chap02/src/main/java/chap02/Greeter.java
package chap02;

public class Greeter {
	private String format;

	public String greet(String guest) {
		return String.format(format, guest);
	}

	public void setFormat(String format) {
		this.format = format;
	}

}
```

```java
//sp5-chap02/src/main/java/chap02/AppContext.java
package chap02;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppContext {

	@Bean
	public Greeter greeter() {
		Greeter g = new Greeter();
		g.setFormat("%s, 안녕하세요!");
		return g;
	}

}
```
**AppContext.java의 주요코드** <br>
06행 : @Configuration 애노테이션은 해당 클래스를 스프링 설정 클래스로 지정한다.<br>
{: .notice--info}

```java
//sp5-chap02/src/main/java/chap02/Main.java
package chap02;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppContext.class);
		Greeter g1 = ctx.getBean("greeter", Greeter.class);
		Greeter g2 = ctx.getBean("greeter", Greeter.class);
		System.out.println("(g1 == g2) = " + (g1 == g2));
		ctx.close();
	}
}
```
**build.gradle의 주요코드** <br>
03행 : AnnotationConfigApplicationContext 클래스는 자바 설정에서 정보를 읽어와 빈 객체를 생성하고 관리한다.<br>
08~09행 : AnnotationConfigApplicationContext 객체를 생성할때 앞서 작성한 AppContext클래스를 생성자 파라미터로 전달한다.<br>
10행 : getBean() 메서드는 greeter()메서드가 생성한 Greeter 객체를 리턴한다.<br>
{: .notice--info}

## Step 5: 싱글톤(Singleton) 객체
```java
//sp5-chap02/src/main/java/chap02/Main2.java
package chap02;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main2 {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppContext.class);
		Greeter g = ctx.getBean("greeter", Greeter.class);
		String msg = g.greet("스프링");
		System.out.println(msg);
		ctx.close();
	}
}
```
스프링은 기본적으로 한 개의 @Bean 애노테이션에 대해 한 개의 빈 객체를 생성한다. 따라서 다음과 같은 설정을 사용하면 "greeter"에 해당하는 객체 한 개와 "greeter1"에 해당하는 객체 한 개, 이렇게 두개의 빈 객체가 생성된다.<br>
싱글톤 범위 외에 프로토타입 범위도 존재한다. 이에 관한 내용은 다음에 살펴본다.<br>

**Info:** 지금까지 이전에 공부했던 내용을 정리해보면서 내용을 다듬었습니다. 잘못된 부분은 댓글 남겨주시면 수정하겠습니다. 감사합니다.<br>
{: .notice--info}

**참고자료** <br>
-- 초보 웹 개발자를 위한 스프링5 프로그래밍 입문 (최범균 저) <br>
{: .notice--info}
