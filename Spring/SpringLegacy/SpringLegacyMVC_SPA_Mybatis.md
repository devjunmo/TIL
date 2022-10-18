# Spring Legacy MVC - SPA & MyBatis 연동

## 전체 디렉토리 구조

![network layer](/img/springLegacy/SpringLegacy2_SPA_Mybatis/1dir.png)
![network layer](/img/springLegacy/SpringLegacy2_SPA_Mybatis/2_dir.png)

## 웰컴 페이지 재구성

처음 생성된 web.xml 파일을 보면

```xml
<servlet-mapping>
    <servlet-name>appServlet</servlet-name>
    <url-pattern>*.shop</url-pattern>
</servlet-mapping>
```

위의 _.shop 부분이 원래는 / 였다. 즉 웰컴페이지 마저도 웹컨테이너가 받아서 서블릿 객체를 생성하는 그 과정을 한다는 것인데, 이것은 비효율 적이므로 _.shop으로 변경하여 .shop으로 올때만 웹 컨테이너로 갈 수 있도록 설정한다.  
부트스트랩에서 가져온 화면들을 webApp 밑에 붙여놓고, 적당한 위치에 회원가입 버튼과 로그인 입력창을 배치하였다.

## 회원가입 구현

index.html에 회원가입 버튼을 만들었고, 그것을 누르면 발동되는 자바스크립트 콜백을 제이쿼리를 통해 작성하였다.

```javascript
// 회원가입 처리
$("#memberInsertBtn").click(function () {
  let name = $("#name").val();
  let id = $("#id").val();
  let pw = $("#pw").val();

  $.post("../memberInsert.shop", { name, id, pw }, function (data) {
    alert(data); // 서버에서 온 데이터
    window.close();
  });
});
```

제이쿼리 AJAX 코드를 통해 post방식으로 서버로 데이터를 보냈고, .shop으로 들어갔으므로 web.xml과 servlet-context.xml을 참고하여 서블릿 객체를 만들고, 들어온 url을 받아서 스프링 컨테이너의 도움으로 컨트롤러의 아래 메소드로 들어온다

```java
	@RequestMapping(value = "memberInsert.shop", method = RequestMethod.GET, produces="application/text;charset=utf-8")
	@ResponseBody
	public String memberInsert(Member m) {
		
		System.out.println(m);
		
		memberService.memberInsert(m);
		
		return m.getName()+"님 안녕하세요?";
	}
	
```
- 위의 @RequestMapping 어노테이션은 말 그대로 들어온 리퀘스트를 맵핑하는데, memberInsert.shop으로 url이 넘어왔으면서, 겟방식인 요청을 받고, 인코딩은 utf-8로 해준다.
- @ResponseBody 어노테이션은 out 버퍼에 데이터 조각을 담아 보내겠다는것을 스프링에 알려주는 역할을 한다. (html 구조 전체가 아닌 스트링과 같은 데이터 조각!)

회원가입 로직은 MemberService의 인스턴스 메소드인 memberInsert로 수행되는데, 

```java
@Autowired
MemberService memberService;
```
memberService는 @Autowired 어노테이션을 통해 외부에서 DI를 하였다.
- @Autowired는 spring bean으로 등록 된 객체들을 보고 @Autowired가 붙은 클래스(또는 인터페이스)변수에 적합한 bean을 자동 주입 해준다.
- spring bean은 @Component, @Controller, @Service, @Repository 어노테이션을 붙이면 등록됨 

마찬가지로 서비스 클래스도 (@Service 어노테이션 붙여서 스프링빈 등록)
```java
@Autowired
MemberDao memberDao;

public void memberInsert(Member m) {
    memberDao.memberInsert(m);
}
```
똑같은 구조로 되어있다.  

MemberDao(@Repository를 붙여 스프링빈 등록)를 보면,

```java
@Autowired
SqlSession sqlSession;

public void memberInsert(Member m) {
    // 파라미터 네이밍 : mapper를 만들건데, 도메인은 member고, 메소드명은 memberInsert
    sqlSession.insert("mapper.member.memberInsert", m);
}

```
SqlSession 인터페이스 (mybatis꺼임)를 의존성 자동주입을 하고있고, 그 객체의 insert메소드를 수행하여 sql문을 분리하였다.

SqlSession은 먼저 전역 설정파일에 해당하는 root-context.xml을 봐야한다.

![network layer](/img/springLegacy/SpringLegacy2_SPA_Mybatis/3_sqlSession.png)

위 그림을 보면 내가 필요한 SqlSession 인터페이스에 의존관계 주입을 하기 위한 구성이 되어있다.
- property태그는 setter이다.
- constructor-arg를 통해 생성자 주입을 한다.
- ref 옵션은 레퍼런스 데이터를 받는다는것
- value 옵션은 값형 데이터를 받는다는것

xml이라 생각하지 말고 저걸 마치 자바코드라고 상상하면 이해하기 쉽다.






