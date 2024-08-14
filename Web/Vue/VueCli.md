# Vue CLI

## 개요
Vue의 개발 환경을 설정해주는 도구, 폴더 구조나 webpack등을 설정하는 과정을 덜 수 있다. .vue 확장자를 통해 HTML을 구성한다. React의 create-react-app의 결과와 비슷한 구조가 생성된다.

## 설치
1. vue-cli 설치
- npm install -g npm
- npm install -g @vue/cli
2. 설치 확인
- vue --version
3. 프로젝트 생성
- vue create [project-name]
4. 기본 모듈
- npm install axios@1.7.2 cors@2.8.5 express@4.19.2 express-session@1.18.0 sequelize@6.37.3 sqlite3@5.1.7 vue-router@3.6.5 vuex@3.6.2
5. 실행
- vue serve

## 구조
프로젝트 생성 시 기본 구조다.
```html
<!--현재 파일의 템플릿을 정의한다.-->
<template>
    <!--최상위 요소는 1개만 가질 수 있다.-->
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<!--javascript 정의, import로 필요한 컴포넌트를 가져오고, export로 현재 컴포넌트를 내보낸다.-->
<script>
import HelloWorld from './components/HelloWorld.vue'

// 사실상 Vue 객체, data와 methods도 가질 수 있다.
export default {
  name: 'App', // 컴포넌트의 이름
  components: {
    HelloWorld // 현재 컴포넌트에서 사용할 외부 컴포넌트
  },
  props: {
    message: String //프로퍼티 정의
  }
}
</script>

<!--CSS 정의 style태그에 scoped를 붙이면 현재 컴포넌트에만 적용된다.-->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

## Route
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

export default new VueRouter({
  mode: 'hash'
  routes: [{
    path: '/path',
    component: component_object
  }]
})

```

## Slot
```html
<template>
    <div>
        <h3>Vue Slot</h3>
        <div-comp>메시지입니다.</div-comp>
        <i>
            <div-comp>
                <b>홍길동님 {{ message }}</b>
            </div-comp>
        </i>
        <div-comp>
            <ul-comp>
                <template v-slot:header>헤더</template>
                <template>바디</template>
                <template #footer>푸터</template>
            </ul-comp>
        </div-comp>
    </div>
</template>

<script>
const DivComp = {
    //slot : template을 template에게 넘기는 경우
    template: `<div style="border:1px solid silver;"><slot>대체내용</slot></div>`,
};

const UlComp = {
    template: `<ul>
        <li><slot name="header"></slot></li>
        <li><slot></slot></li>
        <li><slot name="footer"></slot></li>
        </ul>`,
};

export default {
    data() {
        return {
            message: '안녕하세요',
        };
    },
    components: {
        DivComp,
        UlComp,
    },
};
</script>

<style></style>

```