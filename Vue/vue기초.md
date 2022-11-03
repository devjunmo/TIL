# Vue.js 기초

### ✒︎ MVVM 패턴과 Vue.js

- M(JS 데이터) V(DOM 구조의 html) C(JS function)
- 여기서 Controller 부분을 바닐라 JS로 할수도, 제이쿼리로 할수도, Vue.js로 할수도 있는것.
- MVVM은 새로운게 아니라 단지 M + V + (VM = Vue Model = controller)일 뿐이다.


### ✒︎ 바닐라 자바스크립트 말고 Vue.js를 쓰면 좋은 이유?
- 컴포넌트 기반으로 코드를 작성할 수 있어 가독성과 재사용성이 좋음
- document.querySelector.addEvtL(””, f(){}).. 하던걸 Vue가 편하게 해결해줌
- 마치 JSTL쓰듯 직관적으로 코딩 가능 


### ✒︎ Vue.js 기초 문법

- Vue instance
  - new Vue({ el:"#app", data:{count: 0}, }); 를 통해 vue 객체를 생성한다. 이때 el을 통해 내가 링크하고 싶은 html 태그의 아이디를 

- 데이터 바인딩
  - Mustache 구문 (이중 중괄호)을 통해 el로 링크된 vue 객체의 data 필드의 key를 바로 사용 가능.
- 디렉티브: v- 접두사가 있는 특수 속성
  - v-model
    - 쓰는 이유: 양방향 바인딩 처리를 위해 사용. 인풋에 v-model="vue객체의 필드명" 써주면, 들어온 인풋값이 자동으로 "vue객체의 필드명"의 값으로 맵핑됨.
  - v-bind(:)
    - 엘리먼트 속성과 바인딩 처리를 위해 사용. 약어를 사용할 수 있는데 ':'로 줄일 수 있음. 
    - ```<div :id="vue객체의 필드명">``` 써주면 특정 태그의 id 옵션에 vue객체의 해당 필드 값이 맵핑됨
  - v-show
    - vue 객체의 필드에 boolean값을 넣어주고, v-show="isShow"같이 필드명 넣어주면, true일때 보여지고, false일때 안보여짐
  - v-if / v-else-if / v-else
    - ```<span v-if="age < 10">```같이 써서 조건에 따라 html 태그가 생길 수 있음.
    - 마치 jstl쓰듯 쓸수있음
  - v-for
    - 배열이나 객체의 반복에 사용
    - v-for="user in users" 같이 사용
    ```html
    <ul>
        <li v-for="(region, index) in ssafy" v-if="region.count === count">
          지역 : {{region.name}}, {{region.count}}개반
        </li>
    </ul>
    ```
    이렇게 반복해서 html을 직관적으로 반복해서 짤수있음.
