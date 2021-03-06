---
title: SPRING( SPRING 공부 ) 2 Spring DI
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
article_tag1: 의존이란?
article_tag2: DI를 통한 의존 처리 
article_tag3: DI와 의존 객체 변경의 유연함
article_section: Spring 공부하기
meta_keywords: DI, Dependency,  
last_modified_at: '2020-09-21 23:00:00 +0800'
---

제임스 고슬링(James Gosling)이 개발한 JAVA 언어를 깊이 이해하기 위해서는 Spring의 공부가 필요하게 생각하여 공부하는 내용을 정리하겠습니다.

## Step 1: 의존이란?
<br>
DI는 'Dependency Injection'의 약자로 우리말로는 '의존 주입'이라고 번역한다. <br>
이 단어의 의미를 이해하려면 먼저 '의존(dependency)'이 뭔지 알아야 한다. <br>
여기서 말하는 의존은 객체간의 의존을 의미한다. <br>
이해를 돕기위해 회원가입을 처리하는 기능을 구현한 다음의 코드를 보자. <br>
{: .notice--info}

```java
import java.time.LocalDateTime;

public class MemberRegisterService {
	private MemberDao memberDao = new MemberDao();

	public void regist(RegisterRequest req) {
		//이메일로 회원 데이터(Member) 조회
		Member member = memberDao.selectByEmail(req.getEmail());
		if (member != null) {
			//같은 이메일을 가진 회원이 이미 존재하면 익셉션 발생
			throw new DuplicateMemberException("dup email " + req.getEmail());
		}
		//같은 이메일을 가진 회원이 존재하지 않으면 DB에 삽입
		Member newMember = new Member(
				req.getEmail(), req.getPassword(), req.getName(), 
				LocalDateTime.now());
		memberDao.insert(newMember);
	}
}
```
**의존** <br>
의존은 변경에 의해 영향을 받는 관계를 의미한다.<br> 
예를 들어 MemberDao의 insert() 메서드의 이름을 insertMember()로 변경하면 이 메서드를 사용하는 
MemberRegisterService 클래스의 소스코드도 함께 변경된다. <br>
이렇게 변경에 따른 영향이 전파되는 관계를 '의존'한다고 표현한다. <br>
{: .notice--info}

## Step 2: DI를 통한 의존 처리 
DI(Dependency Injection, 의존 주입)는 의존하는 객체를 직접 생성하는 대신 의존 객체를 전달받는 방식을 사용한다. 
예를 들어 앞서 의존 객체를 직접 생성한 MemberRegisterService 클래스에 DI방식을 적용하면 아래 코드처럼 구현할 수 있다. 
```java
//sp5-chap03/src/main/java/spring/MemberRegisterService.java
package spring;

import java.time.LocalDateTime;

public class MemberRegisterService {
	private MemberDao memberDao;

	public MemberRegisterService(MemberDao memberDao) {
		this.memberDao = memberDao;
	}

	public Long regist(RegisterRequest req) {
		//이메일로 회원 데이터(Member) 조회
		Member member = memberDao.selectByEmail(req.getEmail());
		if (member != null) {
			//같은 이메일을 가진 회원이 이미 존재하면 익셉션 발생
			throw new DuplicateMemberException("dup email " + req.getEmail());
		}
		//같은 이메일을 가진 회원이 존재하지 않으면 DB에 삽입
		Member newMember = new Member(
				req.getEmail(), req.getPassword(), req.getName(), 
				LocalDateTime.now());
		memberDao.insert(newMember);
		return newMember.getId();
	}
}
```
직접 의존 객체를 생성했던 코드와 달리 바뀐 코드는 의존 객체를 직접 생성하지 않는다. <br>
대신 08~10행과 같이 생성자를 통해서 의존 객체를 전달받는다. <br>
즉 생성자를 통해 MemberRegisterService가 의존(Dependency)하고 있는
MemberDao 객체를 주입(Injection) 받은 것이다. <br>
의존 객체를 직접 구하지 않고 생성자를 통해서 전달받기 때문에 이 코드는 DI(의존 주입) 패턴을 따르고 있다.<br>

## Step 3: DI와 의존 객체 변경의 유연함
 
