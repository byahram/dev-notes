---
title: "[vue] 코딩애플 - Vue3 완벽 가이드"
date: "2024-03-04"
category: "vue"
tags: ["vue", "coding apple"]
---

# [vue] 코딩애플 - Vue3 완벽 가이드

<https://codingapple.com/course/vue-js/>

<br />

## Table of Contents

- [Part 1 : 부동산 쇼핑몰](#part-1--부동산-쇼핑몰)
- [Part 2 : blog 레이아웃과 라우터](#part-2--blog-레이아웃과-라우터)
- [Part 3 : 인스타그램 만들기](#part-3--인스타그램-만들기)

<br />

## Part 1 : 부동산 쇼핑몰

<https://github.com/byahram/vue-starter/tree/master/vuedongsan>

### 1. HTML에 데이터 꽂아넣는 Vue 데이터바인딩 문법

```javascript
// JavaScript에서 자바스크립트 변수나 데이터를 HTML에 꽂아 넣는 데이터 바인딩
document.getElementById(어쩌구).innerHTML = 데이터;
```

```html
<!-- 
    Vue에서는 
    1. 데이터 보관하고
    2. {{ 데이터 }} 문법으로 HTML 중간에 꽂아 넣으면 된다
  -->

<template>
  <div>
    <h4 :style="스타일">XX 원룸</h4>
    <p>{{ price1 }} 만원</p>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        price1: 60,
        스타일: "color:red",
      };
    },
  };
</script>
```

### 2. Vue 반복문 v-for

```html
<!-- 1. -->
<div class="menu">
  <a v-for="작명 in 3" :key="작명">Home</a>
</div>

<!-- 2. -->
<div class="menu">
  <a v-for="작명 in 메뉴들" :key="작명">Home</a>
</div>

<!-- 3. -->
<div class="menu">
  <a v-for="(작명,i) in 메뉴들" :key="i"> {{ 작명 }}</a>
</div>
```

### 3. Vue 이벤트 핸들러로 click 감지하기

```html
<div>
  <h4>{{products[0]}}</h4>
  <p>50만원</p>
  <button @click="신고수++">허위매물신고</button>
  <button @click="increase">허위매물신고</button>
  <span>신고수 : {신고수}</span>
</div>

<script>
  data(){
    return {
      신고수 : 0,
    },
  }

  methods : {
    increase(){
      this.신고수 += 1
    }
  }
</script>
```

### 4. v-if 와 모달창 만들기 (Vue에서 동적인 UI 만드는 법)

#### 동적인 UI 만드는 Step

```html
<!-- 1. 현재 HTML UI의 상태를 데이터로 저장해둠 (지금 보이는지 안보이는지 이런거)  -->
<script>
  data(){
    return {
      모달창열렸니 : true,
    }
  }
</script>

<!-- 2. 그 상태에 따라 HTML UI을 보여줄지 말지를 Vue문법으로 작성함  -->
<div class="black-bg" v-if="모달창열렸니 == true">
  <div class="white-bg">
    <h4>상세페이지</h4>
    <p>상세페이지내용임</p>
  </div>
</div>
```

### 5. import/export 문법을 이용해서 다른 js파일 데이터 가져오기

- 어떤 js 파일에서 만든 변수나 자료를 다른 js 파일에서 사용하고 싶은 경우 export와 import 문법 사용
- 다른파일에서 export 하셔야 다른 파일에서 import를 할 수 있다.

```javascript
// (oneroom.js)
export default 위에있던원룸데이터

// (App.vue)
<div>
  <img src="">
  <h4>{{원룸들[0].title}}</h4>
  <p>{{원룸들[0].price}}</p>
</div>

<script>
  import data from './oneroom.js파일경로'

  data(){
    return {
      원룸들 : data
    }
  }
</script>
```

### 6. 부모가 가진 데이터를 자식이쓰고 싶으면 Props

하위컴포넌트로 데이터를 전송하려면 props라는 문법으로 Modal.vue에 보내면 된다.

(자식이 부모가 가진 데이터 쓰려면 props로 전송해주어야 사용 가능)

```html
<!-- (App.vue) -->
<!-- <Modal :작명="하단의데이터이름" /> -->
<Modal :원룸들="원룸들" />

<!-- (Modal.vue) -->
<!-- props : {} 열고 거기다가 아까 작명한 { 데이터이름 : 자료형 } -->
<script>
  export default {
    name: "Modal",
    props: {
      원룸들: Array,
    },
  };
</script>
```

**props 보내는 여러가지 방법**

```html
<!-- 1. 각각 array, object, 숫자, 문자보내기 -->
<Discount :데이터이름="[1,2,3]" />
<Discount :데이터이름="{ age:20 }" />
<Discount :데이터이름="100" />
<Discount 데이터이름="안녕하쇼" />

<!-- 2. 오브젝트 : {name : 'kim', age : 20} 이라는 object 자료를 name, age 각각 props로 보내고 싶으면  -->
<Discount :데이터이름="오브젝트.name" :데이터이름="오브젝트.age" />

<!-- 3. object 자료 통째로 보내기 -->
<Discount v-bind="오브젝트명" />
```

```html
<!-- App.vue -->
<Card :원룸="원룸들[0]" />

<!-- Card.vue -->
<template>
  <div>
    <img :src="원룸.image" class="room-img" />
    <h4>{{원룸.title}}</h4>
    <p>{{원룸.price}} 원</p>
  </div>
</template>

<script>
  export default {
    props: {
      원룸: Object,
    },
  };
</script>
```

### 7. Custom Event

자식이 부모가 가진 데이터를 바꾸고 싶으면 자식컴포넌트는 부모에게 custom event로 메세지를 줘야함

**custom event 문법을 이용한 부모가 가진 데이터 변경 방법**

```html
<!-- 1. 자식은 $emit(작명, 전달할자료) 이렇게 부모에게 메세지 -->
<!-- (Card.vue) -->
<template>
  <div>
    <img :src="a.image" class="room-img" />
    <h4 @click="$emit('openModal', 원룸.id)">{{a.title}}</h4>
    <p>{{a.price}} 원</p>
  </div>
</template>

<!-- 2. 부모는 @작명="데이터변경하는JS코드" 이렇게 메세지를 수신해서 원하는 데이터를 변경하도록 코드 생성 -->
<!-- (App.vue) -->
<!-- 부모는 $event라는 변수를 쓰면 그 보낸 자료가 담겨있다. -->
<Card @openModal="모달창열렸니 = true; 누른거 = $event" />
```

### 8. 사용자의 input을 받는 법 (v-model)

사용자가 입력한 정보를 data로 저장하려면

```html
<!-- 1. -->
<template>
  <input @input="month = $event.target.value" />
</template>
<script>
  export default {
    data() {
      return {
        month: 0,
      };
    },
  };
</script>

<!-- 2. -->
<!-- v-model은 "여기 입력된 값을 data로 바로 저장해주세요~" 라는 문법 -->
<input v-model="month" />
```

### 9. watcher로 데이터 감시하는 법

month라는 데이터 항목에 숫자가 아니라 문자가 들어오면 "숫자만 입력하라"는 alert()창을 띄워주고 month의 초기값을 1로 다시 설정

watch : {} 라는 항목을 신설해서 거기다가 작성하면 어떤 데이터를 계속 감시하는 역할을 한다.

```html
<!-- 특정 데이터가 변경될 때마다 실행되는 코드 -->
<input v-model="month" />

<script>
  export default {
    data() {
      return {
        month: 1,
      };
    },
    watch: {
      month(a) {
        if (isNaN(a) == true) {
          alert("문자입력하지마라");
          this.month = 1;
        }
      },
    },
  };
</script>
```

### 10. \<transition> 태그를 이용하여 UI 애니메이션 주는 법

```html
<transition name="작명"></transition>
```

```css
.작명-enter-from { 애니메이션 동작 전 상태 }
.작명-enter-active { 애니메이션 동작 중 상태, 대부분 transition 이런거 }
.작명-enter-to { 애니메이션 동작 후 상태 }
```

### 11. 상품정렬기능과 데이터 원본 보존

```javascript
// array 자료가 가나다 순으로 정렬
var array = [2, 5, 1];
array.sort();

// array 자료가 숫자 123 순으로 정렬
var array = [2, 5, 1];
array.sort(function (a, b) {
  return a - b;
});
```

```html
<button @click="priceSort()">가격순정렬</button>
<button @click="sortBack()">되돌리기</button>

<script>
    data(){
      return {
        원룸들오리지널 : [...data],
        원룸들 : [...data]
      }
    },
    methods : {
    priceSort(){
      this.원룸들.sort(function(a,b){
        return a - b
      })
    },
    sortBack(){
      this.원룸들 = [...this.원룸들오리지널]
    }
  }
</script>
```

### 12. Vue의 라이프사이클

<div align="center" style="margin-bottom: 2rem;">
  <img src="./images/20240304_1.png" alt="missing" width="80%" />
</div>

1. 컴포넌트를 보여줄 때 create -> mount 이 단계로 생성된다. create는 데이터생성, mount는 index.html 파일에 장착 이렇게 생각하시면 됩니다.
2. 데이터가 바뀌어서 컴포넌트가 재렌더링될 때는 update 단계를 거치고
3. 다른페이지로 이동하거나 그럴 때 컴포넌트가 삭제될 때는 unmount 라는 단계를 거친다.

- 이 단계들 중간중간에 코드를 실행시키고 싶을 때가 있습니다. 예를 들면 mount 되기 전에 뭔가 ajax 요청으로 서버에서 데이터를 가져오거나 update 되기 전에 뭔가 코드를 실행해서 데이터를 검증해보거나 이런 식으로. 그럴 때 lifecycle hook을 골라서 쓰면 된다.

```html
<!-- 메인페이지 방문하자마자 30%라고 적힌 할인 문구가 1초마다 1%씩 감소하려면? -->
<!-- App.vue -->
<template>
  <p>지금 결제하면 {{amount}}% 할인</p>
</template>

<script>
  data(){
    return {
      amount : 30,
    }
  },
  mounted(){
    setInterval(()=>{
        this.amount--;
    }, 1000);
  }
</script>
```

```html
<!-- 모달창 내에 input이 있는데 여기에 2를 기입했을 때 알림창 alert() 을 띄우려면? (watcher 말고 lifecycle hook 이용) -->
<script>
  beforeUpdate(){
    if (this.month == 2){
      alert('2개월은 너무 적음.. 안팝니다')
    }
  }
</script>
```

<br />

## Part 2 : blog 레이아웃과 라우터

<https://github.com/byahram/vue-starter/tree/master/blog>

### 1. 뷰에서 Bootstrap 4, 5 npm으로 설치

```cmd
<!-- 터미널에 다음 명령어를 입력 -->

npm install bootstrap@5 (5버전 설치시)
또는
npm install bootstrap@4 jquery popper.js (4버전 설치시)
```

```css
/* main.js 파일에 다음 코드를 추가 */
import 'bootstrap'
import 'bootstrap/dist/css/bootstrap.min.css'
```

### 2. vue-router 설치와 기본 라우팅

```
// vue-router 4버전을 설치
npm install vue-router@4
```

```javascript
// router.js
import { createWebHistory, createRouter } from "vue-router";
import List from './components/List.vue';

const routes = [
  {
    path: '/list',
    component: List,
  },
  {
    path: '/경로',
    component: 위에서 import 해온 컴포넌트
  }
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

```javascript
// main.js
import router from "./router";
createApp(App).use(router).mount("#app");
```

```html
<!-- props 전송은 <router-view :블로그글="블로그글"></router-view> -->
<router-link to="/list">이동하기</router-link>
```

### 3. 상세페이지 200만개 만들기 (URL 파라미터)

```javascript
// router.js
const routes = [
  {
    path: "/detail/:id",
    component: Detail,
  },
];
```

```html
<!-- 컴포넌트 안에서 URL 파라미터에 뭐가 써있는지 출력하고 싶으면  -->
{{ $route.params.파라미터명 }}
```

### 4. Nested routes & push 함수

<https://next.router.vuejs.org/>

```javascript
// router.js
const routes = [
  {
    path: "/detail/:id",
    component: Detail,
    children: [
      { path: "author", component: Author },
      { path: "comment", component: Comment },
    ],
  },
];
```

```js
$router.push("/detail/0"); // /detail/0으로 이동
$router.go(-1); // 뒤로가기
```

### 5. 라우터 나머지 기능들 (hash mode, guards)

#### **1. Hash mode vs HTML5 mode**

```javascript
// HTML5 mode
import { createRouter, createWebHistory } from "vue-router";

const router = [];
const router = createRouter({
  history: createWebHistory(),
  routes,
});

/**
 * 누군가 /detail 이라고 URL란에 입력하면 이건 서버에 /detail 페이지를 요청해주세요~ 라는 뜻이다
 * Vue가 라우팅을 해주기 전에 서버가 /detail 페이지를 보여주려 할 수 있는데
 * 이때 서버에 아무 기능이 개발이 안되어있으면 404가 뜰 수 있다.
 * 그래서 서버에 "어떤 사람이 /어쩌구로 접속하면 그냥 Vue에게 라우팅 맡겨주세요~" 라고 미리 기능개발이 필요하다
 */
```

```javascript
// Hash mode
import { createRouter, createWebHashHistory } from "vue-router";

const router = [];
const router = createRouter({
  history: createWebHashHistory(),
  routes,
});

/**
 * history: createWebHashHistory()
 * -- URL에 전부 #이 붙은 채로 시작
 * -- codingapple.com/#/ 이게 메인페이지
 * -- URL에서 # 뒤에 있는 내용들은 절대 서버로 전달되지 않아서 #을 붙인다
 * -- 그래서 서버가 라우팅을 채가는 일을 방지할 수 있고 Vue Router에게 온전히 라우팅을 맡길 수 있는 것이다.
 * -- 서버가 없다면 # 붙는 hash 라우터로 사이트를 만드는 것도 좋다
 */
```

#### **2. Navigation guards**

```javascript
// /hello 라는 경로로 들어가기 전에 뭔가 검사를 해주고 싶으면 beforeEnter 라는 항목에
// 1.
const routes = [
  {
    path: "/hello",
    component: HelloWorld,
    beforeEnter: () => {
      if (로그인했냐 == false) {
        return "/login";
      }
    },
  },
];

// 2.
const routes = [
  {
    path: "/hello",
    component: HelloWorld,
    beforeEnter: (to, from) => {
      // 첫 파라미터는 목적지 페이지, 둘째는 출발 페이지
      return to.fullPath; // 전체 경로
    },
  },
];
```

#### **3. 여러개의 route에 같은 navigation guard를 추가하고 싶으면**

```javascript
const router = createRouter({ 어쩌구 });
router.beforeEach((to, from) => {
  //페이지 변경 전에 실행할 코드
});
```

#### **4. Vue 컴포넌트 안에서도 navigation guard 쓸 수 있음**

```javascript
// lifecycle hook쓰는 위치에다가 쓰면 된다
beforeRouteEnter(){}
beforeRouteUpdate(){}
```

<br />

## Part 3 : 인스타그램 만들기

<https://github.com/byahram/vue-starter/tree/master/vuestagram>

### 1. style 속성 데이터바인딩

```html
<div :style="{ fontSize : '20px', marginTop : '10px' }">
  <div :style="{ color : 'red' }">
    <div
      class="post-body"
      :style="{ backgroundImage : 'url(이미지경로ㄷㄷ)' }"
    ></div>

    <!-- '문자' + 변수 + '문자' -->
    <!-- `문자 ${변수} 문자` -->
  </div>
</div>
```

### 2. 서버로 ajax 요청하는 더보기 버튼 만들기

```javascript
import axios from 'axios';

// 1.
axios.get('서버URL').then( 결과 => {
  GET요청 성공시 실행할 코드~~
  console.log(결과);
})

// 2.
axios.get('서버URL').then( 결과 => {
  GET요청 성공시 실행할 코드~~
}).catch( ()=>{
  실패시 실행할 코드
})

// 3.
axios.post('서버URL', '보낼데이터').then( 결과 => {
  POST요청 성공시 실행할 코드~~
}).catch( ()=>{
  실패시 실행할 코드
})

// example
more(){
  axios
    .get(`https://codingapple1.github.io/vue/more${this.더보기}.json`)
    .then( 결과 => {
            this.게시물.push(결과.data);
            this.더보기++;
          })
}
```

### 3. 서버없이 업로드한 이미지 다루기 (잡기술)

```html
<input @change="upload()" type="file" id="file" />

<!-- 
  파일업로드시에 e.target.files 라는 코드를 활용하면 업로드한 파일을 리스트로 알려준다.
  URL.createObjectURL() 여기다가 업로드한 파일을 담으시면 가상의 url을 하나 생성해줌
  -->
<script>
  methods : {
    upload(e){
      let 파일 = e.target.files;
      let url = URL.createObjectURL(파일[0]);
      console.log(url);
      this.step++
    }
  }
</script>
```

### 4. 글 발행기능 만들기

```html
<!-- App.vue에 있던 Next 버튼 -->
<li v-if="step == 1" @click="step++">Next</li>
<li v-if="step == 2" @click="publish()">발행</li>

<!-- Container.vue -->
<textarea @input="$emit('write', $event.target.value)" class="write-box">
write!</textarea
>

<!-- (App.vue) -->
<Container @write="작성한글 = $event" />

<script>
  data(){
    return {
      작성한글 : '',
    },
  }
  publish(){
    var 내게시물 = {
      name: "Kim Hyun",
      userImage: "https://picsum.photos/100?random=1",
      postImage: this.이미지,
      likes: 36,
      date: "May 15",
      liked: false,
      content: this.작성한글,
      filter: "perpetua"
    };
    this.게시물.unshift(내게시물);
    this.step = 0;
  },
</script>
```

### 5. 업로드한 이미지 인스타그램 필터 기능 만들기 (잡기술)

```html
<!-- Container.vue -->
<div v-if="step == 1">
  <div class="upload-image" :style="`background-image:url(${이미지})`"></div>
  <div class="filters">
    <FilterBox :이미지="이미지" v-for="a in 필터들" :key="a"> </FilterBox>
  </div>
</div>

<!-- FilterBox.vue -->
<template>
  <div class="filter-item"></div>
</template>

<style>
  .filter-item {
    width: 100px;
    height: 100px;
    margin: 10px 10px 10px auto;
    padding: 8px;
    display: inline-block;
    color: white;
    background-size: cover;
    background-position: center;
  }
</style>

<script>
  data(){
    return {
      필터들 : [ "aden", "_1977", "brannan", "brooklyn", "clarendon", "earlybird", "gingham", "hudson", "inkwell", "kelvin", "lark", "lofi", "maven", "mayfair", "moon", "nashville", "perpetua", "reyes", "rise", "slumber", "stinson","toaster", "valencia", "walden", "willow", "xpro2"],
    }
  }
</script>
```

```html
<!-- https://cdnjs.cloudflare.com/ajax/libs/cssgram/0.1.12/cssgram.min.css 
      이거 직접 다운받아서 index.html에 집어넣거나
      cdn 방식으로 첨부된 link 태그를 직접 index.html에 넣rl
 -->
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/cssgram/0.1.12/cssgram.min.css"
  integrity="sha512-kr3JaEexN5V5Br47Lbg4B548Db46ulHRGGwvyZMVjnghW1BKmqIjgEgVHV8D7V+Cbqm/VBgo3Rcbtv+mGLoWXA=="
  crossorigin="anonymous"
/>
```

### 6. props 싫으면 slot

자식이 부모데이터를 사용하고 싶으면 props

1. 전송하고
2. 등록하고
3. 사용

- props는 src, style 속성 이런 곳에서도 사용가능

Props 말고 다른 slot을 사용할 수 있다.

1. 자식은 \<slot>\</slot>이라는 HTML 태그를 이용해서 데이터가 꽂힐 곳을 정해놓음
2. 부모는 <자식컴포넌트>데이터</자식컴포넌트> 이렇게 자식컴포넌트 사이에 데이터를 작성해서 보냄
3. 부모가 가진 데이터가 자식의 <slot> 자리에 자동으로 꽂힌다

- HTML 태그기 때문에 HTML 태그처럼만 사용가능

```html
<!-- 자식컴포넌트 -->
1. <slot name="a"></slot>
2. <slot :데이터="데이터"></slot>

<!-- 부모컴포넌트 -->
1.  <자식컴포넌트>
      <template v-slot:a>데이터</template>
    </자식컴포넌트>

2.  <자식컴포넌트>
      <template v-slot:default="작명"> {{작명.데이터}}</template>
      <!-- v-slot: 옆에 default라고 적으면 자식이 보낸 데이터 수신이 가능 -->
    </자식컴포넌트>
```

### 7. 멀리 있는 컴포넌트간 데이터전송할 땐 mitt

```javascript
// 설치 명령어
npm install mitt

// main.js에 mitt 세팅
import { createApp } from 'vue'
import App from './App.vue'

import mitt from 'mitt'
let emitter = mitt();
let app = createApp(App);
app.config.globalProperties.emitter = emitter;

app.mount('#app')
```

```javascript
// 데이터 보내고 싶은 곳에서
this.emitter.emit('이벤트명작명', '데이터')

// 데이터 수신하고 싶은 곳에서 : 보통 수신하는 코드는 mounted() 안에 적는다
this.emitter.on('이벤트명작명', (a)=>{
  데이터수신시 실행할 코드
  a는 출력해보면 데이터나옴
})
```

### 8. Vuex 1 : 사용하는 이유

1. props와 custom event로 데이터 주고받는게 힘들면 사용한다.
   - Vues를 설치하면 js파일하나에다가 모든 데이터를 다 저장할 수 있다. 그럼 모든 컴포넌트들은 그 데이터를 직접 꺼내쓰고 수정할 수 있다.
2. Vue파일과 데이터가 너무 많으면 쓴다.
   - Vuex 라는 라이브러리를 상태관리 (데이터관리) 라이브러리라고 하는데 예를 들어 name이라는 데이터를 컴포넌트 100만개에서 쓰고 있는데 갑자기 삑나면 어디서 삑났는지 name을 쓰는 곳 100개를 다 뒤져야한다.
   - Vuex를 쓰면 데이터를 한 곳에서 관리해주기 때문에, 데이터 수정하는 방법도 한 곳에서 관리하기 때문에 디버깅이 쉽다.

```javascript
// 1. src안에 store.js를 만들고 다음 코드 작성
import { createStore } from "vuex";

const store = createStore({
  state() {
    return {};
  },
});

export default store;

// 2. store.js를 main.js에 등록
import store from "./store.js";
app.use(store).mount("#app");

/**
 * store.js에 저장한 데이터들을 모든 컴포넌트가 가져다쓸 수 있다.
 * 데이터 저장은 store.js에 저장하면 되고
 * 출력하려면 vue파일에서 {{ $store.state.데이터명 }} 하면 된다.
 * 함수나 mounted 이런 곳에서 쓰려면 this.$store.state.어쩌구
 */
```

### 9. Vuex 2 : store에 있는 state 데이터 바꾸는 법

Vuex에 있는 state 데이터를 변경하려면 mutations 라는 항목을 만들어서 거기에 데이터 수정방법을 정의한다.

```javascript
// mutations라는 object 항목을 만든 다음에 거기다가 state 수정방법을 정의

// store.js
const store = createStore({
  state () {
    return {
      name : 'kim',
      age : 20,
    }
  },
  mutations :{
    한살더하기(state){
      state.age++
    }
  },
}
```

```html
<!-- App.vue -->
<button @click="$store.commit('한살더하기')">버튼</button>
```

### 10. Vuex 3 : actions 항목

Vuex에선 state를 수정할 때 mutations 함수를 만들어서 그걸 이용해서 수정하는데

가끔 서버에서 데이터를 가져와서 수정하고 싶을때 서버로 ajax요청을 날리면 되는데

그때는 mutations에 직접 적지 않고 actions라는 항목에 적으면 된다.

```javascript
actions : {
  데이터가져오기(context){
    axios.get('').then(()=>{
      context.commit('mutations함수명')
    })
  }
}
```

### 11. Vuex 4 : mapState를 사용

```javascript
/**
 * computed 안에 만든 함수는 함수를 불러도 안의 코드가 실행안됨
 * computed는 그냥 컴포넌트로드시 한번 실행되고 그 값을 계속 저장해서 쓴다.
 * computed는 일종의 계산결과 저장공간
 * computed 함수는 return 안쓰면 안된다.
 * computed 함수를 가져다가 쓸 때는 소괄호없이 함수명만 쓰면 된다.
 * 자주 꺼내쓰는 state를 computed에 넣어놓으면 나름 짧게 사용가능
 */
computed : {
  now2(){
    return new Date()
  }
},

// methods 안에 만든 함수는 함수를 부를 때마다 안의 코드가 실행됨
methods : {
  now(){
    return new Date()
  }
}
```

```javascript
// state를 vue파일에서 꺼내쓸 때 $store.state.name 이런 식으로 꺼내쓰는데 이게 길고 귀찮으면 mapState라는 함수를 쓴다.
import {mapState} from 'vuex'

// 1.
computed : {
  ...mapState(['state이름1', 'state이름2'])
}

// 2.
computed : {
  ...mapState({ 작명 : 'state이름1'})
}

// 3.
computed : {
  ...mapState(['state이름1', 'state이름2']),
  ...mapMutations([ '좋아요', 'setMore' ])
}
```

### 12. Progressive Web App & 셋팅

```
vue add pwa
```

```javascript
// PWA 설정
module.exports = {
  pwa: {
    name: "님 앱이름",
    themeColor: "#4DBA87",
    msTileColor: "#000000",
    workboxOptions: {
      exclude: [/\.map$/, /manifest\.json$/, "index.html"],
    },
  },
};
```

### 13. Composition API 사용법 (팔로워 페이지 만들기)

```html
<!-- Composition API 식으로 데이터가져오기 -->
<!-- MyPage.vue -->
<!-- 방법 1. -->
<script>
  import { ref } from "vue";
  export default {
    name: "mypage",
    setup() {
      let follower = ref([]);

      return { follower };
    },
  };
</script>

<!-- 방법2 -->
<script setup>
  import { ref } from "vue";
  let follower = ref([1, 2, 3]);
</script>
```

```html
<div style="padding:10px;">
  <h4>팔로워</h4>
  <input placeholder="?" />
  <div class="post-header" v-for="(a,i) in follower" :key="i">
    <div class="profile" :style="`background-image:url(${a.image})`"></div>
    <span class="profile-name">{{a.name}}</span>
  </div>
</div>
```

```javascript
// Ajax 요청 & 데이터 변경
// 1.
import { ref } from 'vue'
import axios from 'axios'

export default {
  name : 'mypage',
  setup(){
    let follower = ref([]);

    axios.get('/follower.json').then((a)=>{
      follower.value = a.data
    })

    return { follower }
  },
}

// 2.
// 컴포넌트가 부착될 때, 업데이트될 때 뭔가 실행하고 싶으면
import { ref, onMounted } from 'vue'
import axios from 'axios'

export default {
  name : 'mypage',
  setup(){
    let follower = ref([]);

    onMounted(()=>{
      axios.get('/follower.json').then((a)=>{
        follower.value = a.data
      })
    })

    return { follower }
  },
}
```

### 14. Composition API 사용법 2 & 간단한 검색기능

#### reactive 사용법

```javascript
// reactive는 object, array 같은 reference data type을 주로 담고
// ref는 숫자, 문자같은 primitive data type을 담는다.

import { ref, reactive } from "vue";

export default {
  setup() {
    let follower = ref([]);
    let test = reactive({ name: "kim" });

    return { follower };
  },
};
```

#### props 사용법

composition API를 써서 개발할 때 setup() 함수 안에서는 위에 등록된 props를 this.props 이런 식으로 가져다쓸 수 없다.

그래서 props 가져와서 뭔가 개발하고 싶을 땐 아래 방식으로.

```javascript
import { ref, toRefs } from "vue";

export default {
  setup(props) {
    let follower = ref([]);
    let { 프롭스명 } = toRefs(props);
    console.log(프롭스명.value);
    return { follower };
  },
};
```

#### watch 사용법

setup() 안에서 watch 같은 걸로 데이터변화를 감시하고 싶으면

```javascript
import { ref, watch } from 'vue'

export default {
  setup(props){
    let follower = ref([]);
    watch( 데이터명, ()=>{ 데이터 변화시 실행할 코드 } )
    return { follower }
  },
}
```

#### computed 사용법

```javascript
import { ref, computed } from "vue";

export default {
  setup(props) {
    let follower = ref([]);
    let 어쩌구 = computed(() => {
      return 10;
    });
    console.log(어쩌구.value);
    return { follower };
  },
};
```

#### methods 사용법

```javascript
import { ref } from "vue";

export default {
  setup(props) {
    let follower = ref([]);

    function hello() {}
    return { follower, hello };
  },
};
```

#### Vuex store 사용법

```javascript
import { ref } from "vue";
import { useStore } from "vuex";

export default {
  setup(props) {
    let follower = ref([]);
    let store = useStore();
    console.log(store.state.name);
    return { follower };
  },
};
```

#### 팔로워 검색기능

```html
<input placeholder="?" @input="search($event.target.value)" />

<script>
  setup(){
    let follower = ref([]);
    let followerOriginal = ref([]);

    onMounted(()=>{
      axios.get('/follower.json').then((a)=>{
        follower.value = a.data;
        followerOriginal.value = [...a.data];
      })
    });

    function search(검색어){
      let newFollower = followerOriginal.value.filter((a)=>{
        return a.name.indexOf(검색어) != -1
      });
      follower.value = [...newFollower]
    }
    return {follower, search}
  },
</script>
```
