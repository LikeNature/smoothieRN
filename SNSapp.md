# SNS UI 클론 앱

## 1. 프로젝트 준비

```bash
react-native init snsapp
```

```bash
cd snsapp
yarn add styled-components
yarn add typescript @types/react @types/react-native @types/styled-components babel-plugin-root-import
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

```tsx
// TODO src 폴더 생성 => App.tsx 이름변경 => src 폴더로 이동 => App.tsx 파일 수정

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
// TODO index.js 파일 수정
import App from '~/App';
```

```bash
# 필요한 react-navigation(v5) 설치
yarn add @react-navigation/native react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view

# 사용할 내비게이션 설치
yarn add @react-navigation/stack @react-navigation/drawer @react-navigation/bottom-tabs

# AsyncStorage 설치
yarn add @react-native-community/async-storage

# 앱 아이콘과 스플래시 이미지 생성위한 react-native-make 라이브러리 설치
yarn add @bam.tech/react-native-make
yarn add react-native-splash-screen

```

## 2. 개발

### 1) 앱 아이콘

#### 이미지 다운로드 및  저장

- path : src\Assets\Images\app_icon.png

```bash
# 앱아이콘 적용
react-native set-icon --path ./src/Assets/Images/app_icon.png
```

### 2) 앱 스플래시 스크린 이미지

#### 이미지 다운로드 및  저장

- path: src\Assets\Images\app_splash.png

```swift
// iOS에 Storyboard 생성위해 ios/SNSApp.xcworkspace 파일 실행
// iOS/snsapp/AppDelegate.m 파일 열고 수정
#import "RNSplashScreen.h"

- (BOOL)application: (UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    ...
    [RNSplashScreen show];
    return YES;
}
```

```bash
# 앱 스플래시 스크린 이미지 적용
react-native set-splash --path ./src/Assets/Images/app_splash.png --resize cover
```

```java
// android\app\src\main\java\com\snsapp\MainActivity.java
// 스플래스 이미지 전체 표시 위해 수정

public class MainActivity extends ReactActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
//        SplashScreen.show(this, R.style.SplashScreenTheme);
            SplashScreen.show(this, true);
        super.onCreate(savedInstanceState);
    }
  /**
   * Returns the name of the main component registered from JavaScript. This is used to schedule
   * rendering of the component.
   */
  @Override
  protected String getMainComponentName() {
    return "snsapp";
  }
}
```

### 3) 내비게이션 컴포넌트

```tsx
// src\Screens\Navigator.tsx
import React, {useContext} from 'react';
import {Image} from 'react-native';                                         // 탭 내비게이션에 아이콘 표시하기 위함
import {NavigationContainer} from '@react-navigation/native';
import {createStackNavigator} from '@react-navigation/stack';
import {createDrawerNavigator} from '@react-navigation/drawer';
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';

// 화면에 표시할 컴포넌트 import
import {UserContext} from '~/Context/User';
import SearchBar from '~/Components/SearchBar';
import Loading from '~/Components/Loading';

import Login from '~/Screens/Login';
import PasswordReset from '~/Screens/PasswordReset';
import Signup from '~/Screens/Signup';

import MyFeed from '~/Screens/MyFeed';
import Feeds from '~/Screens/Feeds';
import FeedListOnly from '~/Screens/FeedListOnly';
import Upload from '~/Screens/Upload';
import Notification from '~/Screens/Notification';
import Profile from '~/Screens/Profile';
import CustomDrawer from '~/Screens/Drawer';

// 내비게이션 함수 호출해 컴포넌트 준비
const Stack = createStackNavigator();
const BottomTab = createBottomTabNavigator();
const Drawer = createDrawerNavigator();

// 내비게이션 헤더가 필요한 컴포넌트는 스택 네비게이션을 추가해야함
const LoginNavigator = () => {
  return (
    <Stack.Navigator screenOptions={{headerShown: false}}>
      <Stack.Screen name="Login" component={Login} />
      <Stack.Screen name="Signup" component={Signup} />
      <Stack.Screen name="PasswordReset" component={PasswordReset} />
    </Stack.Navigator>
  );
};

const MyFeedTab = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="MyFeed"
        component={MyFeed}
        options={{title: 'SNS App'}}
      />
    </Stack.Navigator>
  );
};

const FeedsTab = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Feeds"
        component={Feeds}
        options={{
          header: () => <SearchBar />,
        }}
      />
      <Stack.Screen
        name="FeedListOnly"
        component={FeedListOnly}
        options={{
          headerBackTitleVisible: false,
          title: '둘러보기',
          headerTintColor: '#292929',
        }}
      />
    </Stack.Navigator>
  );
};

const UploadTab = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Upload"
        component={Upload}
        options={{title: '사진 업로드'}}
      />
    </Stack.Navigator>
  );
};

const ProfileTab = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Profile"
        component={Profile}
        options={{title: 'Profile'}}
      />
    </Stack.Navigator>
  );
};

