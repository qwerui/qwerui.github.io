# Hook

## State
state 변수값을 선언 및 할당
```javascript
import React, {useState} from 'react';
const [number, setNumber] = useState(0);
```

## Effect
componentDidMount() + componentDidUpdate(), 2번째 파라미터가 변화할 때 작업을 수행한다. return을 사용할 수 있으며 이는 언마운트될 때 콜백(clean-up)으로써 실행된다.
```javascript
useEffect(()=>{
    //작업
}, []);
```

## Memo
특정 값이 바뀌었을 때만 실행, 메모이제이션, 값을 저장해 불필요한 동작 또는 렌더링을 막아 최적화에 사용하기 적합하다.

## Callback
특정 값이 바뀌었을 때만 실행, Memo의 함수 버전