의존 객체를 직접 생성하는 방식은 필드나 생성자에서 new 연산자를 이용해서 객체를 생성한다. <br>
회원 등록 기능을 제공하는 MemberRegisterService 클래스에서 다음 코드처럼 의존 객체를 직접 생성할 수 있다.<br>
{: .notice--info}
```java
public class MemberRegisterService {
	public MemberDao memberDao = new MemberDao();
	...
}
```
회원의 암호 변경 기능을 제공하는 ChangePasswordService 클래스도 다음과 같이 의존 객체를 직접 생성한다고 하자.<br>
```java
public class ChangePasswordService{
	private MemberDao memberDao = new MemberDao();
	...
}
```
MemberDao 클래스는 회원 데이터를 데이터베이스에 저장한다고 가정해보자. <br>
이 상태에서 회원 데이터의 빠른 조회를 위해 캐시를 적용해야 하는 상황이 발생했다. <br>
그래서 MemberDao 클래스를 상속받은 CachedMemberDao 클래스를 만들었다.<br>
```java
public class CachedMemberDao extends MemberDao{
	...
}
```

**캐시** <br>
캐시(cache)는 데이터 값을 복사해 놓는 임시 장소를 가리킨다. <br>
보통 조회 속도 향상을 위해 캐시를 사용한다. <br>
예를 들어 데이터베이스에서 데이터를 조회하는 경우를 생각해보자. <br>
데이터베이스에서 데이터를 읽어오는데 10밀리초(1밀리 초는 0.001초)가 걸린다면 메모리에 있는 데이터를 접근할 때에는 1밀리초도 안 걸릴 것이다. <br>
DB에 있는 데이터 중 자주 조회하는 데이터를 메모리를 이용하는 캐시에 보관하면 조회 속도를 향상시킬 수 있다. <br>
{: .notice--info}


## Step 4: 예제 프로젝트 만들기
### 회원 데이터 관련 클래스
 - sp5-chap03 프로젝트 폴더를 생성한다.
 - 프로젝트의 하위 폴더로 src/main/java를 생성한다.
 - sp5-chap03 폴더에 pom.xml파일을 작성한다.
```java 
//sp5-chap03/pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>sp5</groupId>
	<artifactId>sp5-chap03</artifactId>
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
- 회원 데이터 관련 클래스 
 - Member
 - WrongldPasswordException
 - MemberDao
- 회원 가입 처리 관련 클래스 
 - DuplicateMemberException
 - RegisterRequest
 - MemberRegisterService
- 암호 변경 관련 클래스
 - MemberNotFoundException
 - ChangePasswordService
 
```java
//sp5-chap03/src/main/java/spring/Member.java
package spring;

import java.time.LocalDateTime;

public class Member {

	private Long id;
	private String email;
	private String password;
	private String name;
	private LocalDateTime registerDateTime;

	public Member(String email, String password, 
			String name, LocalDateTime regDateTime) {
		this.email = email;
		this.password = password;
		this.name = name;
		this.registerDateTime = regDateTime;
	}

	void setId(Long id) {
		this.id = id;
	}

	public Long getId() {
		return id;
	}

	public String getEmail() {
		return email;
	}

	public String getPassword() {
		return password;
	}

	public String getName() {
		return name;
	}

	public LocalDateTime getRegisterDateTime() {
		return registerDateTime;
	}

	public void changePassword(String oldPassword, String newPassword) {
		if (!password.equals(oldPassword))
			throw new WrongIdPasswordException();
		this.password = newPassword;
	}

}
```
changePassword() 메서드는 암호 변경 기능을 구현했다. <br>
이 메서드는 oldPassword와 newPassword의 두 파라미터를 전달받는다.<br>
oldPassword가 현재 암호인 password 필드와 값이 다르면 WrongIdPasswordException을 발생시키고, 값이 같으면 password 필드를 newPassword로 변경한다.
```java
//sp5-chap03/src/main/java/spring/WrongIdPasswordException.java
package spring;

public class WrongIdPasswordException extends RuntimeException{
}
```
다음으로 작성할 코드는 자바의 Map을 이용해서 구현한다.

```java
sp5-chap03/src/main/java/spring/MemberDao.java
package spring;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

public class MemberDao {

	private static long nextId = 0;

	private Map<String, Member> map = new HashMap<>();

	public Member selectByEmail(String email) {
		return map.get(email);
	}

	public void insert(Member member) {
		member.setId(++nextId);
		map.put(member.getEmail(), member);
	}

	public void update(Member member) {
		map.put(member.getEmail(), member);
	}

	public Collection<Member> selectAll() {
		return map.values();
	}
}
```

### 회원 가입 처리 관련 클래스
```java
//sp5-chap03/src/main/java/spring/DuplicateMemberException.java
package spring;

