# Spring Legacy 게시판 프로젝트

## 구현 기능

- Member
  - 로그인
  - 로그아웃
- Board
  - 게시글 조회
  - 게시글 작성

---

## 교수님 노트에 내 설명을 합쳐서 작성

## Member - 로그인 / 로그아웃

1. STS3.x에서 Spring legacy MVC project 생성 (com.ssafy.board)
2. pom.xml에서 버전 설정

   - pom.xml

     ```xml
     <properties>
       <java-version>1.8</java-version>
       <org.springframework-version>5.3.23</org.springframework-version>
       <org.aspectj-version>1.9.9.1</org.aspectj-version>
       <org.slf4j-version>1.7.36</org.slf4j-version>
     </properties>
     ```

3. home.jsp를 index.jsp로 바꾼다

   - index.jsp

     ```jsp
     <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
     <%@ page session="false" contentType="text/html; charset=utf-8"%>
     <html>
     <head>
       <title>My Board</title>
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1">
       <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet">
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
     </head>
     <body>
     	<h1>
     		My Board
     	</h1>


     	<div>
     		<button id="btn-mv-join" class="btn btn-outline-success mb-3">회원가입</button>
     	</div>

     </body>
     </html>
     ```

4. index.jsp에서 회원가입 버튼을 누르면 회원가입 페이지로 가게 이벤트 처리
   - index.jsp
     ```jsp
     <c:set var="root" value="${pageContext.request.contextPath }"/>
     ...
     <script type="text/javascript">
     		document.querySelector("btn-mv-join").addEventListener("click", function(){
     			location.href="${root}/user/join";
     		});
     	</script>
     ```
5. /user/join을 라우팅

   - com.ssafy.member.controller.MemberController.java

     ```java
     package com.ssafy.member.controller;

     import org.springframework.stereotype.Controller;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.RequestMapping;

     @Controller
     @RequestMapping("/user")
     public class MemberController {

     	@GetMapping("/join")
     	public String join() {
     		return "user/join";
     	}

     }
     ```

   - @RequestMapping으로 ~/user/join 으로 들어온 url을 받아준다
   - 뷰 리졸버를 통해 prefix, suffix를 붙여 "webapp/views/user/join.jsp"로 보내준다.