// 탭 라벨 제거위해 tabBarOptions의 showLabel을 false로 설정
const MainTabs = () => {
  return (
    <BottomTab.Navigator tabBarOptions={{showLabel: false}}>
      <BottomTab.Screen
        name="MyFeed"
        component={MyFeedTab}
        options={{
          tabBarIcon: ({color, focused}) => (
            <Image
              source={
                focused
                  ? require('~/Assets/Images/Tabs/ic_home.png')
                  : require('~/Assets/Images/Tabs/ic_home_outline.png')
              }
            />
          ),
        }}
      />
      <BottomTab.Screen
        name="Feeds"
        component={FeedsTab}
        options={{
          tabBarIcon: ({color, focused}) => (
            <Image
              source={
                focused
                  ? require('~/Assets/Images/Tabs/ic_search.png')
                  : require('~/Assets/Images/Tabs/ic_search_outline.png')
              }
            />
          ),
        }}
      />
      <BottomTab.Screen
        name="Upload"
        component={UploadTab}
        options={{
          tabBarLabel: 'Third',
          tabBarIcon: ({color, focused}) => (
            <Image
              source={
                focused
                  ? require('~/Assets/Images/Tabs/ic_add.png')
                  : require('~/Assets/Images/Tabs/ic_add_outline.png')
              }
            />
          ),
        }}
      />
      <BottomTab.Screen
        name="Notification"
        component={Notification}
        options={{
          tabBarIcon: ({color, focused}) => (
            <Image
              source={
                focused
                  ? require('~/Assets/Images/Tabs/ic_favorite.png')
                  : require('~/Assets/Images/Tabs/ic_favorite_outline.png')
              }
            />
          ),
        }}
      />
      <BottomTab.Screen
        name="Profile"
        component={ProfileTab}
        options={{
          tabBarIcon: ({color, focused}) => (
            <Image
              source={
                focused
                  ? require('~/Assets/Images/Tabs/ic_profile.png')
                  : require('~/Assets/Images/Tabs/ic_profile_outline.png')
              }
            />
          ),
        }}
      />
    </BottomTab.Navigator>
  );
};

// drawerPosition="right" : 드로어 내비게이션을 오른쪽에 표시하기 위해  설정
// drawerType="slide" : 메뉴가 화면 위가 아닌 화면 전체를 이동시키면서 표시되게 하기 위해 설정
// Drawer.Navigator 는 자식 컴포넌트 설정하면 자동으로 드로어 내비게이션 만들어줌
// 여기에서는 직접 만든 Drawer 메뉴 사용하도록 설정
const MainNavigator = () => {
  return (
    <Drawer.Navigator
      drawerPosition="right"
      drawerType="slide"
      drawerContent={(props) => <CustomDrawer props={props} />}>
      <Drawer.Screen name="MainTabs" component={MainTabs} />
    </Drawer.Navigator>
  );
};

// 로그인 구현

export default () => {
  const {isLoading, userInfo} = useContext<IUserContext>(UserContext);

  if (isLoading === false) {
    return <Loading />;
  }
  return (
    <NavigationContainer>
      {userInfo ? <MainNavigator /> : <LoginNavigator />}
    </NavigationContainer>
  );
};
```

```tsx
// src\Screens\@types\index.d.ts
// 각 화면에서 사용할 navigation Props 타입 정의
// 상세 페이지 표시할 때 ID 매개변수가 없으므로 undefined 대입

type LoginNaviParamList = {
  Login: undefined;
  Signup: undefined;
  PasswordReset: undefined;
};

type MyFeedTabParamList = {
  MyFeed: undefined;
};

type FeedsTabParamList = {
  Feeds: undefined;
  FeedListOnly: undefined;
};

type ProfileTabParamList = {
  Profile: undefined;
};
```

### 4) App 컴포넌트

```tsx
// App.tsx 수정
import React from 'react';
import {StatusBar} from 'react-native';

import Navigator from '~/Screens/Navigator';
import {UserContextProvider} from '~/Context/User';
import {RandomUserDataProvider} from '~/Context/RandomUserData';

interface Props {}

const App = ({}: Props) => {
  return (
    <RandomUserDataProvider cache={true}>
      <UserContextProvider>
        <StatusBar barStyle="default" />
        <Navigator />
      </UserContextProvider>
    </RandomUserDataProvider>
  );
};
export default App;
```

### 5) 컨텍스트

```tsx
// src\Context\User\@types\index.d.ts
// 로그인, 로그아웃, 사용자 정보를 가지고 있는 User 컨텍스트 사용예정

interface IUserInfo {
  name: string;
  email: string;
}

interface IUserContext {
  isLoading: boolean;
  userInfo: IUserInfo | undefined;
  login: (email: string, password: string) => void;
  getUserInfo: () => void;
  logout: () => void;
}
```

```tsx
// src\Context\User\index.tsx
// User 컨텍스트 구현

import React, {createContext, useState, useEffect} from 'react';
import AsyncStorage from '@react-native-community/async-storage';

const defaultContext: IUserContext = {
  isLoading: false,
  userInfo: undefined,
  login: (email: string, password: string) => {},
  getUserInfo: () => {},
  logout: () => {},
};

const UserContext = createContext(defaultContext);

interface Props {
  children: JSX.Element | Array<JSX.Element>;
}

