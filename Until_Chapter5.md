# Until Chapter5

https://github.com/bjpublic/Reactnative

## create-react-native-app 설치하고 사용하기

```bash
npm install -g create-react-native-app
```

## 리액트 네이티브 앱 실행하기

```bash
create-react-native-app testProject
```

## 앱 실행하기

```bash
npm start
```

### Expo 설치하고 사용하기

```bash
npm install -g expo-cli
```

```bash
expo init AwesomeProject

cd AwesomeProject
npm start # you can also use: expo start
```

{Platform.OS !== 'android' ? '(Android only)' : 'android'}

```js
// CustomButton.js

import React, { Component } from 'react';
import { Image, Alert, Platform, StyleSheet, Text, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback, TouchableWithoutFeedback, View } from 'react-native';

export default class Touchables extends Component {
  
    constructor(props) {
        super(props);
        this.state = {
            top: 0 // Initial value
        };
    }

  onclick = () => {
    console.log('On click works')
    this.setState( { top: this.state.top + 5 }) // 5 is value of change.
};


  render() {
    return (
      <View style={styles.container}>
        <TouchableOpacity style={styles.button1} onPress={() => {Alert.alert('회원가입 버튼');}} >
      
             <Image
            style={styles.buttonImg}
            source={require('./images/images_1.png')}/>
            <Text style={styles.buttonText}>Notifications</Text>
          
        </TouchableOpacity>
        <TouchableOpacity onPress={this.onclick} style={{flexDirection:"row"}}>
          <View style={styles.button1}>
            <Image
            style={styles.buttonImg}
            source={require('./images/images_2.png')}/>
            <Text style={styles.buttonText}>Menu</Text>
          </View>
        </TouchableOpacity>
        <TouchableNativeFeedback
            onPress={this._onPressButton}
            background={Platform.OS === 'android' ? TouchableNativeFeedback.SelectableBackground() : ''}>
          
          <View style={styles.button2}>
           <Image
            style={styles.buttonImg}
            source={require('./images/images_3.png')}/>
            <Text style={styles.buttonText}>Notice </Text>
          </View>
        </TouchableNativeFeedback>
        <TouchableWithoutFeedback
            onPress={this.onclick}
            >
          <View style={styles.button2}>
          <Image
            style={styles.buttonImg}
            source={require('./images/images_4.png')}/>
            <Text style={styles.buttonText}>Dormitory {"\n"}Rules</Text>
          </View>
        </TouchableWithoutFeedback>
        <TouchableHighlight onPress={this._onPressButton} onLongPress={this._onLongPressButton} underlayColor="white">
          <View style={styles.button2}>
          <Image
            style={styles.buttonImg}
            source={require('./images/images_5.png')}/>
            <Text style={styles.buttonText}>Inquiries</Text>
          </View>
        </TouchableHighlight>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexWrap: "wrap",
    flexDirection: "row",
    marginBottom: 180,
    backgroundColor: "aliceblue",
    maxHeight: 400,
  },
    button1: {
    borderRadius: 10,
    height: 170,
    width: 155,
    marginLeft:10,
    marginRight:13,
    marginBottom: 20,
    backgroundColor: '#ffffff'
  },
  button2: {
    borderRadius: 10,
    marginLeft:10,
    marginRight:8,
  
    marginBottom: 30,
    width: 100,
    height: 170,
    backgroundColor: '#ffffff'
  },
  buttonText: {
    textAlign: 'center',
    fontSize: 16,
    padding: 10,
   
    color: 'black'
  },
  buttonImg: {
    marginTop: 50,
    height:40,
    width:40,
    alignSelf: "center",
    
  }
});
```

## 타입 스크립트

### 수동설치

#### 타입스크립트와 타입 정의 파일 설치

```bash
# For the latest stable version:
npm install -g typescript

#For our nightly builds:
npm install -g typescript@next

# 타입스크립트와 라이브러리와 리액트 네이티브 타입이 정의된 타입 정의 파일 설치
npm install typescript @types/react @types/react-native --save-dev
```

#### delete

```bash
npm uninstall typescript
```

#### "tsconfig.json"파일을 프로젝트 루트 폴더에 만들고 다음 내용 추가

```json
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react",
    "lib": ["es6"],
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext",
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

### 자동설치

```bash
react-native init FirstApp --template typescript
```

- App.js 를 App.tsx로 변경하고 아래와 같이 수정
  -  ~~const App: () => React$Node = () = > {~~ 
  - const App = ({ }: props) => {

## Styled Component 사용

### 설치

```bash
npm install --save styled-components  #라이브러리
npm install --save-dev @types/styled-components	# 타입정의 파일
```

### App.tsx

```tsx
import React, {Fragment} from 'react';
import {StatusBar, SafeAreaView} from 'react-native';

