- 2021.03.16. Chapter01~05 
- 2021.03.17. Chapter06
- 2021.03.18. Chapter07
- 2021.03.19. Chapter08
- 2021.03.22.~23. Chapter09
- 2021.03.24.~25. firebaseTest
- 2021.03.26. 클론프로젝트



### 백그라운드 이미지 설정

```tsx
import {ImageBackground} from 'react-native';
<ImageBackground style={{flex: 1}} source={require('./images/backImage.png')} />
```



### 상단바 제거

- 참고: https://www.python2.net/questions-72779.htm

```tsx
import {StatusBar} from 'react-native';
<StatusBar hidden={true} /> 
```

### 버튼 생성 및 테스트 

```tsx
import {TouchableOpacity} from 'react-native';

const onPress = () => 
 <WebView source={{ uri: 'https://dorm.pusan.ac.kr/dorm/function/mealPlan/20000403' }}  />

<TouchableOpacity style={styles.button} onPress={onPress}>
            <Text>here</Text>
</TouchableOpacity>
```

### 네비게이션 테스트

```tsx
import { useNavigation } from '@react-navigation/native';
import { createStackNavigator} from '@react-navigation/stack';

const StackNavigator = createStackNavigator();

const AppNavigator = StackNavigator({
 Cluster1: { screen: Cluster1},
  Play: { screen: Play},
});

```

```tsx
import React, { useState, useRef, Component }  from 'react';

import {
  View,
  Text,
  Image,
  StyleSheet,
  SafeAreaView,
  ImageBackground,
  TouchableOpacity,
  StatusBar,
  Linking ,
} from 'react-native';

import WebView from 'react-native-webview';

import { useNavigation } from '@react-navigation/native';
import { createStackNavigator, createAppContainer, StackNavigationProp } from '@react-navigation/stack';

import Styled from 'styled-components/native';


const Container = Styled.SafeAreaView`
  flex: 1;
  background-color: #141414;
  align-items: center;
  justify-content: center;
`;


const styles = StyleSheet.create({
   container: {
    flex: 1,
    justifyContent: "center",
  
  },
  title: {
    fontSize: 25,
    padding: 15,
    color: 'white',
    fontWeight: 'bold',    
    textAlign: 'center',    
  },
  button: {
    alignItems: "center",
    backgroundColor: "#DDDDDD",
    padding: 10
  },
  countContainer: {
    alignItems: "center",
    padding: 10
  },

});

const MyComponent = props => {
  const {name, children} = props;
  return (
    <Text>
      제 이름은 {name} {"\n"}
      children 값은 {children}
    </Text>
  );
};

MyComponent.defaultProps = {
  name: '기본 이름설정'
};

const BlackDog = () => {
  this.name = '흰둥이';
  return (
      <Text>{name}</Text>
  );
};



const App = () => {
  

  const [count, setCount] = useState(0);
  const onPress = () => setCount(prevCount => prevCount + 1);


  return (
    <Container>
      <ImageBackground
        style={{flex: 1}}
        source={require('../images/backImage.png')} 
      >
        <View style={styles.container}>
        
          <StatusBar hidden={true} /> 

          <Text style={styles.title}>
             React Native Background Image Example
          </Text>

          <Text>Count: {count}</Text>
          <TouchableOpacity style={styles.button} onPress={onPress}>
            <Text>here</Text>
          </TouchableOpacity>
          <MyComponent>children 리엑트</MyComponent>
          <BlackDog />

       
        </View>

      </ImageBackground>

    </Container>
  );
};


export default App;

```