public class DuplicateMemberException extends RuntimeException {

	public DuplicateMemberException(String message) {
		super(message);
	}

}
```

```java
//sp5-chap03/src/main/java/spring/RegisterRequest.java
package spring;

public class RegisterRequest {

	private String email;
	private String password;
	private String confirmPassword;
	private String name;

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getConfirmPassword() {
		return confirmPassword;
	}

	public void setConfirmPassword(String confirmPassword) {
		this.confirmPassword = confirmPassword;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public boolean isPasswordEqualToConfirmPassword() {
		return password.equals(confirmPassword);
	}
}
```

### 암호 변경 관련 클래스

```java
//sp5-chap03/src/main/java/spring/ChangePasswordService.java
package spring;

public class ChangePasswordService {

	private MemberDao memberDao;

	public void changePassword(String email, String oldPwd, String newPwd) {
		Member member = memberDao.selectByEmail(email);
		if (member == null)
			throw new MemberNotFoundException();

		member.changePassword(oldPwd, newPwd);

		memberDao.update(member);
	}

	public void setMemberDao(MemberDao memberDao) {
		this.memberDao = memberDao;
	}

}
```

```java
//sp5-chap03/src/main/java/spring/MemberNotFoundException.java
package spring;

public class MemberNotFoundException extends RuntimeException {
}
```

### 객체 조립기

```java
//sp5-chap03/src/main/java/assembler/Assembler.java
package assembler;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberRegisterService;

public class Assembler {

	private MemberDao memberDao;
	private MemberRegisterService regSvc;
	private ChangePasswordService pwdSvc;

	public Assembler() {
		memberDao = new MemberDao();
		regSvc = new MemberRegisterService(memberDao);
		pwdSvc = new ChangePasswordService();
		pwdSvc.setMemberDao(memberDao);
	}

	public MemberDao getMemberDao() {
		return memberDao;
	}

	public MemberRegisterService getMemberRegisterService() {
		return regSvc;
	}

	public ChangePasswordService getChangePasswordService() {
		return pwdSvc;
	}

}
```

### 조립기 사용 예제 
```java
//sp5-chap03/src/main/java/main/MainForAssembler.java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import assembler.Assembler;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.WrongIdPasswordException;

public class MainForAssembler {

	public static void main(String[] args) throws IOException {
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			}
			printHelp();
		}
	}

	private static Assembler assembler = new Assembler();

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
		MemberRegisterService regSvc = assembler.getMemberRegisterService();
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPassword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
		ChangePasswordService changePwdSvc = 
				assembler.getChangePasswordService();
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println();
	}
}
```

## Step 5: 스프링의 DI 설정 
### 스프링을 이용한 객체 조립과 사용
```java
//sp5-chap03/src/main/config/AppCtx.java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;
import spring.VersionPrinter;

@Configuration
public class AppCtx {

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberRegisterService memberRegSvc() {
		return new MemberRegisterService(memberDao());
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		ChangePasswordService pwdSvc = new ChangePasswordService();
		pwdSvc.setMemberDao(memberDao());
		return pwdSvc;
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
	
	@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter(memberDao(), memberPrinter());
	}
	
	@Bean
	public MemberInfoPrinter infoPrinter() {
		MemberInfoPrinter infoPrinter = new MemberInfoPrinter();
		infoPrinter.setMemberDao(memberDao());
		infoPrinter.setPrinter(memberPrinter());
		return infoPrinter;
	}
	
	@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
}
```

```java
//sp5-chap03/src/main/java/main/MainForSpring.java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;
import spring.WrongIdPasswordException;

public class MainForSpring {

	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppCtx.class);
		
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			} else if (command.equals("list")) {
				processListCommand();
				continue;
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
				continue;
			} else if (command.equals("version")) {
				processVersionCommand();
				continue;
			}
			printHelp();
		}
	}

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
		MemberRegisterService regSvc = 
				ctx.getBean("memberRegSvc", MemberRegisterService.class);
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPassword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
		ChangePasswordService changePwdSvc = 
				ctx.getBean("changePwdSvc", ChangePasswordService.class);
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println();
	}

	private static void processListCommand() {
		MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}

	private static void processInfoCommand(String[] arg) {
		if (arg.length != 2) {
			printHelp();
			return;
		}
		MemberInfoPrinter infoPrinter = 
				ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}

}
```

### DI 방식 1 : 생성자 방식
 ```java
 //sp5-chap03/src/main/java/spring/MemberPrinter.java
 package spring;