6. views/user/join.jsp를 작성

   - join.jsp

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
     <c:set var="root" value="${pageContext.request.contextPath }"/>
     <!DOCTYPE html>
     <html>
     <head>
       <title>My Board</title>
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1">
       <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet">
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
     </head>
     <body>
     	<div class="container">
     		<div class="row justify-content-center">
     			<div class="col-lg-10 col-md-10 col-sm-12">
     				<h2 class="my-3 py-3 shadow-sm bg-light text-center">
     					<mark class="orange">회원가입</mark>
     				</h2>
     			</div>
     			<div class="col-lg-10 col-md-10 col-sm-12">
     				<form action="" id="form-join" method="post">
     					<div class="mb-3">
     						이름 : <input class="form-control" name="userName">
     					</div>
     					<div class="mb-3">
     						아이디 : <input class="form-control" name="userId">
     					</div>
     					<div class="mb-3">
     						비밀번호 : <input class="form-control" name="userPwd">
     					</div>
     					<button id="btn-join" class="btn btn-outline-primary mb-3">회원가입</button>
     				</form>
     			</div>
     		</div>
     	</div>

     	<script type="text/javascript">
     		document.querySelector("#btn-join").addEventListener("click",function(){
     			//추가 이벤트 처리...
     			let form=document.querySelector("#form-join")
     			form.setAttribute("action","${root}/user/join");
     			form.submit();
     		});
     	</script>
     </body>
     </html>
     ```

   - 회원가입 입력창을 채운뒤 action을 js로 동적으로 주입해서 post방식으로 submit

7. MemberController에서 회원가입 처리

   - MemberController.java

     ```java
     package com.ssafy.member.controller;

     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Controller;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PostMapping;
     import org.springframework.web.bind.annotation.RequestMapping;

     import com.ssafy.member.model.dto.MemberDto;
     import com.ssafy.member.model.service.MemberService;

     @Controller
     @RequestMapping("/user")
     public class MemberController {

     	@Autowired
     	MemberService memberService;

     	@GetMapping("/join")
     	public String join() {
     		return "user/join";
     	}

     	@PostMapping("/join")
     	public String join(MemberDto memberDto) {
     		System.out.println("MemberController:"+memberDto);

     		try {
     			memberService.joinMember(memberDto);
     			return "redirect:/user/login";
     		} catch (Exception e) {
     			// TODO Auto-generated catch block
     			e.printStackTrace();
     			return "error/error";
     		}
     	}


     	@GetMapping("/login")
     	public String login() {
     		return "user/login";
     	}
     }
     ```

   - 컨트롤러에서 회원가입 완료시 login.jsp로 리다이렉팅
   - 실패시 에러페이지로 포워딩

8. MemberService작성

   - MemberServiceImpl.java

     ```java
     package com.ssafy.member.model.service;

     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Service;

     import com.ssafy.member.model.dao.MemberDao;
     import com.ssafy.member.model.dto.MemberDto;

     @Service
     public class MemberServiceImpl implements MemberService {

     	@Autowired
     	MemberDao memberDao;

     	@Override
     	public void joinMember(MemberDto memberDto) throws Exception {
     		System.out.println("MemberServiceImpl:"+memberDto);
     		memberDao.joinMember(memberDto);
     	}

     }
     ```

   - 멤버서비스에 회원가입 서비스 로직 구현. 받은 memberDto를 Dao로 넘긴다.

9. MemberDao작성

   - MemberDaoImpl.java

     ```java
     package com.ssafy.member.model.dao;

     import org.apache.ibatis.session.SqlSession;
     import org.springframework.beans.factory.annotation.Autowired;
     import org.springframework.stereotype.Repository;

     import com.ssafy.member.model.dto.MemberDto;
     import com.ssafy.util.SqlMapConfig;

     @Repository
     public class MemberDaoImpl implements MemberDao {

     	@Override
     	public void joinMember(MemberDto memberDto) throws Exception{
     		System.out.println("MemberDaoImpl:"+memberDto);

     		try(SqlSession session=SqlMapConfig.getSqlSession();){
     			System.out.println(session);
     			int i=session.insert("com.ssafy.member.model.dao.MemberDao.joinMember",memberDto);
     			System.out.println(i);
     		}
     	}

     }
     ```

   - SqlSession은 root-context.xml에 의존관계를 구성하여 작성한 다음 @Autowired 의존관계를 주입 할수 있다.
   - 여기서는 따로 SqlMapConfig 클래스를 만들어 session을 get하는 방식으로 코드를 작성하였다.
     - SqlSession Autocloseable을 상속하므로, try 리소스문을 통해 close하여 메모리 누수를 방지한다.
   - mybatis 문법을 통해 member insert를 구현한다.

10. myBatis 라이브러리와 MySQL JDBC Driver 얻기

    - pom.xml

      ```xml
      <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.11</version>
      </dependency>

      <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.31</version>
      </dependency>
      ```

11. myBatis에서 Statement의 역할을 하는 SqlSession을 얻기 위해 유틸리티 클래스 작성

    - com.ssafy.util.SqlMapConfig.java

      ```java
      package com.ssafy.util;

      import java.io.IOException;
      import java.io.Reader;

      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.*;

      public class SqlMapConfig {

      	static SqlSessionFactory factory;

      	static {
      		try {
      			String resource="mapper/mybatis-config.xml";
      			Reader reader=Resources.getResourceAsReader(resource);
      			factory=new SqlSessionFactoryBuilder().build(reader);
      		}catch(IOException e) {
      			e.printStackTrace();
      		}
      	}

      	public static SqlSession getSqlSession() {
      		return factory.openSession(true); // true==>auto-commit
      	}

      }
      ```

12. SqlSession 객체를 DataSource와 매핑 시키기 위해 설정 파일 작성

    - src/main/resources/mapper/mybatis-config.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>

      	<properties resource="mapper/dbinfo.properties"></properties>

      	<typeAliases>
      		<typeAlias type="com.ssafy.member.model.dto.MemberDto" alias="memberDto"/>
      	</typeAliases>

        <environments default="development">
          <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
              <property name="driver" value="${driver}"/>
              <property name="url" value="${url}"/>
              <property name="username" value="${username}"/>
              <property name="password" value="${password}"/>
            </dataSource>
          </environment>
        </environments>
        <mappers>
          <mapper resource="mapper/member.xml"/>
        </mappers>
      </configuration>
      ```

    - mybatis를 사용하기 위한 mapper정보와 객체 alias 설정, mysql 설정을 한다.