const UserContextProvider = ({children}: Props) => {
  const [userInfo, setUserInfo] = useState<IUserInfo | undefined>(undefined);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  const login = (email: string, password: string): void => {
    // Use Eamil and Passowrd for login API
    // Get token and UserInfo via Login API
    AsyncStorage.setItem('token', 'save your token').then(() => {
      setUserInfo({
        name: 'dev-yakuza',
        email: 'dev.yakuza@gamil.com',
      });
      setIsLoading(true);
    });
  };

  const getUserInfo = (): void => {
    AsyncStorage.getItem('token')
      .then(value => {
        if (value) {
          setUserInfo({
            name: 'dev-yakuza',
            email: 'dev.yakuza@gamil.com',
          });
        }
        setIsLoading(true);
      })
      .catch(() => {
        setUserInfo(undefined);
        setIsLoading(true);
      });
  };

  const logout = (): void => {
    AsyncStorage.removeItem('token');
    setUserInfo(undefined);
  };

  useEffect(() => {
    getUserInfo();
  }, []);

  return (
    <UserContext.Provider
      value={{
        isLoading,
        userInfo,
        login,
        getUserInfo,
        logout,
      }}>
      {children}
    </UserContext.Provider>
  );
};
export {UserContextProvider, UserContext};
```

```tsx
// src\Context\RandomUserData\@types\index.d.ts
// 화면에 표시될 이미지, 사용자 정보 등을 API로 가져오고 랜덤으로 표시되는 컨텍스트
// IUserProfile 은 사용자 이름과 이미지 저장하기 위한 타입
interface IUserProfile {
  name: string;
  photo: string;
}
// IFeed 는 사용자 타입 확정해 이미지리스트와 설명문 추가할수 있도록 선언
interface IFeed extends IUserProfile {
  images: Array<string>;
  description: string;
}
```

```tsx
// src\Context\RandomUserData\index.tsx
// src\Context\RandomUserData\@types\index.d.ts 구현

import React, {createContext, useState, useEffect} from 'react';
import {Image} from 'react-native';
import AsyncStorage from '@react-native-community/async-storage';

import Loading from '~/Components/Loading';

interface Props {
  cache?: boolean;
  children: JSX.Element | Array<JSX.Element>;
}

interface IRnadomUserData {
  getMyFeed: (number?: number) => Array<IFeed>;
}

const RandomUserDataContext = createContext<IRnadomUserData>({
  getMyFeed: (number: number = 10) => {
    return [];
  },
});

// userList: 사용자의 이름과 이미지를 가지는 사용자 리스트
// descriptionList: 사용자의 이미지에 대한 설명을 가지고 있는 설명문 리스트
// imageList: 사용자의 이미지를 가지고 있는 리스트
// 각각의 리스트는 25개 데이터 준비, 데이터들을 랜덤으로 조합해 화면에 표시
// 데이터는 Fetch API를 통해 가져오며, useState를 통해 만든 state에 할당
// 캐싱이 필요한 경우 AsyncStorage 를 통해 데이터 캐싱
const RandomUserDataProvider = ({cache, children}: Props) => {
  const [userList, setUserList] = useState<Array<IUserProfile>>([]);
  const [descriptionList, setDescriptionList] = useState<Array<string>>([]);
  const [imageList, setImageList] = useState<Array<string>>([]);


  const getCacheData = async (key: string) => {
    const cacheData = await AsyncStorage.getItem(key);
    // getCacheData는 컨텍스트의 Props인 cache를 false로 선언하였거나 이전에 캐싱한 데이터가 없는 경우, Fetch API를 통해 새롭게 가져옴
    if (cache === false || cacheData === null) {
      return undefined;
    }
    // AsycStorage는 문자열만 저장할 수 있으므로 캐싱한 데이터를 JSON.parse를 통해 문자열 리스트로 복원
    const cacheList = JSON.parse(cacheData);

    // 복원한 캐싱 데이터가 25개인지 판단해 25개가 아닌경우, 다시 Fetch API를 통해 데이터 가져옴
    if (cacheList.length !== 25) {
      return undefined;
    }

    return cacheList;
  };
  // setCachedData는 전달받은 배열데이터를 문자열로 변환해 AsyncStorage에 저장
  const setCachedData = (key: string, data: Array<any>) => {
    AsyncStorage.setItem(key, JSON.stringify(data));
  };

    // AsyncStorage에 저장된 데이터를 useState로 생성한 State에 할당
    // setUsers 함수는 아래 useEffect를 통해, 컴포넌트가 화면에 표시된 후, 한번만 실행되도록 설정
  const setUsers = async () => {
    const cachedData = await getCacheData('UserList');
    if (cachedData) {
      setUserList(cachedData);
      return;
    }

    try {
        // 사용자리스트는 미리지 준비한 JSON 데이터 사용해 가져옴
      const response = await fetch(
        'https://raw.githubusercontent.com/dev-yakuza/users/master/api.json',
      );
      const data = await response.json();
      setUserList(data);
      setCachedData('UserList', data);
    } catch (error) {
      console.log(error);
    }
  };

    // 
  const setDescriptions = async () => {
    // 데이터가 없는 경우 setUsers 함수와 동일하게 캐싱 데이터를 확인하고 나서 데이터가 없는경우 Fetch API를 통해 데이터 가져옴
    const cachedData = await getCacheData('DescriptionList');
    console.log(cachedData);
    if (cachedData) {
      setDescriptionList(cachedData);
      return;
    }
    // 설명문 리스트 데이터는 Optionated Quotes API에서 제공하는 무료 API를 통해 가져옴
    try {
      const response = await fetch(
        'https://opinionated-quotes-api.gigalixirapp.com/v1/quotes?rand=t&n=25',
      );
      const data = await response.json();

      let text = [];
      for (const index in data.quotes) {
        text.push(data.quotes[index].quote);
      }
        // Optionated Quotes API 를 통해 습득한 데이터를 useState를 통해 state에 할당하고, AsyncStorage에 캐싱한 후, 다시 랜덤하게 데이터 만들어 화면에 표시
      setDescriptionList(text);
      setCachedData('DescriptionList', text);
    } catch (error) {
      console.log(error);
    }
  };

   
 
  const setImages = async () => {
    const cachedData = await getCacheData('ImageList');
    // 가져온 데이터는 이미 이전에 가져온 데이터와 동일한 경우, 해당 데이터 저장하지 않고 다시 fetch 로 가져옴
    if (cachedData) {
      if (Image.queryCache) {
        Image.queryCache(cachedData);
        cachedData.map((data: string) => {
          Image.prefetch(data);
        });
      }
      setImageList(cachedData);
      return;
    }

    setTimeout(async () => {
          // 이미지리스트 데이터는 unsplash에서 제공하는 API를 통해 가져오도록 설정
          // 이 API는 랜덤으로 이미지 한장만 얻을 수 있으며, 일정 시간 내에 보내는 요청은 동일한 이미지가 반환됨
            // useState로 생성한 State에 데이터 저장하고 AsyncStorage에 데이터 캐싱하는 부분은 동일
      try {
        const response = await fetch('https://source.unsplash.com/random/');
        const data = response.url;
        if (imageList.indexOf(data) >= 0) {
          setImages();
          return;
        }
        setImageList([...imageList, data]);
      } catch (error) {
        console.log(error);
      }
    }, 400);
  };

  useEffect(() => {
    setUsers();
    setDescriptions();
  }, []);

    // API를 통해 습득가능한 이미지는 하나이므로 useEffect를 통해 ImageList에 25개보다 적은 경우 반복해서 데이터 습득
  useEffect(() => {
    if (imageList.length !== 25) {
      setImages();
    } else {
        // 만족할 경우 setCachedData를 통해 데이터 캐싱
      setCachedData('ImageList', imageList);
    }
  }, [imageList]);

    // 이미지 랜덤하게 가져오도록 설정
  const getImages = (): Array<string> => {
    let images: Array<string> = [];
    // Math.random()은 반환값으로 0 이상 1 미만의 부동소숫점 의사 난수.
    // Math.floor(x) 주어진 수 이하의 가장 큰 정수.
    // 최대 5개의 이미지 랜덤하게 가짐
    const count = Math.floor(Math.random() * 4);

    for (let i = 0; i <= count; i++) {
        // 개수만큼의 이미지를 25개 이미지 리스트에서 랜덤하게 가져옴
      images.push(imageList[Math.floor(Math.random() * 24)]);
    }

    return images;
  };
  // 사용자가 업로드한 하나의 피드에 여러 이미지 표시
  // 몇 개의 피드 데이터가 필요한지 매개변수 number로 받아옴, 기본값 10
  // 피드데이터는 src\Context\RandomUserData\@types\index.d.ts 에서 정의한 IFeed 처럼, 이름, 프로필 이미지, 피드 설명문, 피드 이미지 가지고 있음
  const getMyFeed = (number: number = 10): Array<IFeed> => {
    let feeds: Array<IFeed> = [];
    for (let i = 0; i < number; i++) {
      const user = userList[Math.floor(Math.random() * 24)];
      feeds.push({
        name: user.name,
        photo: user.photo,
        description: descriptionList[Math.floor(Math.random() * 24)],
        images: getImages(),
      });
    }
    return feeds;
  };

  console.log(
    `${userList.length} / ${descriptionList.length} / ${imageList.length}`,
  );
  // 데이터 개수 비교해 25개가 아닌경우, 로딩화면 표시
  // 사용자리스트, 이미지리스트, 설명문 리스트가 모두 25개인 경우, 자식 컴포넌트를 화면에 표시
  return (
    <RandomUserDataContext.Provider
      value={{
        getMyFeed,
      }}>
      {userList.length === 25 &&
      descriptionList.length === 25 &&
      imageList.length === 25 ? (
        children
      ) : (
        <Loading />
      )}
    </RandomUserDataContext.Provider>
  );
};

