# REST API

## ✒︎ Open API (Open Application Programming Interface)

OPEN API와 함께 거론되는 기술이 REST이며, 대부분의 OPEN API는 REST방식으로 지원.

- Open API = 프로그래밍에서 사용할 수 있는 개방된 Interface
- 공공 포털사이트등에서 외부 응용 프로그램에서 사용 할 수 있도록 OPEN API를 제공하며, 대부분 REST방식으로 지원한다.

## ✒︎ URI(Uniform Resource Identifier) = URL(Uniform Resource Locator) + Uniform Resource Name (URN).

- URI의 핵심은 리소스 식별이다.
- 회원 조회, 회원 삭제, 회원 변경 ... 이러한 것들 중 리소스는 "회원"이라는 개념이다.
- 즉, 좋은 URI 형태는 /member_search 가 아니라 /member/{id} 라는것이다.
- 조회, 삭제, 변경은 리소스의 행동인것이다. 그 행동을 HTTP Method(GET, POST, PUT, DELETE)로 나타낼 수 있다는것!

## ✒︎ REST (Representational State Transfer)

REST는 하나의 URI는 하나의 고유한 리소스(Resource)를 대표하도록 설계된다는 개념에 전송방식을 결합해서 원하는 작업을 지정한다.
즉, REST = URI + GET(read) / POST(create) / PUT(update) / DELETE(delete)로 작업을 지정.  
정리하면 REST는 HTTP URI를 통해 제어할 자원(Resource)를 명시하고, HTTP Method(GET, POST, PUT, DELETE)을 통해 해당 자원(Resource)를 제어하는 명령을 내리는 방식의 아키텍처이다.

## ✒︎ REST 예시

관리자 페이지의 유저 정보 관리 페이지를 생각해보자. 여러 기능이 있겠지만, 그 중에서 1. 특정 회원의 상세정보를 가져오는 코드와 2. 특정 회원을 삭제하는 코드를 생각해보자.

- '특정 회원'에 대한 상세조회 / 삭제에서 리소스는 회원, 메소드는 상세조회, 삭제이다.
- 만약 메소드를 빼고 생각한다면, 상세조회와 삭제 모두 /admin/member의 동일한 URI인 것이다.
- 서버에 동일한 URI만을 주는것 만으로는 다른 일을 시킬 수 없으니, 두개를 구별하는 용도로 HTTP Method를 활용하는 것이다.

자바스크립트에서

```javascript
// 회원 조회 - ${root}/admin/user/${id} URI + GET 방식 행동
fetch(`${root}/admin/user/${id}`).then().then();

// 회원 삭제 - ${root}/admin/user/${id} URI + DELETE 방식 행동
let config = {
  method: "DELETE",
  headers: { "Content-Type": "application/json" },
};
fetch(`${root}/admin/user/${id}`, config);
```

컨트롤러에서

```java
@DeleteMapping("/user/{myuserid}") // userid는 내가 붙이고 싶은 이름으로 써도 됨
	public ResponseEntity<?> userDelete(@PathVariable("myuserid") String userId)

@GetMapping("/user/{myuserid}") // userid는 내가 붙이고 싶은 이름으로 써도 됨
	public ResponseEntity<?> userView(@PathVariable("myuserid") String userId)
```

이런식으로 URI + Method를 통해 특정 자원을 제어하는 명령을 내릴 수 있다는 것!
