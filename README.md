# Spring Framework

spring-core : 스프링의 핵심 기능 (IoC/DI 컨테이너)

spring-context : 애플리케이션 컨텍스트, 빈(Bean) 관리

spring-aop : 관점 지향 프로그래밍

spring-jdbc / spring-tx : 데이터베이스 접근과 트랜잭션 처리

spring-web / spring-webmvc : 웹 애플리케이션 개발 (MVC 패턴)

spring-test : 단위 테스트, 통합 테스트 지원



# Spring - (core)
1. IoC (Inversion of Control, 제어의 역전)

객체의 생성과 의존관계 관리를 개발자가 직접 하지 않고, 스프링 컨테이너가 대신 해주는 개념.

개발자는 new 키워드로 객체를 만들 필요 없이, 설정 파일이나 어노테이션으로만 정의하면 스프링이 객체를 생성하고 주입함.

예: @Bean, @Component, @Autowired

2. DI (Dependency Injection, 의존성 주입)

객체 간 의존성을 외부에서 주입해주는 방식.

생성자 주입, 세터 주입, 필드 주입이 대표적임.

장점: 코드의 결합도가 낮아지고, 테스트 및 유지보수가 쉬워짐.

3. 스프링 컨테이너

IoC/DI를 실제로 관리하는 엔진 역할.

ApplicationContext라는 인터페이스를 주로 사용.

컨테이너가 Bean(객체)을 생성, 초기화, 소멸까지 생명주기를 관리.

4. 빈(Bean)

스프링 컨테이너가 관리하는 객체.

@Component 나 @Bean 으로 등록하면 스프링이 자동으로 관리.

컨테이너에 의해 관리되므로, 싱글톤으로 유지되는 경우가 많음.

5. AOP (Aspect Oriented Programming, 관점 지향 프로그래밍)

핵심 기능 코드와 부가 기능(로그, 보안, 트랜잭션 등)을 분리해서 관리

- 주요 개념 (용어)

Aspect: 횡단 관심사를 모듈화한 것 (예: 로깅 기능 클래스)

Advice: 언제 실행할지 (메소드 실행 전, 후, 예외 발생 시 등)

Join point: 적용 가능한 지점 (메소드 실행, 생성자 호출 등)

Pointcut: 실제로 적용할 구체적인 위치(패턴으로 지정)

Weaving: 실제 코드에 Aspect를 적용하는 과정

- 언제 쓰는지 ?
  
로깅

성능 모니터링

트랜잭션 관리

보안 체크

예외 처리

AOP 관련 핵심 어노테이션

1. @Aspect

이 클래스가 **AOP의 Aspect(공통 기능 모듈)**임을 명시

@Component 와 함께 써야 스프링 빈으로 등록

2. @Pointcut

공통 기능을 적용할 메소드를 패턴으로 지정

다른 어드바이스(@Before, @After 등)에서 재사용 가능

3. @Before

대상 메소드 실행 전에 실행되는 어드바이스

4. @AfterReturning

대상 메소드가 정상적으로 실행된 후 실행

returning 속성을 통해 리턴값을 받을 수 있음

5. @AfterThrowing

대상 메소드에서 예외가 발생했을 때 실행

6. @After

메소드 실행이 끝난 후 (정상 종료 + 예외 발생 모두 포함) 실행

7. @Around

메소드 실행 전/후 모두 제어 가능

ProceedingJoinPoint 로 실제 메소드를 실행하고 결과를 반환해야 함

총 정리

@Aspect → 이 클래스가 Aspect임을 선언

@Pointcut → 어떤 메소드에 적용할지 지정

@Before → 실행 전

@AfterReturning → 정상 종료 후

@AfterThrowing → 예외 발생 시

@After → 실행 후 (무조건)

@Around → 전/후 모두 제어

실행순서

클라이언트 호출
   ↓
[AOP 프록시 진입]
   ↓
@Around (before 영역)
   ↓
@Before
   ↓
[실제 대상 메소드 실행]
   ↓
@AfterReturning
   ↓
@After
   ↓
@Around (after 영역)
   ↓
결과 반환
