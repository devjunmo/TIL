# Spring container

Spring 컨테이너는 Spring Framework의 핵심. 스프링 컨테이너는 객체를 생성하고, 연결하고, 구성하고, 생성에서 소멸까지 전체 수명 주기를 관리한다. 이러한 객체를 Spring Bean이라고 함.


## 1. Spring container 생성 
인터페이스인 ApplicationConetxt에 AnnotationConfigApplicationcontext과 같은 구현체를 붙여서 생성한다. ApplicationConetxt를 일반적으로 스프링 컨테이너로 생각하면 됨.


## 2. Spring container속 Spring Bean 저장소 
@Bean이 붙은 생성자 주입용 설정 메소드를 스프링 빈으로써 저장소에 저장한다.  
저장 형태는 key와 value로, key에 기본값으로 메소드 이름이 들어가고, value로 인터페이스에 주입된 구현체(Bean 객체)가 들어간다.


## 3. Spring bean 의존관계 연결
스프링 컨테이너에 스프링 빈이 먼저 올라가고 나서 의존관계가 주입된다.  
이부분은 차후 의존성 자동주입 공부 후 다시 작성