13. DB 정보를 위한 파일
    - src/main/resources/mapper/dbinfo.properties
      ```
      driver=com.mysql.cj.jdbc.Driver
      url=jdbc:mysql://localhost:3306/ssafyweb
      username=ssafy
      password=ssafy
      ```
14. SQL을 작성한 매퍼 파일

    - src/main/resources/member.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

      <mapper namespace="com.ssafy.member.model.dao.MemberDao">
        <insert id="joinMember" parameterType="memberDto">
        	insert into members(user_name,user_id,user_password) values(#{userName},#{userId},#{userPwd})
        </insert>
      </mapper>
      ```

    - mapper에서 sql을 작성한다. 이 부분을 통해 java코드와 sql문을 분리한다.

15. 회원가입 성공이면 로그인 페이지로, 에러이면 에러페이지로 분기되어 있으므로 관련 페이지 작성

    - WEB-INF/views/user/login.jsp

      ```html
      <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> <%@
      taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
      <c:set var="root" value="${pageContext.request.contextPath }" />
      <c:if test="${msg ne null }">
        <script>
          alert(`${msg}`);
        </script>
      </c:if>
      <!DOCTYPE html>
      <html>
        <head>
          <title>My Board</title>
          <meta charset="utf-8" />
          <meta name="viewport" content="width=device-width, initial-scale=1" />
          <link
            href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css"
            rel="stylesheet"
          />
          <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
        </head>
        <body>
          <div class="container">
            <div class="row justify-content-center">
              <div class="col-lg-10 col-md-10 col-sm-12">
                <h2 class="my-3 py-3 shadow-sm bg-light text-center">
                  <mark class="orange">로그인</mark>
                </h2>
              </div>
              <div class="col-lg-10 col-md-10 col-sm-12">
                <form action="" id="form-login" method="post">
                  <div class="mb-3">아이디 : <input class="form-control" name="userId" /></div>
                  <div class="mb-3">비밀번호 : <input class="form-control" name="userPwd" /></div>
                  <button id="btn-login" class="btn btn-outline-primary mb-3">로그인</button>
                </form>
              </div>
            </div>
          </div>

          <script type="text/javascript">
            document.querySelector("#btn-login").addEventListener("click", function () {
              //추가 이벤트 처리...
              let form = document.querySelector("#form-login");
              form.setAttribute("action", "${root}/user/login");
              form.submit();
            });
          </script>
        </body>
      </html>
      ```

16. 로그인 처리

    - MemberController.java

      ```java
      package com.ssafy.member.controller;

      import java.util.Map;

      import javax.servlet.http.Cookie;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import javax.servlet.http.HttpSession;

      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.PostMapping;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestParam;

      import com.ssafy.member.model.dto.MemberDto;
      import com.ssafy.member.model.service.MemberService;

      @Controller
      @RequestMapping("/user")
      public class MemberController {

      	@Autowired
      	MemberService memberService;

      	@GetMapping("/join")
      	public String join() {
      		return "user/join";
      	}

      	@PostMapping("/join")
      	public String join(MemberDto memberDto) {
      		System.out.println("MemberController:"+memberDto);

      		try {
      			memberService.joinMember(memberDto);
      			return "redirect:/user/login";
      		} catch (Exception e) {
      			// TODO Auto-generated catch block
      			e.printStackTrace();
      			return "error/error";
      		}
      	}


      	@GetMapping("/login")
      	public String login() {
      		return "user/login";
      	}

      	@PostMapping("/login")
      	public String login(Model model,   @RequestParam Map<String,String> map,HttpServletRequest request,HttpServletResponse response) {

      		try {
      			MemberDto memberDto=memberService.loginMember(map);
      			if(memberDto!=null) {
      				HttpSession session=request.getSession();
      				session.setAttribute("userinfo", memberDto);
      				Cookie cookie=new Cookie("ssafy_id",memberDto.getUserId());
      				cookie.setPath("/board");
      				response.addCookie(cookie);
      				return "redirect:/";
      			}else {
      				model.addAttribute("msg", "아이디 또는 비밀번호 확인 후 다시 로그인하세요");
      				return "user/login";
      			}

      		} catch (Exception e) {
      			// TODO Auto-generated catch block
      			e.printStackTrace();
      			return "error/error";
      		}
      	}
      }
      ```

      - Model model
        - 등록된 회원이 없을때 메세지를 뿌리기 위한 객체
      - @RequestParam Map<String,String> map
        - id와 pw를 맵핑
      - HttpServletRequest request
        - 스프링 파서가 @RequestMapping 붙은 메소드는 서블릿의 서비스 메소드로 인정해주기 때문에 가능
        - 리퀘스트 객체로 세션을 생성
      - HttpServletResponse response
        - 유저 정보를 쿠키에 담아 리스폰스 객체에 담아서 뷰에 전달

    - MemberServiceImpl.java

      ```java
      package com.ssafy.member.model.service;

      import java.util.Map;

      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Service;

      import com.ssafy.member.model.dao.MemberDao;
      import com.ssafy.member.model.dto.MemberDto;

      @Service
      public class MemberServiceImpl implements MemberService {

      	@Autowired
      	MemberDao memberDao;

      	@Override
      	public void joinMember(MemberDto memberDto) throws Exception {
      		System.out.println("MemberServiceImpl:"+memberDto);
      		memberDao.joinMember(memberDto);
      	}

      	@Override
      	public MemberDto loginMember(Map<String, String> map) throws Exception {

      		return memberDao.loginMember(map);
      	}

      }
      ```

    - MemberDaoImpl.java

      ```java
      package com.ssafy.member.model.dao;

      import java.util.Map;

      import org.apache.ibatis.session.SqlSession;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Repository;

      import com.ssafy.member.model.dto.MemberDto;
      import com.ssafy.util.SqlMapConfig;

      @Repository
      public class MemberDaoImpl implements MemberDao {

      	@Override
      	public void joinMember(MemberDto memberDto) throws Exception{
      		System.out.println("MemberDaoImpl:"+memberDto);

      		try(SqlSession session=SqlMapConfig.getSqlSession();){
      			session.insert("com.ssafy.member.model.dao.MemberDao.joinMember",memberDto);
      		}
      	}

      	@Override
      	public MemberDto loginMember(Map<String, String> map) throws Exception {
      		try(SqlSession session=SqlMapConfig.getSqlSession();){
      			return session.selectOne("com.ssafy.member.model.dao.MemberDao.loginMember",map);

      		}
      	}

      }
      ```

      - mybatis 문법으로 등록된 사용자 select

    - member.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

      <mapper namespace="com.ssafy.member.model.dao.MemberDao">

        <resultMap type="memberDto" id="m">
        	<result column="user_id" property="userId"/>
        	<result column="user_name" property="userName"/>
        </resultMap>

        <insert id="joinMember" parameterType="memberDto">
        	insert into members(user_name,user_id,user_password) values(#{userName},#{userId},#{userPwd})
        </insert>

        <select id="loginMember" parameterType="map" resultMap="m">
        	select user_id,user_name from members where user_id=#{userId} and user_password=#{userPwd}
        </select>
      </mapper>
      ```

      - resultType으로 쓰면 db의 컬럼명과 dto의 변수명이 달라서 객체에 맵핑을 못함. resultMap을통해 dto에 적절히 맵핑될 수 있도록 지정해놓는다.

17. index.jsp 수정

    - index.jsp

      ```html
      <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> <%@
      taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
      <c:set var="root" value="${pageContext.request.contextPath}" />
      <c:if test="${cookie.ssafy_id.value ne null}">
        <c:set var="idck" value=" checked" />
        <c:set var="saveid" value="${cookie.ssafy_id.value}" />
      </c:if>
      <!DOCTYPE html>
      <html lang="ko">
        <head>
          <meta charset="UTF-8" />
          <meta http-equiv="X-UA-Compatible" content="IE=edge" />
          <meta name="viewport" content="width=device-width, initial-scale=1.0" />
          <link
            href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
            rel="stylesheet"
            integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3"
            crossorigin="anonymous"
          />
          <link href="${root}/assets/css/app.css" rel="stylesheet" />
          <title>SSAFY</title>
        </head>
        <body>
          <c:if test="${empty userinfo}">
            <div class="container">
              <div class="row justify-content-center">
                <div class="col-lg-10 col-md-10 col-sm-12">
                  <h2 class="my-3 py-3 shadow-sm bg-light text-center">
                    <mark class="orange">로그인</mark>
                  </h2>
                </div>
                <div class="col-lg-10 col-md-10 col-sm-12">
                  <form id="form-login" method="POST" action="">
                    <div class="form-check mb-3 float-end">
                      <input
                        class="form-check-input"
                        type="checkbox"
                        value="ok"
                        id="saveid"
                        name="saveid"
                        ${idck}
                      />
                      <label class="form-check-label" for="saveid"> 아이디저장 </label>
                    </div>
                    <div class="mb-3">
                      <label for="userid" class="form-label">아이디 : </label>
                      <input
                        type="text"
                        class="form-control"
                        id="userid"
                        name="userid"
                        placeholder="아이디..."
                        value="${saveid}"
                      />
                    </div>
                    <div class="mb-3">
                      <label for="userpwd" class="form-label">비밀번호 : </label>
                      <input
                        type="password"
                        class="form-control"
                        id="userpwd"
                        name="userpwd"
                        placeholder="비밀번호..."
                      />
                    </div>
                    <div class="text-danger mb-2">${msg}</div>
                    <div class="col-auto text-center">
                      <button type="button" id="btn-login" class="btn btn-outline-primary mb-3">
                        로그인
                      </button>
                      <button type="button" id="btn-mv-join" class="btn btn-outline-success mb-3">
                        회원가입
                      </button>
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <script
              src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"
              integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p"
              crossorigin="anonymous"
            ></script>
            <script>
              document.querySelector("#btn-mv-join").addEventListener("click", function () {
                location.href = "${root}/user/join";
              });

              document.querySelector("#btn-login").addEventListener("click", function () {
                if (!document.querySelector("#userid").value) {
                  alert("아이디 입력!!");
                  return;
                } else if (!document.querySelector("#userpwd").value) {
                  alert("비밀번호 입력!!");
                  return;
                } else {
                  let form = document.querySelector("#form-login");
                  form.setAttribute("action", "${root}/user/login");
                  form.submit();
                }
              });
            </script>
          </c:if>
          <c:if test="${!empty userinfo}">
            <div class="container">
              <%@ include file="/WEB-INF/views/common/confirm.jsp" %>
              <div class="row justify-content-center">
                <div class="col-lg-10 col-md-10 col-sm-12">
                  <h2 class="my-3 py-3 shadow-sm bg-light text-center">
                    <mark class="orange">SSAFY 게시판 - MVC Pattern</mark>
                  </h2>
                </div>
                <div class="col-lg-10 col-md-10 col-sm-12 text-center">
                  <a href="${root}/board/list?pgno=1&key=&word=">글목록</a>
                </div>
              </div>
            </div>
          </c:if>
        </body>
      </html>
      ```

      - jstl의 분기문을 통해 로그인 완료되었을때 화면과 로그인 이전의 화면을 구분해서 그려준다.
      - <%@ include file 태그를 통해 로그인 완료 되었을때 전시되는 confirm.jsp를 상단에 띄워준다.

    - /WEB-INF/views/common/confirm.jsp
      ```html
      <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
      <div class="row justify-content-center">
        <div class="col-lg-10 col-md-10 col-sm-12 m-3 text-end">
          <strong>${userinfo.userName}</strong> (${userinfo.userId})님 안녕하세요.
          <a href="${root}/user/logout">로그아웃</a><br />
        </div>
      </div>
      ```

18. 로그아웃 처리
    - MemberController.java
      ```java
      @GetMapping("/logout")
      	public String logout(HttpServletRequest request) {
      		HttpSession session=request.getSession(false);
      		if(session!=null) {
      			session.invalidate();
      		}
      		return "redirect:/";
      	}
      ```
19. 로그인 이 되었을 때

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27db92b4-bd87-4075-821b-820af43b82f7/Untitled.png)

