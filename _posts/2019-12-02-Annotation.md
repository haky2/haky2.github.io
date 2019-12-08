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


### 참고
* [spring annotation](https://gmlwjd9405.github.io/2018/12/02/spring-annotation-types.html)

---

### Spring Boot Test
* @SpringBootTest : 통합 테스트를 제공하는 기본적인 어노테이 (애플리케이션에 설정된 빈을 모두 로드하여 규모가 크면 느림. 단위테스트...?!)
    * value : 프로퍼티 주입
    * properties : 프로퍼티 추가
    * classes : 로드할 클래스 지정 (기본 : @SpringBootConfiguration)
    * webEnvironment : 웹 환경 설정 (기본 : Mock 서블)
    * @Transactional을 사용하면 테스트를 마치고 나서 수정된 데이터가 롤백 됨
* @RunWith : JUnit에 내장된 러너 대신 어노테이션에 정의된 러너 클래스 사용 (@SpringBootTest 사용시 @RunWith(SpringRunner.class) 필요)
* @ActiveProfiles : @ActiveProfiles("local")과 같은 방식으로 원하는 프로파일 환경값을 부여하여 사용 가능
* @WebMvcTest : MVC를 위한 테스트
    * 컨트롤러 테스트에 적합
    * 웹상에서 요청과 응답에 대해 테스트
    * 시큐리티/필터까지 자동으로 테스트하며 수동으로 추가/삭제 가능
* @DataJpaTest : JPA 관련 테스트
    * JPA를 사용하여 데이터 생성, 수정, 삭제 등의 테스트
    * 실제 DB를 사요아힞 않고 내장형 DB 사용 가능
    * 기본적으로 인메모리 임베디드 데이터베이스 사용()
    * JPA 테스트가 끝날 때마다 자동으로 테스트에 사용한 데이터를 롤백
* @AutoConfigureTestDatabse : 데이터 소스 사용 지정
    * @AutoConfigureTestDatabse(replace = AutoConfigureTestDatabse.Replace.ANY) : 기본 내장 데이터소스 사용
    * @AutoConfigureTestDatabse(replace = AutoConfigureTestDatabse.Replace.NONE) : @ActiveProfiles에 설정한 데이터소스 사용
    * application.yml > spring.test.database.replace : NONE 위와 동일
    * @AutoConfigureTestDatabse(connection = H2) : 데이터 베이스 선택 (H2, Derby...)
    * application.yml > spring.test.database.connection: H2
* @JdbcTest : JDBC 관련 테스트
* @DataMongoTest : 몽고디비 관련 테스트
* @RestClientTest : REST 관련 테스트(JSON이 제대로 반환되는지 등을 테스트)
* @Rule : @After, @Before 어노테이션에 상관없이 하나의 테스트 메소드가 끝날 때마다 정의한 값으로 초기화 시켜줌
* @JsonTest : JSON 테스트
    * JSON의 직렬화와 역직렬화를 수행하는 라이브러리인 Gson과 Jackson API의 테스트를 제공
    * 문자열로 나열된 JSON 데이터를 객체로 변환하여 변환된 객체값을 테스트하거나 그 반

### 참고
* [Spring Boot Test](https://meetup.toast.com/posts/124)
* [Spring Test Guide](https://www.popit.kr/spring-guide-테스팅-전략/)