export {RandomUserDataProvider, RandomUserDataContext};
```

### 6) Loading 컴포넌트

```tsx
// src\Component\Loading\index.tsx
// 사용자 로그인 여부 확인시 표시할 Loading 컴포넌트

import React from 'react';
import {ActivityIndicator} from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.View`
  flex: 1;
  background-color: #FEFFFF;
  align-items: center;
  justify-content: center;
`;

const Loading = () => {
  return (
    <Container>
      <ActivityIndicator color="#D3D3D3" size="large" />
    </Container>
  );
};

export default Loading;
```

### 7) Login 컴포넌트

```tsx
// src\Screens\Login\index.tsx

// src\Screens\Navigator.tsx 에서 LoginNavigator 는 Login, Signup, PasswordReset 컴포넌트로 구성


import React, {useContext, useEffect} from 'react';
import Styled from 'styled-components/native';
import {StackNavigationProp} from '@react-navigation/stack';
import SplashScreen from 'react-native-splash-screen';

import {UserContext} from '~/Context/User';

import Input from '~/Components/Input';
import Button from '~/Components/Button';

const Container = Styled.SafeAreaView`
  flex: 1;
  background-color: #FEFFFF;
`;
const FormContainer = Styled.View`
  flex: 1;
  width: 100%;
  align-items: center;
  justify-content: center;
  padding: 32px;
`;

const Logo = Styled.Text`
  color: #292929;
  font-size: 40px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 40px;
`;

const PasswordReset = Styled.Text`
  width: 100%;
  color: #3796EF;
  text-align: right;
  margin-bottom: 24px;
`;

const SignupText = Styled.Text`
  color: #929292;
  text-align: center;
`;
const SignupLink = Styled.Text`
  color: #3796EF;
`;

const Footer = Styled.View`
  width: 100%;
  border-top-width: 1px;
  border-color: #D3D3D3;
  padding: 8px;
`;
const Copyright = Styled.Text`
  color: #929292;
  text-align: center;
`;

type NavigationProp = StackNavigationProp<LoginNaviParamList, 'Login'>;
interface Props {
  navigation: NavigationProp;
}