20. 글목록 처리

- com.ssafy.board.controller.BoardController.java

  ```java
  package com.ssafy.board.controller;

  import java.util.List;
  import java.util.Map;

  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RequestParam;
  import org.springframework.web.servlet.ModelAndView;

  import com.ssafy.board.model.dto.BoardDto;
  import com.ssafy.board.model.service.BoardService;

  @Controller
  @RequestMapping("/board")
  public class BoardController {

  	@Autowired
  	BoardService boardService;


  	@GetMapping("/list")
  	public ModelAndView list(@RequestParam Map<String,String> map) {
  		ModelAndView mav=new ModelAndView();
  		try {
  			List<BoardDto> list=boardService.listArticle(map);
  			mav.setViewName("board/list");
  			mav.addObject("articles", list);

  		} catch (Exception e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  			mav.setViewName("error/error");

  		}
  		return mav;
  	}
  }
  ```

  - 게시글 목록들을 리스트로 가져와서 그 리스트를 통째로 ModelAndView를 통해 뷰로 전달한다. 전달한 리스트의 이름을 "articles"로 전달함.

- com.ssafy.board.model.service.BoardServiceImpl.java

  ```java
  package com.ssafy.board.model.service;

  import java.util.List;
  import java.util.Map;

  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;

  import com.ssafy.board.model.dao.BoardDao;
  import com.ssafy.board.model.dto.BoardDto;
  @Service
  public class BoardServiceImpl implements BoardService {

  	@Autowired
  	BoardDao boardDao;

  	@Override
  	public List<BoardDto> listArticle(Map<String, String> map) throws Exception{
  		return boardDao.listArticle( map);

  	}

  }
  ```

