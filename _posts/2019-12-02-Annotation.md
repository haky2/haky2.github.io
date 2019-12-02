---
title: Spring Boot Annotation
tags: 
    - java
    - spring boot
categories: JAVA
layout: post
---
    
## Annotation
### Lombok
* 자바 컴파일 시점에 특정 어노테이션에 해당하는 코드를 추가/변경하는 라이브러리
* 코드 경량화와 가독성 우성
* @Getter : get 구현
* @Setter : set 구현
* @EqualsAndHashCode : equals, hashcode 구현, @EqualsAndHashCode(of={"name"})와 같이 특정 필드만 지정 가능
* @AllArgsConstructor : 객체의 모든 필드값을 인자로 받는 생성자 구현
* @NoArgsConstructor : 아무런 인자값이 없는 기본 생성자 구현
* @RequiredArgsConstructor : @NonNull이 적용된 필드값만 인자로 받는 생성자 구현
* @ToString : tostrping 구현, @ToString(exclude="password")와 같이 특정 필드 제외 가능
* @Data : @ToString + @EqualsAndHashCode + @Getter + @Setter + @RequiredArgsConstructor
* @Builder : 빌더 패턴 적용

### 참고
* [lombok 주의사항1](https://www.popit.kr/실무에서-lombok-사용법/)
* [lombok 주의사항2](https://kwonnam.pe.kr/wiki/java/lombok/pitfall)
* [빌더 패턴](http://blog.naver.com/PostView.nhn?blogId=varkiry05&logNo=221680218440&categoryNo=107&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)

---

### Spring Annotation
* @RestController : @Controller + @ResponseBody 
* @GetMapping : get 매핑 (기본값 value="") 
* @Value : 
    * 프로퍼티의 키를 사용하여 특정한 값 호출
    * 키 값을 입력해야 하며, 값이 없을 경우에 대해 예외 처리를 해야함. 
    * `.`으로 계층 구분
    * @Value("${property.test.name}")
    * @Value("{noKey: default value}")
    * @Value("#{'${propertyTestList}'.split(',')")
* @ConfigurationProperties
    * 기본적으로 prefix를 사용하여 값을 바인딩
    * 클래스 헤드에 @ConfigurationProperties("접두사") 작성 하여 사용
    * @Component와 같이 사용해야 함(그래야 사용하려는 곳에서 의존성 주입 가능). 
    * application.yml이 아닌 다른 이름의 yml에서 관리할때는 @ConfigurationProperties(preifx = "접두사")
    * 프로퍼티명은 소문자나 케밥 케이스(-)를 지원하고 필드는 카멜 케이스로 작성 (fruit.color-name, fruit.colorname -> string clorName)
* @Component : 직접 컨트롤이 가능한 class인 경우 사
* @Bean : 외부 라이브러들을 bean으로 등록할때 사용
* @SpringBootApplication : @SpringBootConfiguration + @EnableAutoConfiguration + @ComponentScan
* @SpringBootConfiguration : 스프링 부트의 설정을 나타냄
* @EnabledAutoConfiguration : 클래스 경로에 지정된 내용을 기반으로 설정 자동화 수행. @Configuration과 함께 사용해야 함
* @ComponentScan : 특정 패키지 경로를 기반으로 @Configuration에서 사용할 @Component 설정 클래스를 찾음
* @Configuration : 하나 이상의 bean을 구성한 클래