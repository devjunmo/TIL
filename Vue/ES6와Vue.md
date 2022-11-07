# Javascript ES6와 Vue.js

## Javascript 기본 지식
- 변수는 선언되면 기본적으로 window객체(최상위 객체)의 변수로 등록된다.
- 변수던, 함수던, js객체건 모두 객체이고, 모두 윈도우 객체에 변수로써 등록되거나 포인팅 된다.


## ES6 특징

### 1. 변수 선언 (var, let, const)
변수는 let, 상수는 const로 선언하자.
- let은 var와 다르게..
  - 중복 선언 불가
  - 변수 호이스팅 x
  - 블록 스코프를 가진다.(var는 함수 스코프. 블록스코프 해당 없음) 블록단위로 스코프 내부에서는 지역변수로 선언 가능. 
    - 함수는 당연하고, for 루프, if문등 블록 안밖의 this가 다름.
- const는 변경 할수 없는 값에 붙임
  - 주로 함수를 변수로 받을 때 붙여준다.
    - const f = function(){};
      - 참고로 이떄는 const가 붙은 변수로 받았으니 호이스팅이 안됨. 즉 선언 아래에서 호출 해야함.
  - const로 선언한 js 오브젝트의 필드는 변경할 수 있다.


### 2. 함수, 화살표함수, Concise method, this
- 그냥 함수의 this는 윈도우이고, function 키워드가 붙은 메소드의 this는 js object이다.
- 메소드안에 새로운 함수를 정의할 수 있는데, function 키워드를 붙여 선언하면 inner method의 this는 Window 객체를 의미하고, 화살표 함수를 사용하여 inner 메소드를 생성하면 this가 현재 js object를 의미한다. -> inner 메소드 안에서 js 객체의 변수를 쓰고 싶어서 나온게 화살표 함수이다!
- concise method
  - f_name: function test(){} 하던걸 test(){}로 축약해서 메소드를 선언할 수 있다.
  - Vue 객체의 methods에 method를 추가할 때는 concise method 형태로 선언해야함


### 3. Property Shorthand
객체 정의시 key 이름과 value 변수명이 같으면 value를 생략할 수 있다.
```javascript
let id="jun";
let pw="1234";
let name="junmo";

let member = {
  id, pw, name
};

// 결과: {id: "jun", pw: "1234", name: "junmo"}
```


### 4. Destructuring Assignment (비구조화(구조 분해) 할당)
- 비구조화 할당: 배열이나 객체에 입력된 값을 개별 변수에 할당하는 간편한 방식 제공 

    ```javascript

    // 배열일 때 개별 변수에 간편히 할당
    let lst = ["a", "b", "c"]
    const [a, b, c] = lst;

    // 객체일 때 개별 변수에 간편히 할당 
    let student={
        name: "철수",
        grade: "A"
    };

    let {name, grade} = student; // 할당 받을 변수명과 객체 필드명이 같으면 property shorthand 가능
    let {name: usr_name, grade: usr_grade} = student; // 다르면 중괄호에 왼쪽에 객체필드: 오른쪽에 받을 변수 갖춰주고 할당 


    // 함수 인자로 객체를 받을때 이렇게도 됨
    function test({name, grade}){ // 이렇게 맞춰주면 자동 맵핑됨 
       ...
    }

    test(student); 

    ```


### 5. Spread syntax (전개 구문)
iterable한 데이터를 나열해주거나, 배열복사, 병합, for of(foreach 효과)등을 할 수 있음.
- 배열일때
  - 변수 = [...arr]을 통해 배열을 복사할 수 있음
  - 변수 = ["a", ...arr]과 같이 새로운 요소를 추가할 수도 있음
  - function a([a, b, c]) {} 함수에 a([1, 2, 3, 4]); 이렇게 인자로 줄 수 있음(이때 4 drop됨). 
- 객체일 경우
  - 변수 = {...obj}과 같이 객체를 복사할 수 있음
  - 변수 = {...obj, {id:"jun"}}과 같이 객체에 요소를 추가할 수 있음



### 6. Default Parameter
```javascript
function test(id="test_id"){
  console.log(id);
}

test(); // 인자를 주지 않으면 사전에 지정된 test_id로 인자가 설정됨 
```


### 7. Template String
파이썬의 f string 같은것
```javascript

let name = "junmo"

console.log(`안녕하세요 ${name}님~`) // 안녕하세요 junmo님~ 출력됨 
```


### 8. Module
- Javascript를 모듈화 하여 재활용하기.
- html의 script 태그에서 src="my.js"를 통해 html에 javascript 전체를 붙이던것을 export, import를 통해 필요한 모듈화 코드를 불러와서 사용.

먼저 자바스크립트 파일을 생성한다.

```javascript
// 객체의 리터럴 방식으로 객체 생성 
const member={
    name:'전은수',
    age:38
}

let i = 10;
const i2 = 20;

export default function f1() {
    console.log("f1 입니다.");
}

export { member, i, i2 }; // 한번에 방출도 가능 
```
- export를 통해 외부에서 사용할 1. 변수 2. 객체 3. 함수를 설정한다.

```html
<script type="module">
    import ffff, {member, i, i2} from "./jes01.js";

    console.log(member.name);
    console.log(i)
    ffff(); // 함수 호출도 가능 

</script>
```
- script 태그 안에서 import를 통해 모듈을 가져온다.
- 이때 export한 변수명과 import할 변수명을 똑같이 맞춰 줘야 한다.
- 이부분이 불편해서 export default가 나왔다.
  - export default는 한 자바스크립트 파일에 하나의 모듈만 설정할 수 있다.
    - 딱 하나 뿐이므로 import할때 어떤 이름으로 import해도 상관없다.
    - 이것을 응용하면 함수를 하나만 만들어서 export default로 두고 사용할 수 있는데, 이렇게 하면 사용성이 편하다. -> single file component!

- export 하는곳에서는 실행되는 구문이 있으면 불편하다. 때문에 해당 파일은 선언만 모아두고, 실제 import하여 사용하는 쪽에서 실행 구문을 사용하는것이 좋다. -> Vue의 Component 이해의 핵심 





