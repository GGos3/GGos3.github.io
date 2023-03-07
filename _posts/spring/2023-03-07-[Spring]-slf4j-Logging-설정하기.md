---
layout: post
title: "[Spring] slf4j Logging 설정하기"
date: 2023-03-07 11:16:33
tags:
categories: spring
noindex: false
comment: true
image: /assets/img/[Spring]-slf4j-Logging-설정하기/thumbnail_(2).png
---

# [Spring] slf4j Logging 설정하기
Spring 프로젝트에서 간단한 로깅 설정을 해보도록 하겠다.

## 프로젝트 적용

`slf4j` 라이브러리는 기본적으로 `spring boot` 에 포함 되어있지만 어떠한 이유로 `slf4j` 가 없다면

`build.gradle` 에 의존성을 추가해 프로젝트에 `slf4j` 를 적용할 수 있다.

`build.gradle`

```
dependencies {
		// slf4j 시작
    implementation 'org.slf4j:slf4j-api:1.7.31'
    implementation 'ch.qos.logback:logback-core:1.2.3'

    implementation ('ch.qos.logback:logback-classic:1.2.3'){
        exclude group: 'org.slf4j', module: 'slf4j-api'
        exclude group: 'ch.qos.logback', module: 'logback-core'
		// slf4j 끝
}
```

---

## 로그 선언

```java
private Logger logger = LoggerFactory.getLogger(getClass());

private static final Logger logger = LoggerFactory.getLogger(클래스명.class)

//Lombok
@Slf4j
```

## 로그 사용

`LogTestController`

```java
@RestController
public class LogTestController {
    private final Logger logger = LoggerFactory.getLogger(getClass());

    @RequestMapping("/log")
    public String logTest() {
        String name = "Spring";

        logger.trace(" trace log={}", name); // 로컬
        logger.trace(" debug log={}", name); // 개발
        logger.info(" info log={}", name); // 운영
        logger.warn(" warn log={}", name);
        logger.error(" error log={}", name);

        return "Logging on";
    }
}
```

### 출력 결과

```java
name = Spring
2023-03-07T10:47:17.305+09:00  INFO 11176 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  info log=Spring
2023-03-07T10:47:17.306+09:00  WARN 11176 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  warn log=Spring
2023-03-07T10:47:17.306+09:00 ERROR 11176 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  error log=Spring
```

---

```java
logger.trace(" trace log={}", name); // 로컬
logger.trace(" debug log={}", name); // 개발
```

출력 결과를 확인하면 위 두 줄의 코드에 해당하는 로그가 없는 것을 발견 할 수 있다.

이와 같은 경우는 로그 레벨을 설정하지 않아서 발생하는 것으로 `[application.properties](http://application.properties)` 에서 설정을 해주어야 한다.

## 로그 레벨 설정

### 로그 레벨

- trace: 로컬 서버
- debug: 개발 서버
- info: 운영 서버

로그 레벨을 자신의 환경에 맞게 설정해 불필요한 로그를 줄일 수 있다.

### 로그 레벨 적용

 `[application.properties](http://application.properties)` 

```java
logging.level.hello.springmvc=trace
```

로그 레벨을 적용한 뒤 프로젝트를 다시 실행해 보면

```java
name = Spring
2023-03-07T11:06:00.817+09:00 TRACE 1116 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  trace log=Spring
2023-03-07T11:06:00.820+09:00 TRACE 1116 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  debug log=Spring
2023-03-07T11:06:00.820+09:00  INFO 1116 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  info log=Spring
2023-03-07T11:06:00.820+09:00  WARN 1116 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  warn log=Spring
2023-03-07T11:06:00.820+09:00 ERROR 1116 --- [nio-8080-exec-1] h.springmvc.basic.LogTestController      :  error log=Spring
```

아까는 보이지 않던 `TRACE` 로그가 추가된 것을 알 수 있다.

---

## 마무리

이제 `slf4j` 라이브러리를 사용해 Logging을 설정함으로서 로그의 시간, 스레드, 클래스 등 정보를 한눈에 볼 수 있고, 로그 레벨을 통해 환경에 맞게 로그의 수를 관리 할 수 있게 되었다.