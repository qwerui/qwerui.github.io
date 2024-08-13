# React-Native

## 개요
네이티브 앱 제작을 위한 오픈소스 라이브러리, 모바일 간 크로스 플랫폼이 가능하다. JSX를 네이티브 뷰 형태로 렌더링하는 역할

## StyleSheet
```javascript

import {View, StyleSheet} from 'react-native';

const styles = StyleSheet.create({
    someView:{
        flex:1; //화면을 차지하는 가중치
        height:100%;
    }
});

function App(){
    return(
        <View style={styles.someView}>

        </View>
    );
}

// styled-components
import styled from 'styled-components/native';

const Container = styeld.View`
    //css구문
`;

// 아래 형식으로 사용(View에 css를 적용한 태그와 비슷한 개념)
<Container>
</Container>

```

## Navigation
```javascript
import {NavigationContainer} from '@react-navigation/native';
import {createStackNavigator} from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
    return (
        <NavigationContainer> //최상위 컴포넌트
            <Stack.Navigator> //화면 관리 컴포넌트
                <Stack.Screen name="Home" component={HomeScreen}/> //실제 화면 컴포넌트
            </Stack.Navigator>
        </NavigationContainer>
    );
}

// 화면 컴포넌트
function Home({navigation}){
    return(
        <>
            <Button title="화면이동" onPress={()=>{navigation.navigate('다른 화면')}}/>
        </>
    )
}
```