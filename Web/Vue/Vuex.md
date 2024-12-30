# Vuex

## 개요
global하게 상태를 관리할 수 있게 해주는 라이브러리. Vue는 기본적으로 형제간 직접 데이터 교환이 일어날 수 없고, 부모를 경유해야한다.

## 구조
[출처](https://simplevue.gitbook.io/intro/16.-vuex)
```javascript
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    // 글로벌로 관리될 상태 값 
    counter: 0
  },
  getters: {
    // 데이터에 변화를 줄 순 없다.
    counter: state => state.counter
  },
  mutations: {
    // 실제 데이터 변화가 일어나는 곳
    increment: state => (state.counter += 1),
    decrement: state => (state.counter -= 1)
  },
  actions: {
    // mutaion 을 일을키위한 행동, 컴포넌트에서는 actions 를 사용한다
    addCounter: context => context.commit("increment"),
    subCounter: context => context.commit("decrement")
  }
});
//-----------------------------
// main.js

import Vue from "vue";
import App from "./App.vue";
import store from "./store";

Vue.config.productionTip = false;

new Vue({
  store, // Vue 객체에 등록 필요
  render: h => h(App)
}).$mount("#app");
```
```html
<script>
import { mapActions, mapGetters } from 'vuex';

export default {
    data() {
        return {
            count: 0,
        };
    },
    methods: {
        ...mapActions(['addCounter', 'subCounter']),
    },
    computed: {
        ...mapGetters(['counter']),
        // this.$store을 통해서도 접근가능
    },
};
</script>
```
## reactive-store
vue 2.7부터 사용가능한 vuex와 비슷한 기능을 수행하는 기능
```
//reactive-store.js
import { reactive } from 'vue';

export const store = reactive({
    count: 0,
    increment() {
        ++this.count;
    },
    decrement() {
        --this.count;
    },
});
//-----------------------
//컴포넌트
<script setup>
import { store } from '@/store/reactive-store';
</script>


```