import {
  Header,
  LearnMoreLinks,
  Colors,
  DebugInstructions,
  ReloadInstructions,
} from 'react-native/Libraries/NewAppScreen';

import Styled from 'styled-components/native';

const ScrollView = Styled.ScrollView`
  background-color: ${Colors.lighter};
`;

const Body = Styled.View`
  background-color: ${Colors.white};
`;

const SectionContainer = Styled.View`
  margin-top: 32px;
  padding-horizontal: 24px;
`;

const SectionDescription = Styled.Text`
  margin-top: 8px;
  font-size: 18px;
  font-weight: 400;
  color: ${Colors.dark};
`;

const HighLight = Styled.Text`
  font-weight: 700;
`;

interface Props {}

const App = ({}: Props) => {
  return (
    <Fragment>
      <StatusBar barStyle="dark-content" />
      <SafeAreaView>
        <ScrollView contentInsetAdjustmentBehavior="automatic">
          <Header />
          <Body>
            <SectionContainer>
              <SectionDescription>Step One</SectionDescription>
              <SectionDescription>
                Edit <HighLight>App.js</HighLight> to change this screen and
                then come back to see your edits.
              </SectionDescription>
            </SectionContainer>
            <SectionContainer>
              <SectionDescription>See Your Changes</SectionDescription>
              <SectionDescription>
                <ReloadInstructions />
              </SectionDescription>
            </SectionContainer>
            <SectionContainer>
              <SectionDescription>Debug</SectionDescription>
              <SectionDescription>
                <DebugInstructions />
              </SectionDescription>
            </SectionContainer>
            <SectionContainer>
              <SectionDescription>Learn More</SectionDescription>
              <SectionDescription>
                Read the docs to discover what to do next:
              </SectionDescription>
            </SectionContainer>
            <LearnMoreLinks />
          </Body>
        </ScrollView>
      </SafeAreaView>
    </Fragment>
  );
};

export default App;
```



## 절대 경로로 컴포넌트 추가

### babel-plugin-root-import 사용을 위한 라이브러리 설치

```bash
npm install --save-dev babel-plugin-root-import
```

### babel.config.js 수정

```js
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'babel-plugin-root-import',
      {
        rootPathPrefix: '~',
        rootPathSuffix: 'src',
      },
    ],
  ],
};
```

### tsconfig.json 수정

```json
  
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react",
    "lib": ["es6"],
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext",
    "baseUrl": "./src",  // 추가
    "paths": {			// 추가
      "~/*": ["*"]		// 추가, ~ 기호를 src 폴더와 매핑 시킴
    }
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

### src 폴더 생성후 App.tsx 이동 , index.js 수정

- ~~import App from './App';~~
- import App from '~App';
  - ~ : 절대경로 사용

## 개발자 메뉴

### 디버깅 

- iOS: Cmd + D(⌘+D)
- Android
  - 맥 : Cmd + M(⌘+M)
  - 윈도우: Ctrl + M

### 패스트 리프레스

- iOS: "Cmd + R(⌘ + R)
- 안드로이드: "R" 키 연속 두번

## 라이프 사이클 함수

### 1) constructor 함수

- 클래스 컴포넌트에서  State 사용하지 않을시, State의 초기값 설정이 필요하지 않으면, 생략가능
- 생성자 함수 사용시 반드시 super(prop) 함수 호출하여 부모 클래스의 생성자 호출
- 생성자 함수는 해당 컴포넌트가 생성될 때 한 번만 호출

### 2) render 함수

- render 함수는 부모로부터 받는 Props 값이 변경되거나, this.setState로 State의 값이 변경되어 화면을 갱신할 필요가 있을 때마다 호출
- 이 함수에서 this.setState를 사용해 State값을 직접 변경할 경우, 무한루프에 빠질 수 있음.

### 3) getDerivedStateFromProps 힘스

- 부모로부터 받은 Props와 State를 동기화 할 때 사용

```tsx
// 이 함수는 컴포넌트가 생성될 때 한 번 호출
// Props와 State를 동기화해야 하므로 Props가 변경될 때마다 호출
static getDerivedStateFromProps(nextProps, prevState){
	if(nextProps.id !== prevState.id){
    	return {value: nextProps.value};
    }
    return null;
}
```

### 4) componentDidMount 함수

- render 함수가 처음 한 번 호출된 후, 이 함수가 호출됨
- 한 번만 호출되므로, ajax를 통해 데이터를 습득하거나 다른 자바스크립트 라이브러리와의 연동 수행에 적합
- Props 값이 변경되거나 this.setState로 State 값이 변경되어도 다시 호출되지 않음.
- render 함수와 다르게 this.setState 직접 호출 가능.
- ajax를 통해 받은 데이터를 this.setState를 사용해 State에 설정하기 적합

### 5) shouldComponentUpdate 함수

