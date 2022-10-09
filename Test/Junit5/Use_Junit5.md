# Junit5 문법

### 1. @Test

테스트 메소드에 붙여 사용

### 2. @DisplayName("테스트 제목")

콘솔창에 테스트 제목을 띄울때 사용

### 3. @BeforeEach

각 테스트가 실행되기 전에 콜되는 메소드에 붙임

### 4. AssertThrow 메소드

Junit의 Assertions를 import 해야 한다.

```java
Assertions.assertThrows(NoUniqueBeanDefinitionException.class, () -> ac.getBean(MemberRepository.class));
```

이와 같이 사용하는데, 첫번째 인자의 에러가 두번째 인자의 코드로 인해 발생하면 테스트가 성공.
