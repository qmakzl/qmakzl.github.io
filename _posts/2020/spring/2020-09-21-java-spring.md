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
article_tag3: 
article_section: 
meta_keywords: 
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
08행 : 프로젝트의 식별자를 지정한다. 여기서는 프로젝트 폴더와 동일한 이름인 sp5-chap02를 사용한다.
12~16행 : 프로젝트에서 5.0.2.RELEASE 버전의 spring-context모듈을 사용한다고 설정한다.
21~29행 : 1.8버전을 기준으로 자바 소스를 컴파일하고 결과 클래스를 생성한다. 자바 컴파일러가 소스 코드를 읽을 때 사용할 인코딩은 UTF-8로 설정한다.
{: .notice--info}

## Step 1: 메이븐 의존 설정 
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



**Info:** 지금까지 이전에 공부했던 내용을 정리해보면서 내용을 다듬었습니다. 잘못된 부분은 댓글 남겨주시면 수정하겠습니다. 감사합니다.<br>
{: .notice--info}

**참고자료** <br>
-- 초보 웹 개발자를 위한 스프링5 프로그래밍 입문 (최범균 저) <br>
{: .notice--info}