- com.ssafy.board.model.dao.BoardDaoImpl.java

  ```java
  package com.ssafy.board.model.dao;

  import java.util.List;
  import java.util.Map;

  import org.apache.ibatis.session.SqlSession;
  import org.springframework.stereotype.Repository;

  import com.ssafy.board.model.dto.BoardDto;
  import com.ssafy.util.SqlMapConfig;
  @Repository
  public class BoardDaoImpl implements BoardDao {

  	@Override
  	public List<BoardDto> listArticle(Map<String, String> map) throws Exception {
  		try(SqlSession sqlSession=SqlMapConfig.getSqlSession()){
  			return sqlSession.selectList("com.ssafy.board.model.dao.BoardDao.listArticle", map);
  		}
  	}

  }
  ```

- com.ssafy.board.model.dto.BoardDto.java

  ```java
  package com.ssafy.board.model.dto;

  public class BoardDto {

  	private int articleNo,hit;
  	private String userId,userName,subject,content,registerTime;


  	public BoardDto() {
  		super();
  		// TODO Auto-generated constructor stub
  	}
  	public BoardDto(int articleNo, int hit, String userId, String userName, String subject, String content,
  			String registerTime) {
  		super();
  		this.articleNo = articleNo;
  		this.hit = hit;
  		this.userId = userId;
  		this.userName = userName;
  		this.subject = subject;
  		this.content = content;
  		this.registerTime = registerTime;
  	}
  	public int getArticleNo() {
  		return articleNo;
  	}
  	public void setArticleNo(int articleNo) {
  		this.articleNo = articleNo;
  	}
  	public int getHit() {
  		return hit;
  	}
  	public void setHit(int hit) {
  		this.hit = hit;
  	}
  	public String getUserId() {
  		return userId;
  	}
  	public void setUserId(String userId) {
  		this.userId = userId;
  	}
  	public String getUserName() {
  		return userName;
  	}
  	public void setUserName(String userName) {
  		this.userName = userName;
  	}
  	public String getSubject() {
  		return subject;
  	}
  	public void setSubject(String subject) {
  		this.subject = subject;
  	}
  	public String getContent() {
  		return content;
  	}
  	public void setContent(String content) {
  		this.content = content;
  	}
  	public String getRegisterTime() {
  		return registerTime;
  	}
  	public void setRegisterTime(String registerTime) {
  		this.registerTime = registerTime;
  	}
  	@Override
  	public String toString() {
  		return "BoardDto [articleNo=" + articleNo + ", hit=" + hit + ", userId=" + userId + ", userName=" + userName
  				+ ", subject=" + subject + ", content=" + content + ", registerTime=" + registerTime + "]";
  	}



  }
  ```