// 로그인 버튼 누르면 아래 함수 실행해 메인화면으로 이동
// 로그인 화면이기 때문에 splash 이미지 닫아줌
// 모바일 앱 실행 시 가장 처음 만나게 되는, 스플래시 화면
const Login = ({navigation}: Props) => {
  const {login} = useContext<IUserContext>(UserContext);

  useEffect(() => {
    SplashScreen.hide();
  }, []);

// Login 컴포넌트는 크게 로그인 폼과 하단의 Footer로 구성
// Login 화면에서 다른 화면으로 전환 가능하도록 구현
// 패스워드 잊어버린 경우, PasswordRest과 Signup 으로 이동하도록 구성
  return (
    <Container>
      <FormContainer>
        <Logo>SNS App</Logo>
        <Input style={{marginBottom: 16}} placeholder="이메일" />
        <Input
          style={{marginBottom: 16}}
          placeholder="비밀번호"
          secureTextEntry={true}
        />
        <PasswordReset onPress={() => navigation.navigate('PasswordReset')}>
          비밀번호 재설정
        </PasswordReset>
        <Button
          label="로그인"
          style={{marginBottom: 24}}
          onPress={() => {
            login('dev.yakuza@gmail.com', 'password');
          }}
        />
        <SignupText>
          계정이 없으신가요?{' '}
          <SignupLink onPress={() => navigation.navigate('Signup')}>
            가입하기.
          </SignupLink>
        </SignupText>
      </FormContainer>
      <Footer>
        <Copyright>SNSApp from dev-yakuza</Copyright>
      </Footer>
    </Container>
  );
};

export default Login;
```

### 8) Input 컴포넌트

```tsx
// src\Component\Input\index.tsx
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.View`
  width: 100%;
  height: 40px;
  padding-left: 16px;
  padding-right: 16px;
  border-radius: 4px;
  background-color: #FAFAFA;
  border-width: 1px;
  border-color: #D3D3D3;
`;
const InputField = Styled.TextInput`
  flex: 1;
  color: #292929;
`;

interface Props {
  placeholder?: string;
  keyboardType?: 'default' | 'email-address' | 'numeric' | 'phone-pad';
  secureTextEntry?: boolean;
  style?: Object;
  clearMode?: boolean;
  onChangeText?: (text: string) => void;
}

const Input = ({
  placeholder,
  keyboardType,
  secureTextEntry,
  style,
  clearMode,
  onChangeText,
}: Props) => {
  return (
    <Container style={style}>
      <InputField
        selectionColor="#292929"
        secureTextEntry={secureTextEntry}
        keyboardType={keyboardType ? keyboardType : 'default'}
        autoCapitalize="none"
        autoCorrect={false}
        allowFontScaling={false}
        placeholderTextColor="#C3C2C8"
        placeholder={placeholder}
        clearButtonMode={clearMode ? 'while-editing' : 'never'}
        onChangeText={onChangeText}
      />
    </Container>
  );
};

export default Input;
```

### 9) Button 컴포넌트

```tsx
// src\Component\Button\index.tsx
import React from 'react';
import Styled from 'styled-components/native';

const StyleButton = Styled.TouchableOpacity`
  width: 100%;
  height: 40px;
  border-radius: 4px;
  justify-content: center;
  align-items: center;
  background-color: #3796EF;
`;
const Label = Styled.Text`
  color: #FFFFFF;
`;

interface Props {
  label: string;
  style?: Object;
  color?: string;
  onPress?: () => void;
}

const Button = ({ label, style, color, onPress }: Props) => {
  return (
    <StyleButton style={style} onPress={onPress}>
      <Label style={{ color: color ? color : '#FFFFFF' }}>{label}</Label>
    </StyleButton>
  );
};

export default Button;
```

### 10) PasswordReset 컴포넌트

```tsx
// src\Screens\PasswordReset\index.tsx
import React, {useState} from 'react';
import {StackNavigationProp} from '@react-navigation/stack';

import Styled from 'styled-components/native';

import Input from '~/Components/Input';
import Button from '~/Components/Button';
import Tab from '~/Components/Tab';

const Container = Styled.SafeAreaView`
  flex: 1;
  background-color: #FEFFFF;
`;

const FormContainer = Styled.View`
  flex: 1;
  width: 100%;
  align-items: center;
  padding: 32px;
`;

const LockImageContainer = Styled.View`
  padding: 24px;
  border-width: 2px;
  border-color: #292929;
  border-radius: 80px;
  margin-bottom: 24px;
`;
const LockImage = Styled.Image``;
const Title = Styled.Text`
  font-size: 16px;
  margin-bottom: 16px;
`;
const Description = Styled.Text`
  text-align: center;
  margin-bottom: 16px;
  color: #292929;
`;
const TabContainer = Styled.View`
  flex-direction: row;
  margin-bottom: 16px;
`;
const HelpLabel = Styled.Text`
  color: #3796EF;
`;
const Footer = Styled.View`
  width: 100%;
  border-top-width: 1px;
  border-color: #D3D3D3;
  padding: 8px;
`;
const GoBack = Styled.Text`
  color: #3796EF;
  text-align: center;
`;

type NavigationProp = StackNavigationProp<LoginNaviParamList, 'PasswordReset'>;
interface Props {
  navigation: NavigationProp;
}