- 클래스 컴포넌트는 기본적으로 부모로부터 받은 Props가 변경되거나, this.setState로 State를 변경하면 리렌더링되어 화면을 다시 그림
- 다시 화면을 그리고 싶지 않은 경우 이 함수를 이용해 렌더링 제어 가능
- false를 반환하면 리렌더링 막음. true는 항상 리렌더링

```js
shouldComponentUpdate(nextProps: Props, nextState: State){
	console.log('shouldComponentUpdate');
    return nextProps.id !== this.props.id;
}
```

### 6) getSnapshotBeforeUpdate 함수

- Props  또는 State가 변경되어 화면을 다시 그리기 위해 render 함수가 호출된 후, 실제로 화면이 갱신되기 바로 직전, 이 함수 호출
- 반환값은 세번째 매개변수(snapshot)로 전달
- 화면을 갱신하는 동안 수동으로 스클롤 위치 고정시 사용

### 7) componentDidUpdate 함수

- componentDidMount 는 컴포넌트가 처음 화면에 표시된 후 실행되고 두 번 다시 호출되지 않는 함수
- componentDidUpdate 는 처음 화면에 표시될 때 실행되지 않지만, Props 또는 State가 변경되어 화면이 갱신될 때마다 render 함수 호출 이후에, 호출되는 함수
- getSnapshotBeforeUpdate 함수와 함께 스크롤 수동으로 고정시 활용
- render 함수처럼 State 값이 변경될 때 호출

### 8) componentWillUnmount 함수

- 해당 컴포넌트가 화면에서 완전히 사라진 후, 호출되는 함수
- 라이브러리 해지 또는 해제할 때 사용
- this.setState 호출시 Warning 발생

### 9) componentDidCatch 함수

- 컴포넌트 랜더링에서 예외처리해주는 함수
- render 함수의 return 부분에 에러 발생시 실행
- 에러발생시, 자식 컴포넌트를 표시하지 않게 함으로써 비정상 종료 예방

```tsx
// index.tsx

import React, { useState } from 'react';
import Styled from 'styled-components/native';
import Button from '~/Components/Button';

const Container = Styled.SafeAreaView`
    flex: 1;
`;

const TitleContainer = Styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
`;
const TitleLabel = Styled.Text`
    font-size: 24px;
`;

const CountContainer = Styled.View`
    flex: 2;
    justify-content: center;
    align-items: center;
`;
const CountLabel = Styled.Text`
    font-size: 24px;
    font-weight: bold;
`;

const ButtonContainer = Styled.View`
    flex: 1;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
`;

interface Props {
  title?: string;
  initValue: number;
}
interface State {
  count: number;
  error: boolean;
}

class Counter extends React.Component<Props, State> {
  constructor(props: Props) {
    super(props);
    console.log('constructor');

    this.state = {
      count: props.initValue,
      error: false,					// error 예방
    };
  }

  render() {
    console.log('render');
    const { title } = this.props;
    const { count, error } = this.state;
    return (
      <Container>
        {!error && (
          <>
            {title && (
              <TitleContainer>
                <TitleLabel>{title}</TitleLabel>
              </TitleContainer>
            )}
            <CountContainer>
              <CountLabel>{count}</CountLabel>
            </CountContainer>
            <ButtonContainer>
              <Button
                iconName="plus"
                onPress={() => this.setState({ count: count + 1 })}
              />
              <Button
                iconName="minus"
                onPress={() => this.setState({ count: count - 1 })}
              />
            </ButtonContainer>
          </>
        )}
      </Container>
    );
  }

  static getDerivedStateFromProps(nextProps: Props, prevState: State) {
    console.log('getDerivedStateFromProps');

    return null;
  }

  componentDidMount() {
    console.log('componentDidMount');
  }

  shouldComponentUpdate(nextProps: Props, nextState: State) {
    console.log('shouldComponentUpdate');
    return true;
  }

  getSnapshotBeforeUpdate(prevProps: Props, prevState: State) {
    console.log('getSnapshotBeforeUpdate');

    return null;
  }

  componentDidUpdate(prevProps: Props, prevState: State, snapshot: null) {
    console.log('componentDidUpdate');
  }

  componentWillUnmount() {
    console.log('componentWillUnmount');
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    this.setState({
      error: true,		// error 예방
    });
  }
}
export default Counter;
```

### 10) 호출 순서

- 컴포넌트 생성시
  - constructor → getDerivedStateFromProps → render → componentDidMount
- 컴포넌트의 Props가 변경될 때
  - getDerivedStateFromProps → shouldComponentUpdate → render → getSnapshotBeforeUpdate → componentDidUpdate
- 컴포넌트의 State가 변경될때
  - shouldComponentUpdate →  render → getSnapshotBeforeUpdate → componentDidUpdate
- 컴포넌트 렌더 중 에러 발생될 때
  - componentDidCatch
- 컴포넌트 제거될 때
  - componentWillUnmount

