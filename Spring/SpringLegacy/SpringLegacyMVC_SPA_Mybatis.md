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
- 여기서 재미있는점은 자바 빈 기술인데, memberInsert 메소드의 파라미터가 Member를 받는다. 이것은 서버로 들어온 데이터 스트림의 순서(name, id, pw)와 파라미터에 열려있는 Member 객체(자바 빈)의 필드의 순서가 정확히 일치하고, 디폴트 생성자가 있으며 셋겟 메소드가 있다면 자동으로 저 파라미터 m에 데이터가 맵핑되는 점이다. 때문에 뭘 할필요도 없이 바로 m.getName()을 할수가 있음.

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

또한 SqlSession 메소드중

```java
sqlSession.insert("mapper.member.memberInsert", m);
```

코드의 "mapper.member.memberInsert"의 의미는 root-context.xml의 내용 중

```xml
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis/model/modelConfig.xml"></property>
    <property name="mapperLocations" value="classpath:mybatis/mappers/*.xml"></property>
</bean>
```

부분의 두번째 세번째 property 태그를 보면 된다.  
property는 setter이고, value 옵션을 통해 값을 넣어주어 변수를 setting하는데,

- setConfigLocation()를 통해 classpath:mybatis/model/modelConfig.xml의 값을 세팅한다.
- setMapperLocations()를 통해 classpath:mybatis/mappers/\*.xml의 값을 세팅한다.
- classpath는 /src/main/resources를 의미한다.
- 두 value에 해당하는 xml 파일들을 올바른 경로에 맞추어 생성해준다.

먼저 modelConfig.xml를 보자

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<typeAliases>
		<typeAlias type="com.ssafy.cafe3.dto.Member" alias="myMember"/>
	</typeAliases>
</configuration>

```

여길 보면 내가 만든 dto의 경로를 type에 놓았고, alias를 아무거나 지정하면 되는데 나는 일단 myMember로 했다.  
내가 쓸 dto를 mybatis에 등록한 코드이다.

그 다음 member.xml을 보자

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="mapper.member">
	<insert id="memberInsert" parameterType="myMember">
		insert into member(name, id, pw) values(#{name}, #{id}, #{pw})
	</insert>
	<select id="login" resultType="String" parameterType="myMember">
		select name from member where id=#{id} and pw=#{pw}
	</select>
</mapper>
```

이 xml 코드들은 실제 사용할 sql을 작성 및 등록하는 역할을 한다.  
namespace="mapper.member"를 네임스페이스로 지정했는데, "맵퍼를 만들건데, 도메인은 멤버이다"의 의미로 만들었다.  
회원가입 할때 사용할 sql의 id는 memberInsert로 하였고, 넘어올 파라미터의 타입으로 아까 alias로 설정했던 myMember를 둔다. sql문을 보면 #{name} 이런식으로 되어있는데, #을 붙이면 PreparedStatement를 통해 쿼리를 실행시키는것. 참고로 $표시는 그냥 statement이므로 주의해야한다.

다시 DAO코드를 보면,

```java
sqlSession.insert("mapper.member.memberInsert", m);
```

- 여기서 insert는 sqlSession의 메소드이고
- 첫번째 인자는 "맵퍼를 만들껀데 도메인은 멤버고 (여기까지 네임스페이스) id가 memberInsert인 쿼리를 수행하자"는 의미이다. 참고로 네임스페이스까지 써주면 맵퍼 xml이 많을때 단숨에 찾을 수 있기 때문에 속도가 빠르다.
- 두번째 인자는 Member dto 객체인데, 이것은 아까 맵퍼에서 parameterType="myMember"을 해줬기 때문에 받을 수 있는것.

DTO까지 모두 수행되고, DB에 멤버가 등록되어 회원가입이 완료되면, 서비스 메소드로 돌아오고, 컨트롤러로 다시 돌아온다.