// useStae를 사용해 사용자이름, 전화번호 탭 구성, 탭 선택시 설명문과 일력 창에 표시될 문구 변경
const PasswordReset = ({navigation}: Props) => {
  const [tabIndex, setTabIndex] = useState<number>(0);
  const tabs = ['사용자 이름', '전화번호'];
  const tabDescriptions = [
    '사용자 이름 또는 이메일을 입력하면 다시 계정에 로그인 할 수 있는 링크를 보내드립니다',
    '전화번호를 입력하면 계정에 다시 액세스할 수 있는 코드를 보내드립니다.',
  ];
  const placeholders = ['사용자 이름 또는 이메일', '전화번호'];

    // navigation.goBack() 을 사용해 이벤트에 돌아가기 버튼 구현
  return (
    <Container>
      <FormContainer>
        <LockImageContainer>
          <LockImage source={require('~/Assets/Images/lock.png')} />
        </LockImageContainer>
        <Title>로그인에 문제가 있나요?</Title>
        <Description>{tabDescriptions[tabIndex]}</Description>
        <TabContainer>
          {tabs.map((label: string, index: number) => (
            <Tab
              key={`tab-${index}`}
              selected={tabIndex === index}
              label={label}
              onPress={() => setTabIndex(index)}
            />
          ))}
        </TabContainer>
        <Input
          style={{marginBottom: 16}}
          placeholder={placeholders[tabIndex]}
        />
        <Button label="다음" style={{marginBottom: 24}} />
        <HelpLabel>도움이 더 필요하세요?</HelpLabel>
      </FormContainer>
      <Footer>
        <GoBack onPress={() => navigation.goBack()}>로그인으로 돌아가기</GoBack>
      </Footer>
    </Container>
  );
};

export default PasswordReset;
```

### 11) Tab 컴포넌트

```tsx
// src\Component\Tab\index.tsx
import React from 'react';
import { ImageSourcePropType } from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.Touchablimport React from 'react';
import { ImageSourcePropType } from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.TouchableOpacity`
  flex: 1;
  border-bottom-width: 1px;
  border-color: #929292;
  padding-bottom: 8px;
  align-items: center;
  justify-content: center;
`;
const Label = Styled.Text`
  font-size: 16px;
  color: #929292;
  text-align: center;
`;
const TabImage = Styled.Image`
  margin-top: 8px;
`;

interface Props {
  selected: boolean;
  label?: string;
  imageSource?: ImageSourcePropType;
  onPress?: () => void;
}

const Tab = ({ selected, label, imageSource, onPress }: Props) => {
  let color: string = selected ? '#292929' : '#929292';

//  부모로부터 전달받은 Props인 selectd에 따라 글자색과 테두리색 변경
// label로 문자열 전달받으면 문자열 화면에 표시
// imageSource 를 통해 이미지 전달받으면 이미지 화면에 표시
  return (
    <Container
      activeOpacity={1}
      style={{ borderColor: color }}
      onPress={onPress}>
      {label && <Label style={{ color: color }}>{label}</Label>}
      {imageSource && <TabImage source={imageSource} />}
    </Container>
  );
};

export default Tab;
```

### 12) Signup 컴포넌트

```tsx
// src\Screens\Signup\index.tsx
import React, {useState} from 'react';
import {StackNavigationProp} from '@react-navigation/stack';
import Styled from 'styled-components/native';

import Input from '~/Components/Input';
import Button from '~/Components/Button';
import Tab from '~/Components/Tab';

const Container = Styled.SafeAreaView`
  flex: 1;
  background-color: #FEFFFF;
`;

const FormContainer = Styled.View`
  flex: 1;
  width: 100%;
  align-items: center;
  padding: 32px;
`;
const Description = Styled.Text`
  text-align: center;
  font-size: 12px;
  color: #929292;
  margin: 0px 8px;
`;
const TabContainer = Styled.View`
  flex-direction: row;
  margin-bottom: 16px;
`;
const Footer = Styled.View`
  width: 100%;
  border-top-width: 1px;
  border-color: #D3D3D3;
  padding: 8px;
`;
const FooterDescription = Styled.Text`
  color: #929292;
  text-align: center;
`;
const GoBack = Styled.Text`
  color: #3796EF;
`;

type NavigationProp = StackNavigationProp<LoginNaviParamList, 'Signup'>;
interface Props {
  navigation: NavigationProp;
}

const Signup = ({navigation}: Props) => {
  const [tabIndex, setTabIndex] = useState<number>(0);
  const tabs = ['전화번호', '이메일'];

  return (
    <Container>
      <FormContainer>
        <TabContainer>
          {tabs.map((label: string, index: number) => (
            <Tab
              key={`tab-${index}`}
              selected={tabIndex === index}
              label={label}
              onPress={() => setTabIndex(index)}
            />
          ))}
        </TabContainer>
        <Input
          style={{marginBottom: 16}}
          placeholder={tabIndex === 0 ? '전화번호' : '이메일'}
        />
        <Button label="다음" style={{marginBottom: 24}} />
        {tabIndex === 0 && (
          <Description>
            SNS App의 업데이트 내용을 SMS로 수신할 수 있으며, 언제든지 수신을
            취소할 수 있습니다.
          </Description>
        )}
      </FormContainer>
      <Footer>
        <FooterDescription>
          이미 계정이 있으신가요?{' '}
          <GoBack onPress={() => navigation.goBack()}>로그인</GoBack>
        </FooterDescription>
      </Footer>
    </Container>
  );
};

export default Signup;
```

### 13) MyFeed 컴포넌트

```tsx
// src\Screens\MyFeed\index.tsx

import React, {useContext, useState, useEffect} from 'react';
import {StackNavigationProp} from '@react-navigation/stack';
import {FlatList} from 'react-native';
import Styled from 'styled-components/native';
import SplashScreen from 'react-native-splash-screen';

const HeaderRightContainer = Styled.View`
  flex-direction: row;
`;

