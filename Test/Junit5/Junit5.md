# Junit5

## 1. Junit5란

- Java 환경에서 단위 테스트를 하기위한 '프레임워크'

## 2. Quick start

```java
@Test
    @DisplayName("Vip는 10% 할인이 되어야 한다. (성공테스트)")
    void vip_o() {
        // given
        Member memberVip = new Member(1L, "memberVip", Grade.VIP);

        // when
        int discount = discountPolicy.discount(memberVip, 10000);

        // then
        Assertions.assertThat(discount).isEqualTo(1000);
    }
```

(출처: 인프런 - 김영한님 스프링 기본편 강좌)

Test annotation을 붙이고 작성하고 싶은 테스트 코드를 채워넣는다.
해당 테스트 코드는 Junit의 생명주기 과정에서 적절한 타이밍에 콜됨 (IoC 이므로 Junit은 프레임워크이다)
