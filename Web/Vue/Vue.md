# Vue.js

## 개요
반응형 웹 UI 프레임워크, Vue 인스턴스에 데이터를 바인딩하고 HTML 요소에 v- 접두사로 시작하는 디렉티브를 속성으로 추가하여 DOM 요소와 데이터를 연결한다. MVVM 패턴으로 설계되어있다.
- V-DOM : Vue는 가상 DOM을 가지고 있다. Vue를 통해 DOM을 변경하는 명령을 내리면 V-DOM에 먼저 반영하고, 모든 변경사항이 완료된 뒤에 DOM에 적용된다. 기본 Javascript의 append를 루프에서 사용한다면, 요소 하나 추가 될 때 마다 DOM을 렌더링하지만, v-for을 사용한다면 모든 요소를 V-DOM에 추가한 뒤, DOM에 반영해 렌더링은 1번만 적용된다.

## 생명주기
1. 인스턴스 생성 (beforeCreate)
2. 화면에 반응성 주입 (Created)
3. 템플릿 컴파일 (beforeMount)
4. 초기 렌더링, DOM 노드 생성 및 삽입 (mounted)
5. 마운트
6. 화면 리렌더링 및 데이터 갱신 (beforeUpdate, updated)
7. 소멸 (beforeDestroy, destroyed)

## 기본 구조
```javascript
const app1 = new Vue({
    el: '#app', //적용할 element 영역, #은 id
    data: {
        message: '데이터',
        },
    methods: {
        myfunc: function(){
            // 메소드
        }
    },
    compute: {
        //getter property
        getter_name: function(){
            return ''; 
        },

        //setter property
        setter: {
            get: function(){
                //get은 필수
            },
            set: function(param){
                //setter
            }
        }
    },
    watch: {
        model_name: function(){
            // model이 변경될 때 마다 호출되는 메소드
        }
    }
    });
```
이중 중괄호를 통해 HTML에서 직접 출력할 수 있다.

## 디렉티브
- v-bind : 속성에 값을 바인딩, v-bind:HTML속성으로 원하는 HTML 속성에 값을  적용할 수 있으며 :으로 생략할 수 있다. 속성을 key로 가지는 객체를 바인딩해서 한번에 적용할 수도 있다.
- v-if : 값에 따라 태그를 활성화/비활성화한다. 보통 boolean 값을 사용한다.
- v-for : 배열을 순회하며 태그를 생성한다. item in array 형태로 사용되며, item에 배열의 값이 들어간다.
- v-on : 이벤트를 적용, v-on:HTML이벤트로 원하는 이벤트에 Vue의 methods에 해당하는 메소드를 호출할 수 있다. @로 생략이 가능하다.

## 컴포넌트
태그를 재사용하게 해주는 기능
```javascript
// 전역 컴포넌트
Vue.component('tag-name', {
  props: ['variable'], // 속성 추가
  template: '<li>{{ variable.key }}</li>' //출력할 HTML
})

// 지역 컴포넌트
const component = {
    template: '<div>지역 Hello Vue.js!</div>',
};

const app = new Vue({
    el: '#app',
    components: {
    'tag-name': component,
    },
});
```

### 컴포넌트간 통신
- 부모->자식 : props이용
- 자식->부모 : 사용자 이벤트 이용

## 라우터
Vue-router는 별도의 cdn이 필요하다.
```html
    <body>
        <h3>뷰 라우터(Vue Router)</h3>
        <h4>라우터 기본</h4>
        <!-- 출력할 컴포넌트의 조건을 경로(URI)를 이용하여 지정 -->
        <div id="app1">
            <p>
                <router-link to="/page1">Page1 컴포넌트</router-link>
                <router-link to="/page2/홍길동">Page2 컴포넌트</router-link>
            </p>
            <!-- 여기서 출력 -->
            <router-view></router-view>
        </div>
        <script>
            // 1. 컴포넌트 생성
            const comp1 = {
                template: '<h3>Page1!!!</h3>',
            };

            const comp2 = {
                props: ['user'],
                template: `<div>
                    <h3>Page2!!! {{user}}</h3>
                    <router-link to="/page1"><button type="button">페이지 1</button></router-link>
                    </div>`,
            };

            // 2. 라우터 생성
            const router = new VueRouter({
                routes: [
                    { path: '/page1', component: comp1 },
                    { path: '/page2/:user', component: comp2, props: true },
                ],
            });

            // 3. 인스턴스 등록
            const app1 = new Vue({
                el: '#app1',
                router: router,
            });
        </script>
    </body>
```

### 중첩 라우터
```javascript
            const User = {
                template: `
                    <div>
                        <h2>User Page</h2>
                        <router-view></router-view>
                    </div>
                `,
            };

            const UserInfo = {
                template: '<h3>회원 정보!!!</h3>',
            };

            const UserPost = {
                template: '<h3>등록한 글들!!!</h3>',
            };

            const app2_router = new VueRouter({
                routes: [
                    {
                        path: '/user',
                        component: User,
                        children: [
                            { path: '/user/info', component: UserInfo },
                            { path: '/user/post', component: UserPost },
                        ],
                    },
                ],
            });

            const app2 = new Vue({
                el: '#app2',
                router: app2_router,
            });
```

### components
```html
    <body>
        <h3>Vue Named view</h3>
        <div id="app">
            <router-view name="header"></router-view>
            <router-view></router-view>
            <router-view name="footer"></router-view>
        </div>
        <script>
            const Body = {
                template: '<div><h1>Body</h1></div>',
            };
            const Header = {
                template: '<div><h1>Header</h1></div>',
            };
            const Footer = {
                template: '<div><h1>Footer</h1></div>',
            };

            const router = new VueRouter({
                routes: [
                    {
                        path: '/',
                        components: {
                            // 하나의 path에 복수의 component
                            default: Body,
                            header: Header,
                            footer: Footer,
                        },
                    },
                ],
            });

            const app = new Vue({
                el: '#app',
                router: router,
            });
        </script>
    </body>
```