```java
	@RequestMapping(value = "memberInsert.shop", method = RequestMethod.GET, produces="application/text;charset=utf-8")
	@ResponseBody
	public String memberInsert(Member m) {

		System.out.println(m);

		memberService.memberInsert(m);

		return m.getName()+"님 안녕하세요?";
	}

```

다시 여기로 돌아오는데, 회원가입이 성공했다면(중복 회원 검사 코드는 아직 작성하지 않았다) m.getName()+"님 안녕하세요?"이라는 스트링 조각을 아웃 버퍼에 담는다. 아웃 버퍼에 담는것은 @ResponseBody 어노테이션을 부착 했기 때문에 담을 수 있는것이고, 아웃 버퍼에 담아서 자바스크립트로 보낸 후 화면에 alert를 띄우고 회원가입이 완료된다.

## 로그인 구현

로그인도 마찬가지 구조로 되어있다. 먼저 index.html의 로그인 영역에 id, pw를 입력하고 로그인 버튼을 누르면, 자바스크립트 콜백이 작동되고,

```javascript
//로그인 처리
$("#loginBtn").click(function () {
  let id = $("#id").val();
  let pw = $("#pw").val();

  $.post(
    "login.shop",
    {
      id: id,
      pw: pw,
    },
    function (data, status) {
      alert(data);
      $("#msgDiv").html(data);
    }
  ); //end post()
}); //end 로그인 처리
```

post방식으로 login.shop url을 데이터와 함께 보낸다 (마찬가지로 AJAX).  
.shop으로 왔으니 웹 컨테이너가받는것이고, 스프링컨테이너가 @RequestMapping 정보를 참고하여 적절한 컨트롤러 메소드로 보내준다.

```java
@RequestMapping(value = "login.shop",
			method= {RequestMethod.POST},
			produces = "application/text;charset=utf8")
	@ResponseBody
	public String login(HttpServletRequest request,
			HttpServletResponse response){

		String id=request.getParameter("id");
		String pw=request.getParameter("pw");

		try {
			Member m = new Member(id,pw);
			String name=memberService.login(m);
			if(name!=null) {
				HttpSession session=request.getSession();
				session.setAttribute("member", m);
				System.out.println(m);
				return name+"님 접속중";
			}else {
				return "로그인 실패";
			}
		}catch(Exception e) {
			return e.getMessage();
		}
	}
```

여기서 특이한 점은 로그인 메소드에 HttpServletRequest request, HttpServletResponse response 인자를 내 마음대로 쓴 것인데, 거대한 스프링 파서가 @RequestMapping 붙은 메소드는 서블릿의 서비스 메소드로 인정해주기 때문에 가능하다.  
기존 로그인 코드와 거의 비슷하고, 위에서 설명한 의존성 자동주입 어노테이션을 통해 memberService, memberDao 객체는 의존성 주입이 된 상태이다.

DAO 코드를 보면,

```java
public String login(Member m) {
    return sqlSession.selectOne("mapper.member.login", m);
}
```

sqlSession.selectOne 메소드를 사용하였는데, 이것은 로그인같이 딱 하나만 select될때 사용한다.

다시 member.xml을 보면,

```xml
	<select id="login" resultType="String" parameterType="myMember">
		select name from member where id=#{id} and pw=#{pw}
	</select>
```

로그인 처리를 하는 부분인데, 받는 파라미터는 위에서 한것과 똑같이 parameterType="myMember"를 썼고, 리턴 타입도 써줬는데, 로그인 메소드의 리턴은 사용자 이름의 스트링이므로 resultType="String"를 써주었다.

Dao에서 로그인이 완료되면 사용자 이름을 리턴해주고, 서비스를 지나 컨트롤러로 돌아오며, 받아온 name이 null이 아니면 항상 하던대로 HttpSession에 올려주고, "로그인 성공" 데이터 조각을 리턴 / null이면 "로그인 실패" 데이터 조각을 리턴하는 식으로 코드를 구성해 주었다.  
자바스크립트에서는 다시 서버로부터 데이터 조각을 받고, html을 재구성 하는 식으로 로그인 완료를 동적 html로 만들어 준다.
