# AdapterPattern

### ✒︎ 어댑터 패턴 구조 및 쓰는 상황

![adapterPattern](/img/patterns/adaptorPattern.jpeg)

현재 개발 완료되어 수정을 하기 힘든 Adaptee 클래스를 내가(client) 의존하고 있는 Target 인터페이스의 구현체로 쓰고 싶을 때 사용한다. 물론 Target 인터페이스와 Adaptee는 아무런 관계가 없는 상태임.

직관적으로 클라이언트 코드에서

```java
Adaptee adaptee = new Adaptee();
Target t = new Adapter(adaptee);
```

이거 하고 싶다는 것.

Adapter 클래스에서 Target interface를 상속한다. 그러면 Target의 메소드를 모두 구현해야하는데, 그 구현한 메소드 바디에다가 현재 의존중인 Adaptee 객체의 메소드를 호출하는 식으로 구성한다.  
결과적으로 client에서 target 인터페이스 구현체로 Adapter를 선택 한다면, Adaptee에 대한 수정 없이 target.메소드()를 통해 Adaptee의 기능을 사용할 수 있게 된다.

추가 예시로는 유명한 오리/칠면조 예시가 있다.