- src/main/webapp/WEB-INF/views/board/list.jsp

  ```jsx
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <c:set var="root" value="${pageContext.request.contextPath}" />
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <title>My Board</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
  </head>
  <body>

  <div class="container mt-3">
    <h2>글목록</h2>

    <table class="table table-hover">
      <thead>
        <tr>
          <th>글번호</th>
          <th>제목</th>
          <th>작성자</th>
        </tr>
      </thead>
      <tbody>
       <c:forEach items="${articles}" var="boardDto">
       	<tr>
          <td>${boardDto.articleNo}</td>
          <td>${boardDto.subject}</td>
          <td>${boardDto.userId}</td>
        </tr>
       </c:forEach>


      </tbody>
    </table>
  </div>

  </body>
  </html>
  ```

  - mav를 통해 가져온 글목록 list를 articles로 꺼내는데, 그것은 EL로 편하게 가능함.
  - jstl의 forEach를 통해 화면에 편하게 출력 가능.

21. 글쓰기 화면 제공

- src/main/webapp/WEB-INF/views/board/list.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> <%@ taglib
  prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <c:set var="root" value="${pageContext.request.contextPath}" />
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <title>My Board</title>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <link
        href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css"
        rel="stylesheet"
      />
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
    </head>
    <body>
      <div class="container mt-3">
        <h2>글목록</h2>

        <form action="" id="form-param">
          <button id="btn-mv-register">글쓰기</button>
        </form>

        <table class="table table-hover">
          <thead>
            <tr>
              <th>글번호</th>
              <th>제목</th>
              <th>작성자</th>
            </tr>
          </thead>
          <tbody>
            <c:forEach items="${articles}" var="boardDto">
              <tr>
                <td>${boardDto.articleNo}</td>
                <td>${boardDto.subject}</td>
                <td>${boardDto.userId}</td>
              </tr>
            </c:forEach>
          </tbody>
        </table>
      </div>

      <script type="text/javascript">
        document.querySelector("#btn-mv-register").addEventListener("click", function () {
          let form = document.querySelector("#form-param");
          form.setAttribute("action", "${root}/board/write");
          form.submit();
        });
      </script>
    </body>
  </html>
  ```

  - 글 목록 화면. db에서 가져온 글 리스트를 화면에 뿌리는 역할.
  - 글 작성 버튼을 누르면 자바스크립트에서 이벤트를 캐치하여 서버로 board write관련 url을 보낸다.

- com.ssafy.board.controller.BoardController.java
  ```java
  @GetMapping("/write")
  	public String write() {
  		return "board/write";
  	}
  ```
  - 서버에서 write url을 보내면, 실제 글을 작성하는 페이지로 뷰 리졸버를 활용해 보내준다.
- src/main/webapp/WEB-INF/views/board/write.jsp
  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> <%@ taglib
  prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <c:set var="root" value="${pageContext.request.contextPath}" />
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>
    </head>
    <body>
      <form action="${root}/board/writeBoard" enctype="multipart/form-data" method="post">
        제목 <input name="subject" /><br />
        내용 <textarea name="content"></textarea><br />
        파일 <input type="file" name="upfile" /><br />
        <input type="submit" value="글등록" />
      </form>
    </body>
  </html>
  ```
  - 게시글 작성은 제목, 내용, 파일을 받는다.
  - 파일을 보낼때 form의 enctype은 enctype="multipart/form-data"로 지정한다.

