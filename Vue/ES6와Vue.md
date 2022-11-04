# Javascript ES6와 Vue.js

## Javascript 기본 지식
- 변수는 선언되면 기본적으로 window객체(최상위 객체)의 변수로 등록된다.
- 변수던, 함수던, js객체건 모두 객체이고, 모두 윈도우 객체에 변수로써 등록되거나 포인팅 된다.


## ES6 특징

템플릿 리터럴
화살표 함수
this
변수선언
모듈
클래스

### 1. 변수 선언
변수는 let, 상수는 const로 선언하자.
- let은 var와 다르게..
  - 중복 선언 불가
  - 변수 호이스팅 x
  - 스코프가 다를때는 중복 선언 할수 있음 (중복 선언인것 처럼 보이는것 뿐)
    - 함수는 당연하고, for 루프 안밖의 this가 다름. 
- const는 변경 할수 없는 값에 붙임
  - 주로 함수를 변수로 받을 때 붙여준다.
    - const f = function(){};
      - 참고로 이떄는 const가 붙은 변수로 받았으니 호이스팅이 안됨. 즉 선언 아래에서 호출 해야함.


### 2. 함수, 화살표함수, this
- 그냥 함수의 this는 윈도우이고, function 키워드가 붙은 메소드의 this는 js object이다.
- 메소드안에 새로운 함수를 정의할 수 있는데, function 키워드를 붙여 선언하면 inner method의 this는 Window 객체를 의미하고, 화살표 함수를 사용하여 inner 메소드를 생성하면 this가 현재 js object를 의미한다. inner 메소드 안에서 js 객체의 변수를 쓰고 싶어서 나온게 화살표 함수이다!
- concise method
  - function test(){} 하던걸 test(){}로 축약해서 메소드를 선언할 수 있다.
  - Vue 객체의 methods에 method를 추가할 때는 concise method 형태로 선언해야함

### 3. 비구조화 할당
- 비구조화 할당 - > 미리 변수 객체 만들어놓고, 필드랑 똑같은 이름의 변수를 선언해서 대입하면 자동 대입됨.

    ```javascript
    let student={
        name: "철수",
        grade: "A"
    };

    let {name, grade} = student;

    ```
    이렇게 변수명을 맞춰주면 값이 자동으로 맵핑된다.


### 4. 전개 연산자
iterable한 데이터를 나열해주거나, 깊은복사, 병합, for of 사용 (foreach 효과)등을 할 수 있음.
- 배열일때 -> 변수 = [...변수명]   
- 객체일 경우 -> 변수 = {...변수명}