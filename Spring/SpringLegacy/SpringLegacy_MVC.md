# 스프링 레거시 MVC

## 1. 프로젝트 생성 방법

sts3버전(jar 파일 추가의 수고로움을 덜어줌)에서 New -> Spring Legacy Project -> Spring MVC Project 선택

## 2. 디렉토리 구조와 순서

### ✒︎ 프로젝트 생성 후 디렉토리 구조

![network layer](/img/springLegacy/1_SpringLegacydirStructure.png)  
아래는 형광펜 칠한 부분을 다룬다.
참고로, Dynamic Web Project의 WebContent가 여기에선 src/main/webapp 디렉토리이다.

webapp 밑에는 WEB-INF 디렉토리가 있는데, 이곳의 views 디렉토리에 view관련 코드들을 위치시킨다.

### ✒︎ I. web.xml

![network layer](/img/springLegacy/2_Web_xml.png)

- 서블릿 맵핑 태그속에 url-pattern 태그에 /로 되어 있다. 이것은 원래 웹서버 레벨에서 주고받았던 index.html까지도 디스패쳐 서블릿을 거치겠다는 의미이다. 즉, 모든 요청에 대해 디스패쳐 서블릿으로 보내겠다는것. 보낼 서블릿의 이름은 appServlet.
- 서블릿 태그에 appServlet이라는 이름의 서블릿을 작성해두었다. 스프링 프레임워크의 디스패쳐 서블릿 라이브라리가 심어져 있고, 요청이 넘어오면 param-value 태그에 적혀있는 경로의 xml로 간다. 위치와 이름을 무엇으로 써도 상관없으나, WEB-INF 밑에 두면 보안상 좋다.

### ✒︎ II. Servlet-context.xml

![network layer](/img/springLegacy/3_servletContext_xml.png)

- annotation-driven 태그는 어노테이션을 사용하겠다는것
- InternalResourceViewResolver 라이브러리를 통해 view로 쓸 디폴트 파일을 설정한다. 여기선 jsp 파일의 prefix와 suffix를 통해 정의함.
- component-scan은 스프링이 모든 디렉토리를 스캔하여 어노테이션을 찾으면 너무 느리니까 특정 디렉토리 한정으로 찾으라고 제한을 준것. 여기서 제한 준 부분은 아래 그림과 같다.  
  ![network layer](/img/springLegacy/4_controllerDir.png)

### ✒︎ IV. HomeController.java

![network layer](/img/springLegacy/5_Controller.png)

- Servlet-context.xml에서 제한 준 디렉토리에 있는 컨트롤러.
- Servlet-context.xml에서 어노테이션을 사용하기로 햤으므로 @Controller를 통해 컨트롤러 임을 스프링에 알려준다.
- @RequestMapping을 통해 리퀘스트의 메소드와 url에 매칭되는 메소드를 수행시켜준다.
- model.addAttribute()를 통해 request.setAttribute() 효과를 냈다.
- return으로 단지 스트링을 리턴했는데, Servlet-context.xml에서 설정한 기본 뷰 접두사, 접미사를 참고해 view로 포워딩 하는 효과를 내준다.

### ✒︎ V. home.jsp

![network layer](/img/springLegacy/6_viewJsp.png)

EL문법을 통해 컨트롤러에서 request.setProperty한 효과로 넘어온 데이터를 화면에 뿌린다.
