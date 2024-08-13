# React

프런트엔드 자바스크립트 라이브러리, 동적인 웹 페이지를 효율적으로 유지보수하고 관리하기 위해 개발되었다. 보통 같은 js환경인 node.js를 백엔드로 사용

## 특징
- 컴포넌트 : 컴포넌트를 조합해 화면을 구성한다. 재사용이 용이, key를 지정하면 어떤 원소에 변동이 있었는지 알아낼 수 있다.
- 가상 DOM : 메모리에 가상의 DOM을 실행해 내용 변경 시 이 가상 DOM을 변경, 실제 DOM을 변경한 것이 아니기에 렌더링이 바로 발생하지 않고, 모든 변경사항을 반영한 뒤에 실제 DOM에 렌더링을 요청한다. 이를 통해 성능저하를 해결

## 기본 형식
```javascript

// App.js
import React from 'react';
import './App.css';

function App() {
    return (
        <div>Hello React</div> //리액트 컴포넌트는 루트 요소를 무조건 하나 가져야한다.
    );
}

// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <React.StrictMode>
    <App/>
    </React.StrictMode>
)

// 사용자 컴포넌트(App.js에서 추가해야한다)
import React from 'react';

function Comp() {
    return (
        <h1>Component</h1>
    );
}

export default Comp;

// 클래스 컴포넌트
import React, {Component} from 'react';

class ClassComp extends Component {
    render() {
        return (
            <div>
                <h1>ClassComponent</h1>
            </div>
        )
    }
}

```

## 컴포넌트
웹 페이지에 표시되는 모듈화된 개체

### 생명주기
마운트(생성)->업데이트->언마운트(삭제)   

- 마운트
1. constructor
2. getDerivedStateFromProps
3. render
4. componentDidMount

- 업데이트
1. getDerivedStateFromProps
2. shouldcomponentUpdate
3. render
4. getSanpshotBeforeUpdate
5. componentDidUpdate

- 언마운트
1. componentWillUnmount

## props
컴포넌트 간 매개변수, 이름="값"의 속성과 동일한 형식으로 지정할 수 있다. 매개변수를 받는 컴포넌트에서 인자로 props를 가지거나, 구조분해할당으로 props의 이름을 통해 사용할 수 있다. props를 사용하기 위해서는 중괄호로 감싸야할 필요가 있다.
```javascript
function App() {
    const something = "name";
    return(
        <div>
            <Comp name = {something}/>
        </div>
    );
}

// Comp
function Comp({name}){
    return <h1>{name}</h1>
}

export default Comp;
```