# Dependency injection (의존관계 주입)

<mark> 사용 영역의 코드를 수정하지 않고 여러 구현체를 바꿔 낄 수 있도록 해야한다.</mark>

[(참고) 클래스 의존관계](%ED%81%B4%EB%9E%98%EC%8A%A4_%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84.md)

## 1. DIP를 잘 지킨 코드는 인터페이스에만 의존한다.

(코드 출처: 인프론 김영한님 스프링 기본편 강좌)

```java
public class MemberServiceImpl implements MemberService {

    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }
}

```

- 내 눈에 구현체를 의존하는 코드가 안보임
- 인터페이스에 구현체를 주입하는 방법 중 하나로 생성자 주입을 사용함

## 2. OCP를 잘 지킨 코드는 구현체를 갈아껴도 사용 영역의 코드를 수정할 필요가 없다.

(코드 출처: 인프론 김영한님 스프링 기본편 강좌)

```java
@Configuration
public class AppConfig {
    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

- 설정 파일을 구성할 때 인터페이스와 구현체가 한눈에 들어올 수 있도록 코드를 구성하면 좋다.
  - MemberRepository 인터페이스의 현재 구현체는 MemoryMemberRepository임이 한눈에 보임
- 구현체를 갈아낄때는 이곳 설정 영역의 부분만 수정하면 된다.
- <mark>이곳에서 생성자 주입 형태로 정적 클래스 의존관계인 사용영역에 의존성을 주입한다.</mark>