import {RandomUserDataContext} from '~/Context/RandomUserData';
import IconButton from '~/Components/IconButton';
import Feed from '~/Components/Feed';

import StoryList from './StoryList';

type NavigationProp = StackNavigationProp<MyFeedTabParamList, 'MyFeed'>;
interface Props {
  navigation: NavigationProp;
}

const MyFeed = ({navigation}: Props) => {
  const {getMyFeed} = useContext(RandomUserDataContext);
  const [feedList, setFeedList] = useState<Array<IFeed>>([]);
  const [storyList, setStoryList] = useState<Array<IFeed>>([]);
  const [loading, setLoading] = useState<boolean>(false);

  React.useLayoutEffect(() => {
    navigation.setOptions({
      headerLeft: () => <IconButton iconName="camera" />,
      headerRight: () => (
        <HeaderRightContainer>
          <IconButton iconName="live" />
          <IconButton iconName="send" />
        </HeaderRightContainer>
      ),
    });
  }, []);

 // MyFeed는 로그인한 사용자가 가장먼저 보는 화면이기 때문에 스플래시 이미지 닫아줌
  useEffect(() => {
    setFeedList(getMyFeed());
    setStoryList(getMyFeed());
    SplashScreen.hide();
  }, []);

    // getMyFeed 함수를 사용해 데이터를 가져와 State에 값 설정
    // FlatList onRefresh 사용해 새로고침 구현 / 이때 헤더의 스토리 리스트와 피드 리스트 다시 가져와 화면 갱신
    // onEndReached 사용해 스크롤이 최하단으로 이동했을 때, 데이터 다시 가져와 추가함으로써 무한 스크롤 구현
    // FlatList renderItem 에 공통 컴포넌트 설정해 컨텍스트로부터 얻은 랜덤 데이터 화면에 표시
    // ListHeaderComponent 에 MyFeed에 종속되어 있는  StoryList 컴포넌트 설정
  return (
    <FlatList
      data={feedList}
      keyExtractor={(item, index) => {
        return `myfeed-${index}`;
      }}
      showsVerticalScrollIndicator={false}
      onRefresh={() => {
        setLoading(true);
        setTimeout(() => {
          setFeedList(getMyFeed());
          setStoryList(getMyFeed());
          setLoading(false);
        }, 2000);
      }}
      onEndReached={() => {
        setFeedList([...feedList, ...getMyFeed()]);
      }}
      onEndReachedThreshold={0.5}
      refreshing={loading}
      ListHeaderComponent={<StoryList storyList={storyList} />}
      renderItem={({item, index}) => (
        <Feed
          id={index}
          name={item.name}
          photo={item.photo}
          description={item.description}
          images={item.images}
        />
      )}
    />
  );
};

export default MyFeed;
```

### 14) StoryList 컴포넌트

```tsx
// src\Screens\MyFeed\StoryList\index.tsx
// 부모컴포넌트인  MyFeed 로부터 Props를 통해 화면에 표시될 데이터(storyList) 전달받음
// 가로로 스크롤 될 수 있도록 FlatList 사용

import React from 'react';
import { FlatList } from 'react-native';

import Styled from 'styled-components/native';

const StoryContainer = Styled.View`
  padding: 8px;
  width: 72px;
`;
const Story = Styled.View`
  width: 56px;
  height: 56px;
  border-radius: 56px;
  overflow: hidden;
  align-items: center;
  justify-content: center;
`;
const StoryBackground = Styled.Image`
  position: absolute;
`;
const StoryImage = Styled.Image`
  width: 50px;
  height: 50px;
  border-radius: 50px;
`;
const StoryName = Styled.Text`
  width: 100%;
  text-align: center;
`;

interface Props {
  storyList: Array<IFeed>;
}

const StoryList = ({ storyList }: Props) => {
  return (
    <FlatList
      data={storyList}
      horizontal={true}
      showsHorizontalScrollIndicator={false}
      keyExtractor={(item, index) => {
        return `story-${index}`;
      }}
      renderItem={({ item, index }) => (
        <StoryContainer>
          <Story>
            <StoryBackground
              source={require('~/Assets/Images/story_background.png')}
            />
            <StoryImage
              source={{ uri: item.photo }}
              style={{ width: 52, height: 52 }}
            />
          </Story>
          <StoryName numberOfLines={1}>{item.name}</StoryName>
        </StoryContainer>
      )}
    />
  );
};

export default StoryList;
```

### 15) IconButton 컴포넌트

```tsx
// src\Component\IconButton\index.tsx
// Props를 통해,부모 컴포넌트로부터 지정된 아이콘 이미지 이름을 전달받아, 해당 아이콘 이미지를 표시
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.TouchableOpacity`
  padding: 8px;
`;
const Icon = Styled.Image`
`;

interface Props {
  iconName:
    | 'camera'
    | 'live'
    | 'send'
    | 'dotMenu'
    | 'favorite'
    | 'comment'
    | 'bookmark'
    | 'menu';
  style?: object;
  onPress?: () => void;
}

const IconButton = ({ iconName, style, onPress }: Props) => {
  const imageSource = {
    camera: require('~/Assets/Images/ic_camera.png'),
    live: require('~/Assets/Images/ic_live.png'),
    send: require('~/Assets/Images/ic_send.png'),
    dotMenu: require('~/Assets/Images/ic_dot_menu.png'),
    favorite: require('~/Assets/Images/Tabs/ic_favorite_outline.png'),
    comment: require('~/Assets/Images/ic_comment.png'),
    bookmark: require('~/Assets/Images/ic_bookmark.png'),
    menu: require('~/Assets/Images/ic_menu.png'),
  };

  return (
    <Container
      style={style}
      onPress={() => {
        if (onPress && typeof onPress === 'function') {
          onPress();
        }
      }}>
      <Icon source={imageSource[iconName]} />
    </Container>
  );
};

