# Spring Concepts

### Spring Framework와 Spring Boot 차이

Spring Framework는 오픈소스 자바 웹 어플리케이션 표준 프레임워크이다.\
자바 웹 어플리케이션을 개발하기 위해서 Servlet 클래스를 상속받아서 구현해야 하지만, Spring을 사용하면 POJO만으로 웹 어플리케이션 개발이 가능하다. Spring이 Servlet 작업을 알아서 처리해주기 때문이다.

Spring Boot는 Spring Framework를 개선한 새로운 프레임워크라기 보다,\
Spring 설정을 최대한 줄이고 (XML 설정 필요없이 Java로 설정), 사용하기 편하게 Spring Best Practice를 모아놓은 Spring Wrapper이다.

### Spring Framework 특징

#### Spring Container

Spring Container는 자바 객체(POJO, Beans)의 라이프 사이클을 관리한다.

#### POJO

POJO는 프레임워크에 의존하지 않는 순수 자바 객쳐를 말하면, Beans라고도 부른다.\
Spring Beans는 Spring IoC Container가 관리하는 자바 객체이다.

_Java Beans는 DTO, VO 형태를 Java Beans라 하며, private 필드에 getter, setter로만 접근 가능한 형태로 전달 인자가 없는 생성자를 가지는 형태의 클래스이다._

#### DI / IoC (Dependency Injection / Inversion Of Control)

POJO 간 의존 관계를 위해 필요한 객체를 직접 생성하는 것이 아니라, 외부에서 (Spring Framework가) 생성한 후 주입시켜주는 방식이다. (Dependency Injection) 그래서 클래스 관리 주체가 Spring Framework이다.

객체 간의 의존관계를 설정파일이나 어노테이션으로 설정할 수 있으며, @Autowired 어노테이션을 사용하여 Component 간의 의존 관계를 맺을 수 있다.

#### PSA (Portable Service Abstraction)

특정 기술에 영향 받지 않게 POJO 기반으로 추상화하여 외부 라이브러리가 달라지더라도 동일한 인터페이스를 유지하는 구조를 말한다. 예를 들어, JPA로 Hibernate 혹은 EclipseLink를 사용하든 의존성을 고려할 필요가 없다.

#### AOP (Aspect Oriented Programming)

AOP는 핵심 기능과 부가 기능(로그인, 트랜잭션, 로깅, 보안, 캐싱 등)을 분리하여, 복잡도를 낮추고 재사용성을 높이기 위한 프로그래밍 기법이다.

공통기능이나 절차 등을 하나로 묶어 별도로 관리하는 것이 목적이다.

객체지향 기본 원칙(OOP)을 적용해서 어플리케이션의 핵심 기능(Core Concerns)과 부가 기능(Cross-cutting Concerns)을 분리해서 모듈화하는 것은 매우 어렵다.

### Spring 구성요소



### Spring MVC



__