public class MemberPrinter {

	public void print(Member member) {
		System.out.printf(
				"회원 정보: 아이디=%d, 이메일=%s, 이름=%s, 등록일=%tF\n", 
				member.getId(), member.getEmail(),
				member.getName(), member.getRegisterDateTime());
	}

}
 ```
 
 ```java
 //sp5-chap03/src/main/java/spring/MemberListPrinter.java
 package spring;

import java.util.Collection;

public class MemberListPrinter {

	private MemberDao memberDao;
	private MemberPrinter printer;

	public MemberListPrinter(MemberDao memberDao, MemberPrinter printer) {
		this.memberDao = memberDao;
		this.printer = printer;
	}

	public void printAll() {
		Collection<Member> members = memberDao.selectAll();
		members.forEach(m -> printer.print(m));
	}

} 
 ```
 
 ### DI 방식 2 : 세터 메서드 방식
 
 ```java
 // sp5-chap03/src/main/java/spring/MemberInfoPrinter.java
package spring;

public class MemberInfoPrinter {

	private MemberDao memDao;
	private MemberPrinter printer;

	public void printMemberInfo(String email) {
		Member member = memDao.selectByEmail(email);
		if (member == null) {
			System.out.println("데이터 없음\n");
			return;
		}
		printer.print(member);
		System.out.println();
	}

	public void setMemberDao(MemberDao memberDao) {
		this.memDao = memberDao;
	}

	public void setPrinter(MemberPrinter printer) {
		this.printer = printer;
	}

}
 ```
 
 ### 기본 데이터 타입 값 설정
 
 ```java
 //sp5-chap03/src/main/java/spring/VersionPrinter.java
 package spring;

public class VersionPrinter {

	private int majorVersion;
	private int minorVersion;

	public void print() {
		System.out.printf("이 프로그램의 버전은 %d.%d입니다.\n\n",
				majorVersion, minorVersion);
	}

	public void setMajorVersion(int majorVersion) {
		this.majorVersion = majorVersion;
	}

	public void setMinorVersion(int minorVersion) {
		this.minorVersion = minorVersion;
    }

}
 ```
 
 ## @Configuration 설정 클래스의 @Bean 설정과 싱글톤
 
 ## 두 개 이상의 설정 파일 사용하기
 
 ```java
 //sp5-chap03/src/main/java/config/AppConf1.java
 package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.MemberDao;
import spring.MemberPrinter;

@Configuration
public class AppConf1 {

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
	
}
 ```
 
 ```java
 //sp5-chap03/src/main/java/config/AppConf2.java
 package config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;
import spring.VersionPrinter;

@Configuration
public class AppConf2 {
	@Autowired
	private MemberDao memberDao;
	@Autowired
	private MemberPrinter memberPrinter;
	
	@Bean
	public MemberRegisterService memberRegSvc() {
		return new MemberRegisterService(memberDao);
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		ChangePasswordService pwdSvc = new ChangePasswordService();
		pwdSvc.setMemberDao(memberDao);
		return pwdSvc;
	}
	
	@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter(memberDao, memberPrinter);
	}
	
	@Bean
	public MemberInfoPrinter infoPrinter() {
		MemberInfoPrinter infoPrinter = new MemberInfoPrinter();
		infoPrinter.setMemberDao(memberDao);
		infoPrinter.setPrinter(memberPrinter);
		return infoPrinter;
	}
	
	@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
}
 ```
 
 ### @Configuration 애노테이션, 빈, @Autowired 애노테이션
 
 ### @Import 애노테이션 사용
 ```java
//sp5-chap03/src/main/java/config/AppConfImport.java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import spring.MemberDao;
import spring.MemberPrinter;

@Configuration
@Import({AppConf2.class})
public class AppConfImport {

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
}
 ```

### getBean() 메서드 사용

지금까지 작성한 예제는 getBean() 메서드를 이용해서 사용할 빈 객체를 구했다.
```java
VersionPrinter versionPrinter = ctx.getBean("versionPrinter", VersionPrinter.class);
```

### 주입 대상 객체를 모두 빈 객체로 설정해야 하나?



**Info:** 지금까지 이전에 공부했던 내용을 정리해보면서 내용을 다듬었습니다. 잘못된 부분은 댓글 남겨주시면 수정하겠습니다. 감사합니다.<br>
{: .notice--info}

**참고자료** <br>
-- 초보 웹 개발자를 위한 스프링5 프로그래밍 입문 (최범균 저) <br>
{: .notice--info}