export default IconButton;
```

### 16) Feed 컴포넌트

```tsx
// src\Component\IconButton\index.tsx

// Props를 통해,부모 컴포넌트로부터 지정된 아이콘 이미지 이름을 전달받아, 해당 아이콘 이미지를 표시
// 부모 컴포넌트로부터의 Props를 통해 사용자 프로필 정보 표시하는 헤더, 사용자 이미지 리스트를 표시하는 Body, 마지막으로 피드에 대한 설명을 표시하는 풋터로 나누어 표시
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.TouchableOpacity`
  padding: 8px;
`;
const Icon = Styled.Image`
`;

interface Props {
  iconName:
    | 'camera'
    | 'live'
    | 'send'
    | 'dotMenu'
    | 'favorite'
    | 'comment'
    | 'bookmark'
    | 'menu';
  style?: object;
  onPress?: () => void;
}

const IconButton = ({ iconName, style, onPress }: Props) => {
  const imageSource = {
    camera: require('~/Assets/Images/ic_camera.png'),
    live: require('~/Assets/Images/ic_live.png'),
    send: require('~/Assets/Images/ic_send.png'),
    dotMenu: require('~/Assets/Images/ic_dot_menu.png'),
    favorite: require('~/Assets/Images/Tabs/ic_favorite_outline.png'),
    comment: require('~/Assets/Images/ic_comment.png'),
    bookmark: require('~/Assets/Images/ic_bookmark.png'),
    menu: require('~/Assets/Images/ic_menu.png'),
  };

  return (
    <Container
      style={style}
      onPress={() => {
        if (onPress && typeof onPress === 'function') {
          onPress();
        }
      }}>
      <Icon source={imageSource[iconName]} />
    </Container>
  );
};

export default IconButton;
```

### 17) FeedBody 컴포넌트

```tsx
// src\Component\Feed\FeedBody\index.tsx

// 사용자 이미지 리스트 표시할 FeedBody 컴포넌트

import React, { useState } from 'react';
import {
  Dimensions,
  ScrollView,
  NativeSyntheticEvent,
  NativeScrollEvent,
  Image,
} from 'react-native';

import Styled from 'styled-components/native';

import IconButton from '~/Components/IconButton';

const Container = Styled.View``;

const ImageContainer = Styled.View`
  border-top-width: 1px;
  border-bottom-width: 1px;
  border-color: #D3D3D3;
  width: ${Dimensions.get('window').width}px;
  height: 400px;
`;

const FeedImageIndicatorContainer = Styled.View`
  flex: 1;
  flex-direction: row;
  justify-content: center;
  align-items: center;
`;
const FeedImageIndicator = Styled.View`
  width: 8px;
  height: 8px;
  border-radius: 8px;
  margin: 2px;
`;

const FeedMenuContainer = Styled.View`
  flex-direction: row;
`;
const MenuContainer = Styled.View`
  flex: 1;
  flex-direction: row;
`;

interface Props {
  id: number;
  images: Array<String>;
}

// 부모컴포넌트로 전달받은 이미지 리스트 표시하고, 이미지 리스트의 현재 위치 표시하는 인디케이터 다루기 위해 useState 사용
const FeedBody = ({ id, images }: Props) => {
  const [indicatorIndex, setIndicatorIndex] = useState<number>(0);
  const imageLength = images.length;

    // pagingEnabled : 이미지리스트를 페이지 형식으로 스크롤하기 위해 true 설정
    // showsHorizontalScrollIndicator: ScrollView가 기본으로 제공한느 인디케이터 비활성화
    // scrollEnabled: 이미지가 한장있는 경우, 스크롤 되지 않게 설정
    // onScroll 이벤트 발생하면 현재 스크롤 위치 구함 -> useState를 사용하여 setIndicatorIndex 함수를 통해 설정
  return (
    <Container>
      <ScrollView
        horizontal={true}
        pagingEnabled={true}
        showsHorizontalScrollIndicator={false}
        scrollEnabled={imageLength > 1}
        onScroll={(event: NativeSyntheticEvent<NativeScrollEvent>) => {
          setIndicatorIndex(
            event.nativeEvent.contentOffset.x / Dimensions.get('window').width
          );
        }}>
        {images.map((image, index) => (
          <ImageContainer key={`FeedImage-${id}-${index}`}>
            <Image
              source={{ uri: image as string }}
              style={{ width: Dimensions.get('window').width, height: 400 }}
            />
          </ImageContainer>
        ))}
      </ScrollView>
      <FeedMenuContainer>
        <MenuContainer>
          <IconButton iconName="favorite" />
          <IconButton iconName="comment" />
          <IconButton iconName="send" />
        </MenuContainer>
        <MenuContainer>
          <FeedImageIndicatorContainer>
            {imageLength > 1 &&
              images.map((image, index) => (
                <FeedImageIndicator
                  key={`FeedImageIndicator-${id}-${index}`}
                  style={{
                    backgroundColor:
                      indicatorIndex >= index && indicatorIndex < index + 1
                        ? '#3796EF'
                        : '#D3D3D3',
                  }}
                />
              ))}
          </FeedImageIndicatorContainer>
        </MenuContainer>
        <MenuContainer style={{ justifyContent: 'flex-end' }}>
          <IconButton iconName="bookmark" />
        </MenuContainer>
      </FeedMenuContainer>
    </Container>
  );
};

export default FeedBody;
```