22. 글등록 처리

- com.ssafy.board.controller.BoardController.java

  ```java
  @PostMapping("/writeBoard")
    public String write(@RequestParam("subject") String subject,
        @RequestParam("content") String content,
        @RequestParam("upfile") MultipartFile file) {
      System.out.println(subject+":"+content);
      System.out.println(file);

      // 파일 저장
      try {
        file.transferTo(new File("/Users/jun/Desktop/tmpRes/"+file.getOriginalFilename()));
      } catch (IllegalStateException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }

      return "board/list";
    }
  ```

  - @RequestParam("subject") String subject 이런 형태로 board dto의 정보를 하나하나 가져온다.
  - file은 MultipartFile 타입으로 받는다.
  - 파일 업로드를 처리하기 위해 BoardDto 변경 (upfile의 타입을 MultipartFile로!)

  ```java
  package com.ssafy.board.model.dto;

  import org.springframework.web.multipart.MultipartFile;

  public class BoardDto {

  	private int articleNo,hit;
  	private String userId,userName,subject,content,registerTime;
  	private MultipartFile upfile;

  	public BoardDto() {
  		super();
  		// TODO Auto-generated constructor stub
  	}
  	public BoardDto(int articleNo, int hit, String userId, String userName, String subject, String content,
  			String registerTime) {
  		super();
  		this.articleNo = articleNo;
  		this.hit = hit;
  		this.userId = userId;
  		this.userName = userName;
  		this.subject = subject;
  		this.content = content;
  		this.registerTime = registerTime;
  	}
  	public int getArticleNo() {
  		return articleNo;
  	}
  	public void setArticleNo(int articleNo) {
  		this.articleNo = articleNo;
  	}
  	public int getHit() {
  		return hit;
  	}
  	public void setHit(int hit) {
  		this.hit = hit;
  	}
  	public String getUserId() {
  		return userId;
  	}
  	public void setUserId(String userId) {
  		this.userId = userId;
  	}
  	public String getUserName() {
  		return userName;
  	}
  	public void setUserName(String userName) {
  		this.userName = userName;
  	}
  	public String getSubject() {
  		return subject;
  	}
  	public void setSubject(String subject) {
  		this.subject = subject;
  	}
  	public String getContent() {
  		return content;
  	}
  	public void setContent(String content) {
  		this.content = content;
  	}
  	public String getRegisterTime() {
  		return registerTime;
  	}
  	public void setRegisterTime(String registerTime) {
  		this.registerTime = registerTime;
  	}

  	public MultipartFile getUpfile() {
  		return upfile;
  	}
  	public void setUpfile(MultipartFile upfile) {
  		this.upfile = upfile;
  	}
  	@Override
  	public String toString() {
  		return "BoardDto [articleNo=" + articleNo + ", hit=" + hit + ", userId=" + userId + ", userName=" + userName
  				+ ", subject=" + subject + ", content=" + content + ", registerTime=" + registerTime + ", upfile="
  				+ upfile + "]";
  	}
  }
  ```

- 파일 업로드를 도와주는 라이브러리를 다운하기 위해 pom.xml에 디펜던시 추가
  ```java
  <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
  </dependency>
  ```
- 스프링에서 이 라이브러리를 사용한다고 등록!!! servlet-context.xml
  ```xml
  <beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
          <beans:property name="defaultEncoding" value="UTF-8"/>
          <beans:property name="maxUploadSize" value="52428800"/>
          <beans:property name="maxInMemorySize" value="1048576"/>
  </beans:bean>
  ```
