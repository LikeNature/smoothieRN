# 날씨앱 - 위치정보와 API 사용법

## 1. Fetch

- 자바스크립트 비동기 처리: XHMLHttpRequest(XHR)
  - XHR API 는 이벤트 기반 모델로 입출력과 state를 모두 하나의 객체로 관리. 
  - state는 이벤트를 통해 추적
  - 이벤트 기반은 Promise 기반 비동기 방식과 잘 어울리지 못함

```js
// Just getting XHR is a mess!
if (window.XMLHttpRequest) { // Mozilla, Safari, ...
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) { // IE
  try {
    request = new ActiveXObject('Msxml2.XMLHTTP');
  } 
  catch (e) {
    try {
      request = new ActiveXObject('Microsoft.XMLHTTP');
    } 
    catch (e) {}
  }
}

// Open, send.
request.open('GET', 'https://davidwalsh.name/ajax-endpoint', true);
request.send(null);
```



- fetch API 
  - XMLHttpRequest를 대신하기 위한 방안 중 하나
  - 기본 사용

```js
// Basic fetch Usage
// A fetch function is now provided in the global window scope, with the first argument being the URL:

// url (required), options (optional)
fetch('https://davidwalsh.name/some/url', {
	method: 'get'
}).then(function(response) {
	
}).catch(function(err) {
	// Error :(
});
```

- 자바스크립트 Promises 사용

![img](https://mdn.mozillademos.org/files/8633/promises.png)

-  [Chaining](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Using_promises#chaining)

  보통 하나나 두 개 이상의 비동기 작업을 순차적으로 실행해야 하는 상황에서 **promise chain**을 이용하여 해결

```js
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);
// 또는
const promise2 = doSomething().then(successCallback, failureCallback);
```

```js
// promise 적용방식 , 반환된 promise에 promise chain을 형성하도록 추가
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log('Got the final result: ' + finalResult);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);

// then 에 넘겨지는 인자는 선택적(optional)
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);  // then(null, failureCallback) 의 축약

// Promise 화살표 표현
// 반환값이 반드시 있어야 하며, 없다면 콜백 함수가 이전의 promise의 결과를 받지 못함
doSomething()
.then(result => doSomethingElse(result))
.then(newResult => doThirdThing(newResult))
.then(finalResult => {
  console.log(`Got the final result: ${finalResult}`);
})
.catch(failureCallback);
```

## 2. 위치 정보

- 리엑트 네이티브는 위치정보 API 스펙(watchPosition, getCurrentPosition)을 확장하여 사용
- 안드로이드 기본 API : android.location.API
- 구글권장 API : Google Location Service API
  - react-native-geolocation-servce 라이브러리 사용

## 3. 프로젝트 준비

```c++
react-native init weatherapp
```

```bash
cd weatherapp
npm install --save styled-components
npm install --save-dev typescript @types/react @types/react-native @types/styled-components babel-plugin-root-import
```

```json
// root 폴더에 tsconfig.json 파일 생성 및 내용 추가
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
    "baseUrl": "./src",
    "paths": {
      "~/*": ["*"]
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

```js
// 절대 경로로 컴포넌트 추가하기 위한 설정
/**
 * Metro configuration for React Native
 * https://github.com/facebook/react-native
 *
 * @format
 */

module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  // 이하 추가
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

- src 폴더 생성 => App.tsx 이름변경 => src 폴더로 이동 => App.tsx 파일 수정

```tsx
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 *
 * @format
 * @flow strict-local
 */

import React, {Fragment} from 'react';
import { StatusBar, SafeAreaView } from 'react-native';


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

const App = ( { } : Props) => {
  return (
  <Fragment>
    <StatusBar barStyle="dark-content" />
    <SafeAreaView>
      <ScrollView
        contentInsetAdjustmentBehavior="automatic">
        <Header />
        <Body>
          <SectionContainer>
            <SectionDescription>one</SectionDescription>
            <SectionDescription>
              Edit <HighLight>App.js</HighLight> to change
            </SectionDescription>
          </SectionContainer>
          <SectionContainer>
            <SectionDescription>See Change</SectionDescription>
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
              Read the docs
            </SectionDescription>
          </SectionContainer>
        </Body>
      </ScrollView>
    </SafeAreaView>
  </Fragment>
  );
};

export default App;
```

```js
// index.js 파일 수정
import App from '~/App';
```

## 4. 개발

### 1) Weather API

- OpenWeather: https://openweathermap.org/api
- API 키 이메일로 받음

### 2) 위치 정보

- react -native-geolocation-service 라이브러리
  - https://github.com/Agontuk/react-native-geolocation-service

```bash
npm install -save react-native-geolocation-service
```

#### ios

- ./ios/Podfile 수정

```swift
target 'weatherapp' do
  config = use_native_modules!
  pod 'react-native-geolocation', path: '../node_modules/@react-native-community/geolocation'

  use_react_native!(
    
    :path => config[:reactNativePath],
    # to enable hermes on iOS, change `false` to `true` and then install pods
    :hermes_enabled => false
  )

  target 'weatherappTests' do
    inherit! :search_paths
    # Pods for testing
  end

  # Enables Flipper.
  #
  # Note that if you have use_frameworks! enabled, Flipper will not work and
  # you should disable the next line.
  use_flipper!()

  post_install do |installer|
    react_native_post_install(installer)
  end
end
```

- ./ios/weatherapp/Info.plist  수정

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>날씨 정보를 가져오기 위해서 위치정보 권한 필요합니다</string>
```

#### Android

- android\app\src\main\AndroidManifest.xml

```xml
 <uses-permission android:name="android.permission.INTERNET" />
 <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

### 3) WeatherView  컴포넌트

npm start --reset-cache //메트로 서버 캐시 초기화

1. *npm install --save-dev babel-plugin-styled-components*
2. create file: **.babelrc** with content: `{ "presets": ["next/babel"], "plugins": [["styled-components", { "ssr": true }]] }`