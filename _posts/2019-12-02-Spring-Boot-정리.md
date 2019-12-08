---
title: Spring Boot 정리
tags: 
    - java
    - spring boot
categories: Spring-Boot
layout: post
---

## Spring Boot

### Feature
1. 임베디드 톰캣, 제티, 언더토우를 사용하여 독립 실행 가능
2. 통합 스타터를 제공하여 maven/gradle 구성 간소화
3. 스타터를 통한 자동화된 스프링 설정 제공
4. 번거로운 xml 설정을 요구하지 않음
5. JAR을 사용하여 자바 옵션만으로도 배포 가능
6. 의존성 관리 수월
    
### Gradle
* 버전 수정 : gradle-wrapper.properties에서 distributionUrl 변경
* 프로젝트 설정 : settings.gradle (ex. rootProject.name = "demo")

### Basic Pachage Path
* src/main/java : 자바 소스 경로
* src/test/java : 스프링 부트의 테스트 코드 경로
* src/main/resources/static : static 파일의 디폴트 경로 (css, image, js...)
* src/main/resources/templates : 템플릿 파일 경로 (thymeleaf, freemarker...)

### Multi Module
* 웹, API, 배치, 기타 프로젝트가 공통된 도메인이나 유틸리티를 사용할때 구성
* 중복 코드를 제거 할 수 있어서 실수와 번거로움을 줄임
* 코드 재사용성이 높아지고 하나의 통합 프로젝트처럼 관리 가능

### environment yml
* application.properties와 application.yml 둘 다 생성되어 있다면 application.yml만 오버라이드 되어 적용됨
* 프로퍼티 설정 구분 방법
    * 파일 내 `---` 사용
    * application-{profile}.yml 파일 사용 (해당 프로파일 yml 적용 후 application.yml 적용)
* 프로퍼티 값을 추가하는 것만으로도 환경 설정에 자동으로 적용