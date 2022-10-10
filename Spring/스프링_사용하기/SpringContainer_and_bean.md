# Spring container

Spring 컨테이너는 Spring Framework의 핵심. 스프링 컨테이너는 객체를 생성하고, 연결하고, 구성하고, 생성에서 소멸까지 전체 수명 주기를 관리한다. 이러한 객체를 Spring Bean이라고 함.

## 1. Spring container 생성

인터페이스인 ApplicationConetxt에 AnnotationConfigApplicationcontext과 같은 구현체를 붙여서 생성한다. ApplicationConetxt를 일반적으로 스프링 컨테이너로 생각하면 됨.

- ApplicationConetxt는 스프링 빈을 관리하고 조회하는 BeanFactory 인터페이스의 기능을 모두 상속받아 사용함. BeanFactory 인터페이스에 여러 부가기능들을 추가한것이 ApplicationConetxt.
- 보통 ApplicationConetxt를 사용함.

## 2. Spring bean 만들기

✒︎ spring boot를 사용하는 최근에는 Annotation을 통해 Spring bean을 만든다.

- 설정 클래스에 @Configuration, 의존성을 나타낸 메소드에 @bean 어노테이션을 붙임으로써 스프링 빈으로 만든다.
- 어노테이션으로 만든 설정 클래스는 AnnotationConfigApplicationcontext 메소드에 인자로 넘겨 사용한다.

✒︎ 레거시 프로젝트에서는 xml을 통해 Spring bean을 만든다.

- xml파일을 통해 스프링 빈을 생성한다.
- xml파일로 자바 클래스로 만든 설정 구성과 똑같이 구성할 수 있다. (형태만 다르고 내용은 똑같음)
- xml로 만든 설정 클래스는 GenericXmlApplicationContext 메소드에 인자로 넘겨 ApplicationContext에 붙여 사용한다.

✒︎ 여러 형태의 설정정보를 받을 수 있는 원리

- 스프링 컨테이너가 BeanDefinition이라는 인터페이스만 바라보고 스프링 빈을 만들기 때문임.
  - Java 코드로 만든 설정정보를 BeanDefinition으로 만들어서 스프링 컨테이너에 던진다
  - xml로 만든 설정 정보를 BeanDefinition으로 만들어서 스프링 컨테이너에 던진다.

## 3. Spring container속 Spring Bean 저장소

스프링 빈을 저장하는 저장소.
저장 형태는 key와 value로, key에 기본값으로 메소드 이름이 들어가고, value로 인터페이스에 주입된 구현체(Bean 객체)가 들어간다.

## 4. Spring bean 의존관계 연결

스프링 컨테이너에 스프링 빈이 먼저 올라가고 나서 의존관계가 주입된다.  
이부분은 차후 의존성 자동주입 공부 후 다시 작성
