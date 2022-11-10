# Vue를 CLI 환경에서 사용하기

## ✒︎ CLI 프로젝트 디렉토리 구조

![dir구조](../img/vue/vueCLI구조.png)

1. 전체 큰 판떼기 = public/index.html
2. 판떼기에 붙일 도메인 별로 나눈 큼직한 포스트잇 = src/views/\*.vue
3. 포스트잇 속의 조그만한 포스트잇 = src/components/{domain}/\*.vue

## ✒︎ 순서

1. 맨 처음에 index.html에 있는 `<div id="app"></div>`를 보고 App.vue를 간다.
2. App.vue

```html
<template>
  <div id="app">
    <!-- nav에서 클릭하면 라우팅되고 -->
    <nav>
      <router-link to="/">Home</router-link> | <router-link to="/about">About</router-link> |
      <router-link to="/board">게시판</router-link> |
      <router-link to="/todo">Todo</router-link>
    </nav>
    <!-- 뿌리는 위치는 여기에 뿌리겠다 -->
    <router-view />
  </div>
</template>
```

App.vue의 template 태그. router-link 태그를 통해 라우팅 한다.(이때 라우팅은 서버로 흐름을 보내지 않고, 마치 html의 a태그에 #을 붙여준것과 같이 클라이언트의 다른 위치로 흐름을 넘기는것을 말함)

router-link 태그에 의해 router 디렉토리의 index.js가 수행된다.

해당 위치의 내용을 router-view 태그 부분에 뿌린다.

3. router/index.js

```javascript

import HomeView from "../views/HomeView.vue";
import BoardView from "../views/BoardView.vue";

Vue.use(VueRouter);

const routes = [
  {
    path: "/",
    name: "home",
    component: HomeView,
  },
  {
    path: "/about",
    name: "about",
    component: () => import("../views/AboutView.vue"),
  },
];

...
```

만들어져 있는 routes 배열에 라우팅 정보를 설정한다.

- 각 객체 하나 하나를 도메인 별로 나눈 포스트잇이라 생각할것.
- path에 router-link 태그의 to 값을 똑같이 넣어준다.
- name은 필수는 아니고 이름 붙여준것
- component에 라우팅할 vue 컴포넌트(도메인별로 나눈 큼직한 컴포넌트, TodoView같이 {도메인+View}로 네이밍하며, 파스칼 네이밍 룰을 따름)를 지정 해 주는데, import해서 넣어준다. 이때 파스칼 네이밍 룰로 설정해줌. 혹은 화살표 함수 형태로 바로 import 해줘도 됨.

4. 지정한 큼직한 컴포넌트를 실제로 생성하기

- src밑에 views 디렉토리밑에 \*.vue 파일을 만들어 준다.

```javascript
<template>
  <div class="todo">
    <h1>투두</h1>
    <div>
      <!-- 자식 컴포넌트들 @addTodo / 바인딩으로 자식에게 넘기고, 자식에선 props로 받는다  -->
      <todo-header></todo-header>
      <todo-input @addTodo="addTodo"></todo-input>
      <todo-list :todos="todos"></todo-list>
      <todo-footer :todos="todos"></todo-footer>
    </div>
  </div>
</template>
```

큼직한 포스트잇에 template을 하나 파고 거기다 내가 사용할 자식 컴포넌트들을 태그로써 선언한다. 내가 사용할 자식 컴포넌트는 /src/components/{domain}/TodoHeader.vue와 같이 생성할것임. import, export는 파스칼 네이밍룰로 정했으니 TodoHeader로써 import 해옴. 이때 태그로써 선언하는 곳은 TodoHeader 이니까 todo-header 형태로 케밥 네이밍룰로써 설정한다.

```javascript
<script>
import TodoHeader from "../components/todo/TodoHeader.vue";
import TodoInput from "../components/todo/TodoInput.vue";
import TodoList from "../components/todo/TodoList.vue";
import TodoFooter from "../components/todo/TodoFooter.vue";

export default {
  name: "TodoView",
  components: {
    // 자식 쓸거면 여다 등록 해야함
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todos: [], // 자식인 헤더에서 온 데이터를 여기다 푸쉬 후 다른 자식인 list에게 보내겠다는 의도
    };
  },
  methods: {
    addTodo(item) {
      console.log(item);
      this.todos.push(item);
    },
  },
};
</script>
```

위에서 router/index.js의 const routes 배열에 큰 포스트잇을 설정하는 객체를 넣어주었었고, 거기의 component 자리에 TodoView를 import 해 주었음. 따라서 여기서도 export 문이 필요함.  
뷰 객체 형태로 써주는데, 포인트는 components key의 value값으로 내가 사용할 자식 컴포넌트들을 import해서 파스칼 네이밍룰 형태로 넣어주는 부분이다.

5. 큼직한 포스트잇 컴포넌트에 붙일 잔바리 컴포넌트(자식 컴포넌트)를 실제 생성. 이때 게시판이면 게시판, todo면 todo 이렇게 디렉토리를 세분화 해서 구성하면 좋다.

#

## ✒︎ Todo Component 구현 핵심 내용 정리

### ❆ TodoView - 큼직한 포스트잇

1. `<template>`태그속에 내가 쓸 잔바리 포스트잇을 모아둔다. 이때 잔바리들은 케밥 네이밍 룰을 사용함

```javascript
<template>
  <div class="todo">
    <h1>투두</h1>
    <div>
      <!-- 자식 컴포넌트들 @addTodo / 바인딩으로 자식에게 넘기고, 자식에선 props로 받는다  -->
      <todo-header></todo-header>
      <todo-input @addTodo="addTodo"></todo-input>
      <todo-list :todos="todos"></todo-list>
      <todo-footer :todos="todos"></todo-footer>
    </div>
  </div>
</template>
```

자식에게 데이터를 줄때는 바인딩으로 주고, 자식으로부터 데이터를 가져올때는 v-on으로 가져온다.

2. 스크립트에는 자식으로 쓸 컴포넌트들을 import 해오고 components에다가 등록한다.

```javascript
<script>
import TodoHeader from "../components/todo/TodoHeader.vue";
import TodoInput from "../components/todo/TodoInput.vue";
import TodoList from "../components/todo/TodoList.vue";
import TodoFooter from "../components/todo/TodoFooter.vue";

export default {
  name: "TodoView",
  components: {
    // 자식 쓸거면 여다 등록 해야함
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todos: [], // 자식인 헤더에서 온 데이터를 여기다 푸쉬 후 다른 자식인 list에게 보내겠다는 의도
    };
  },
  methods: {
    addTodo(item) {
      console.log(item);
      this.todos.push(item);  // 실제 addTodo 구현은 여기서
    },
  },
};
</script>
```

자식으로부터 데이터를 가져와서 로직을 수행한 뒤, 데이터를 자식들에게 넘겨준다.

- 자식으로 부터 데이터를 가져올때는 자식에서 $.emit("evt명", data) 이렇게 emit으로 내뿜고, 부모에서 캐치를 `<todo-input @evt명="이벤트핸들러"></todo-input>` 여기서 위 같은 형태로 캐치한다. 데이터는 이벤트 핸들러 구현부의

```javascript
addTodo(item) {
  console.log(item);
  this.todos.push(item);  // 실제 addTodo 구현은 여기서
},
```

이부분의 item 인자를 둠으로써 캐치할 수 있음.

### ❆ TodoHeader

- 제목 + export로 구성

### ❆ TodoInput

- 입력 테스트 -> vmodel로 양방향 맵핑
- `@keyup.enter="이벤트 핸들러"`로 엔터시 특정 메소드로 가게 하고,
- 그 메소드에서 부모에게 입력 데이터를 보내기 위한 `this.$emit("addTodo", item);`를 코딩

### ❆ TodoList

- vfor로 부모로 부터 가져온 todos를 반복 출력한다.
- 이때 `props: ["todos"],` 형태로

### ❆ TodoFooter

- true 건수 확인: 부모로부터 가져온 todos를 computed를 통해 변화를 감시하고, 변화시 filter를 통해서 done == true 인것의 갯수를 센 후 머스타치로 출력

#

## ✒︎ Board Component 구현 핵심 내용 정리

### ❆ BoardView

```javascript
<template>
  <div class="board">
    <h1>게시판</h1>
    <!-- <board-header></board-header> -->
    <board-list v-if="showBoardList" :boardList="boardList" @goWriteForm="writeForm"></board-list>
    <board-write v-if="showBoardWrite" @showBoard="writeList"></board-write>
    <!-- <board-footer></board-footer> -->
  </div>
</template>
```

- v-if를 통해 게시판 출력 화면과 게시글 입력 화면을 번갈아 출력한다.
- 부모에서 자식에게 데이터를 넘겨줄땐 바인딩! 자식에서 부모로 데이터를 넘겨줄땐 emit!

```javascript

<script>
import BoardList from "../components/board/BoardList.vue";
import BoardWrite from "../components/board/BoardWrite.vue";

export default {
  name: "BoardView",
  components: {
    BoardList,
    BoardWrite,
  },
  data() {
    return {
      no: "",
      title: "",
      contents: "",
      showBoardList: true,
      showBoardWrite: false,
      boardList: [
        { no: 1, title: "t1", contents: "aaa" },
        { no: 2, title: "t2", contents: "bbb" },
      ],
    };
  },
  methods: {
    writeList(usrInput) {
      let curLen = this.boardList.length + 1;
      this.boardList.push({ no: curLen, ...usrInput });
      this.showBoardList = true;
      this.showBoardWrite = false;
    },
    writeForm() {
      this.showBoardList = false;
      this.showBoardWrite = true;
    },
  },
};
</script>
```

- components에 사용할 자식 컴포넌트를 임포트 해서 등록한다.
- writeList(usrInput) 메소드를 통해 자식(BoardWrite)에서 emit으로 넘어온 게시판 행을 게시글 리스트에 추가함

### ❆ BoardList

- 맨 처음에 BoardView의 showBoardList: true, showBoardWrite: false 데이터로 인해 이 컴포넌트가 먼저 출력됨
- 부모로부터 bind로 전해받은 boardList를 props으로 등록하고, v-for로 출력한다.
- 버튼을 하나 만들어서 wirteform 컴포넌트를 출력하라는 이벤트를 만드는 메소드를 만들고, 거기에 emit으로 goWriteForm 시그널을 부모로 보낸다

### ❆ BoardWrite

- 부모가 goWriteForm 시그널을 받은 후 showBoardList: false, showBoardWrite: true를 통해 작성 컴포넌트를 출력한다.
- 버튼을 하나 만들어서 생성한 데이터를 emit으로 부모에 올려줄 메소드를 만들고, showBoard라는 이름의 이벤트를 작성한 게시글 객체와 함께 부모로 넘겨준다.
- 부모는 넘겨받은 데이터를 게시글 리스트에 push해주고 showBoardList: true, showBoardWrite: false로 다시 변경해주어 게시판 리스트를 출력해준다.

## ✒︎ Board Component 구현 - Vue의 라우팅으로 화면전환 해보기

### ❆ /src/router/index.js

라우팅 설정 부분 수정 -> redirect 지정과 children 지정.

```javascript
{
    path: "/board",
    name: "board",
    component: BoardView,
    redirect: "/board/list",
    children: [
      {
        path: "list",
        name: "boardlist",
        component: () => import("@/components/board/BoardList"),
      },
      {
        path: "write",
        name: "boardwrite",
        component: () => import("@/components/board/BoardWrite"),
      },
    ],
  },
```

- children을 통해 라우터 설정부에서 자식 컴포넌트들을 임포트 해준다

### ❆ /src/views/BoardView.vue

- 라우팅을 통해 화면을 전환할것이므로 `<board-list>`와 `<board-write>` 를 제거해준다.
- v-if 디렉티브를 쓰지 않을 것 이므로, boolean으로 쓰던 변수들을 제거한다
- 서버에서 그때그때 데이터를 조회하지 않고, 클라이언트에서 데이터를 전달하는 형태를 만들어야 한다.
- board list를 띄우는 일은 boardlist 컴포넌트에게 위임하기로 한다. 관련 변수, 함수는 view에서 제거한다.
- BoardView에서 BoardList로 리다이렉션을 하게하여 기본 출력 화면을 BoardList로 설정한다.

### ❆ /src/components/board/BoardList.vue

```javascript

// 라우팅시 객체가 새로 찍히는데, 초기화 방지를 위해 뺌
const boardListRepo = [
        { no: 1, title: "t1", contents: "aaa" },
        { no: 2, title: "t2", contents: "bbb" },
];

export default {
  name: "BoardList",

  data() {
    return {
      boardList: boardListRepo,
    }
  },
```

- BoardList로 흐름이 전달되면, 객체가 새로 찍히는데, 이때 초기화 되는것을 방지하기 위해 export default 외부로 변수를 빼준다.
- 그리고 나서 boardList라는 이름으로 게시판 정보를 저장한다.

```javascript
methods: {
  writeBoard() {
    // this.$emit("goWriteForm");
    this.$router.push({name:'boardwrite'})
  },
},
```

- 글작성 버튼을 누르면 name이 boardwrite인 곳으로 routs/index.js를 참고하여 화면을 전환시킨다.

### ❆ /src/components/board/BoardWrite.vue

```javascript
methods: {
  writeBoard() {
    this.$router.push({ name: "boardlist", params: { newRow: { title: this.title, contents: this.contents } } });
  },
},
```

- 사용자로부터 입력받은 데이터를 다시 boardlist라는 이름인 곳으로 routs/index.js를 참고하여 화면을 전환시킨다.
- 이때 params 인자에 새로 생성된 row 정보를 js 객체로 구성해서 같이 태워보낸다.

### ❆ 다시 /src/components/board/BoardList.vue

```javascript
created() {
  // 라우팅 될때마다 뷰 객체 생성
  let newRow = this.$route.params.newRow;
  if (Object.keys(newRow).length > 0) {
    this.boardList.push({ no: boardListRepo.length, ...newRow })
  }
},
```

- 전달되어 들어온 new row를 꺼내 쓰는데, 객체가 비어있는지 체크를 해서 정보가 들어있을때만 게시글 리스트에 push 해준다.
