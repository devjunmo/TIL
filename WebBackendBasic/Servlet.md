# Servlet

## Servlet이란?

사용자 요구에 맞춰 동적으로 반응하는 페이지을 만들기 위한 자바 프로그램.

## Servlet Container란?

서블릿 컨테이너는 웹컨테이너라고도 하며, 서블릿 생성부터 실행, 소멸까지 관리해주는 역할을 한다.
웹 컨테이너 = 서블릿 컨테이너 = 서블릿, JSP 엔진 이라고 생각하자.

## Servlet 장점

- request당 thread로 동작한다.
  - 다른 언어들은 리퀘스트당 프로세스로 동작.
  - 자바는 메모리의 스택 영역을 제외한 힙 영역은 쓰레드가 공유하여 사용한다. 때문에 가볍게 멀티리퀘스트 처리가 가능
- Platform independent
  - 실행 플랫폼에 java ee 구현체인 웹 컨테이너(http는 전혀 모르고 서블릿만 앎. jsp는 서블릿으로 바꾸고 컴파일 해서 서블릿 엔진에 넣어줌)와 웹 서버(http 프로토콜만 상대함)가 있으면 어디서든 어떤 플랫폼에서도 동작 가능.

## Servlet 생명주기

웹 서버에서 처리 하지 못하는 동적 요청이 들어오면 웹 컨테이너에 의해 서블릿 객체가 생성된다. 객체가 생성되면 아래 메소드들이 호출된다.

- init()
  - 첫 서블릿 요청때 서블릿이 생성되면서 호출된다.
- service():
  - init() 호출 후 또는 웹브라우저 refresh시 호출됨.
  - service 메소드가 호출되면 [쓰레드객체, request객체, response 객체] 한 세트가 생김
  - 생성된 쓰레드 객체의 run 메소드에서 서비스 메소드가 실행되며, run 메소드가 끝나면 생겨난 객체 세트 세개가 메모리에서 GC에 의해 수거됨.
  - 서비스 메소드에서 요청 방식에 따라 doGet, doPost 메소드 호출.
- destroy()
  - 서블릿 내용 갱신 또는 종료시 호출됨

계속..
