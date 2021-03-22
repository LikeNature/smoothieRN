# 영화소개 앱 - 네비게이션과 앱 리소스

![](.\movieapp.png)

## 1. 네비게이션

- 오픈소스 라이브러리 활용
  - react-navigation(v5) 

## 2. 앱 리소스

## 3. 준비

## 4. 개발

### 1) 앱 아이콘

- react-native-make: https://github.com/bamlab/react-native-make

npm install --save-dev @bam.tech/react-native-make

npm install --save-dev react-native-splash-screen

```bash
react-native set-icon --path ./src/Assets/Images/app_icon.png
```

**node-gyp.js rebuild 에러가 발생하는 경우는 아래의 명령어로 해결이 가능하다.**

```
npm install --global node-gyp 
npm install --global --production windows-build-tools 
```

- 에러발생 => 해결

```json
{
  "name": "movieapp",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "lint": "eslint ."
  },
  // 아래사항 추가후 해결
  "engines": {
    "node": ">=8.0.0"
  },
  "dependencies": {
    "react": "17.0.1",
    "react-native": "0.64.0",
    "sharp": "0.27.2",
    "styled-components": "5.2.1"
  },
  "devDependencies": {
    "@babel/core": "7.13.10",
    "@babel/runtime": "7.13.10",
    "@bam.tech/react-native-make": "3.0.0",
    "@react-native-community/eslint-config": "2.0.0",
    "@types/react": "17.0.3",
    "@types/react-native": "0.63.52",
    "@types/styled-components": "5.1.9",
    "babel-jest": "26.6.3",
    "babel-plugin-root-import": "6.6.0",
    "eslint": "7.14.0",
    "jest": "26.6.3",
    "metro-react-native-babel-preset": "0.64.0",
    "react-native-splash-screen": "3.2.0",
    "react-test-renderer": "17.0.1",
    "typescript": "4.2.3"
  },
  "jest": {
    "preset": "react-native"
  }
}
```

- `npm cache clean -f`
- `npm install --unsafe-per`

### package-lock.json 

1. 팀원끼리 node/npm 버전이 일치하는지 확인: `node -v` and `npm -v` (optional)
2. node modules 삭제: `rm -rf node_modules/`
3. npm 캐시 삭제: `npm cache clean --force`
4. `package-lock.json` 파일 변경 사항 원상복귀
5. 다시 dependencies 설치: `npm i`

### 2) 스플래시 스크린 이미지

- 3,000 X 3,000  사이즈 png 파일 필요

```bash
react-native set-splash --path ./src/Assets/Images/app_splash.png --resize cover
```

### 3) AsyncStorage

- 라이브러리 설치

```bash
npm install --save @react-native-community/async-storage
```

### 4) 내비게이션

```bash
# 기본적으로 내비게이션 사용하기 위한 라이브러리
npm install --save @react-navigation/native
npm install --save react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

```bash
# 본 프로젝트에는 Stack Navigation 만 사용
npm install --save @react-navigation/stack
```

```bash
# 다른 내비게션 사용하고 싶은 경우, 아래와 같이 따로 설치
# Stack Navigation
npm install --save @react-navigation/stack
# Drawer Navigation
npm install --save @react-navigation/drawer
# Bottom Tab Navigation
npm install --save @react-navigation/bottom-tabs
# Material Bottom Navigation
npm install --save @react-navigation/material-bottom-tabs react-native-paper
# Material Top Tab Navigation
npm install --save @react-navigation/material-top-tabs react-native-tab-view

```

### 5) UserContext 컴포넌트

```tsx
// src\Context\User\@types\index.d.ts
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
      .then((value) => {
        if (value) {
          // Get UserInfo via UserInfo API
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

### 6) Loading 컴포넌트

```tsx
// src\Screens\Loading\index.tsx
// 실제 사용은 UserContext 에서 서버와 통신하게됨
import React from 'react';
import {ActivityIndicator} from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.View`
  flex: 1;
  background-color: #141414;
  align-items: center;
  justify-content: center;
`;

// ActivityIndicator 은 화면에 로딩 중임을 표시함
const Loading = () => {
  return (
    <Container>
      <ActivityIndicator color="#E70915" size="large" />  
    </Container>
  );
};

export default Loading;
```

### 7) Login 컴포넌트

```tsx
import React, {useContext, useEffect} from 'react';
import Styled from 'styled-components/native';
import {Linking} from 'react-native';
import SplashScreen from 'react-native-splash-screen';
import {StackNavigationProp} from '@react-navigation/stack';

import {UserContext} from '~/Context/User';

import Input from '~/Components/Input';
import Button from '~/Components/Button';

const Container = Styled.SafeAreaView`
  flex: 1;
  background-color: #141414;
  align-items: center;
  justify-content: center;
`;
const FormContainer = Styled.View`
  width: 100%;
  padding: 40px;
`;

const PasswordReset = Styled.Text`
  width: 100%;
  font-size: 12px;
  color: #FFFFFF;
  text-align: center;
`;

type NavigationProp = StackNavigationProp<LoginNaviParamList, 'Login'>;
interface Props {
  navigation: NavigationProp;
}

const Login = ({navigation}: Props) => {
  const {login} = useContext<IUserContext>(UserContext);

  useEffect(() => {
    SplashScreen.hide();
  }, []);

  return (
    <Container>
      <FormContainer>
        <Input style={{marginBottom: 16}} placeholder="이메일" />
        <Input
          style={{marginBottom: 16}}
          placeholder="비밀번호"
          secureTextEntry={true}
        />
        <Button
          style={{marginBottom: 24}}
          label="로그인"
          onPress={() => {
            login('dev.yakuza@gmail.com', 'password');
          }}
        />
        <PasswordReset
          onPress={() => {
            Linking.openURL('https://dev-yakuza.github.io/ko/');
          }}>
          비밀번호 재설정
        </PasswordReset>
      </FormContainer>
    </Container>
  );
};

export default Login;
```

```tsx
// src\Screens\@types\index.d.ts
type LoginNaviParamList = {
  Login: undefined;
};

type MovieNaviParamList = {
  MovieHome: undefined;
  MovieDetail: {
    id: number;
  };
};
```

### 8) Input 컴포넌트

```tsx
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.View`
  width: 100%;
  height: 40px;
  padding-left: 16px;
  padding-right: 16px;
  border-radius: 4px;
  background-color: #333333;
`;
const InputField = Styled.TextInput`
  flex: 1;
  color: #FFFFFF;
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
    // sectionColor: 복사나 붙여넣기 위한 색성셜정
    // secureTextEntry: 비밀번호 입력시 내용 숨길지 결정
    // keyboardType: 이메일, 숫자, 전화번호 등과 같이 키보드 타입 결정
    // autoCapitalize: 영어 입력시 첫글자 대문자로 자동으로 변경할지 결정
    // autoCorrect: 사용자의 입력 내용 철자가 틀렸을 경우, 자동으로 수정할지 여부설정
    // allowFontScaling: 사용자가 단말기 설정을 통해 수정한 폰트 크기를 적용할지 여부 설정
    // placeholder: 사용자 입력 내용이 없을 때, 표시할 내용 설정
    // clearButtonMode: 사용자 입력내용 있을 때, 입력창 오른쪽 부분 전체 삭제버튼 표시여부
    // onChangeText: 입력 창의 내용 변경시 호출되는 콜백함수
  return (
    <Container style={style}>
      <InputField
        selectionColor="#FFFFFF"
        secureTextEntry={secureTextEntry}
        keyboardType={keyboardType ? keyboardType : 'default'}
        autoCapitalize="none"
        autoCorrect={false}
        allowFontScaling={false}
        placeholderTextColor="#FFFFFF"
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
import React from 'react';
import Styled from 'styled-components/native';

const StyleButton = Styled.TouchableOpacity`
  width: 100%;
  height: 40px;
  border-radius: 4px;
  justify-content: center;
  align-items: center;
  border: 1px;
  border-color: #333333;
`;
const Label = Styled.Text`
  color: #FFFFFF;
`;

interface Props {
  label: string;
  style?: Object;
  onPress?: () => void;
}

const Button = ({label, style, onPress}: Props) => {
  return (
    <StyleButton style={style} onPress={onPress}>
      <Label>{label}</Label>
    </StyleButton>
  );
};

export default Button;
```

### 10) 영화 데이터 타입

- YTS: https://yts.lt/
- 타입스크립트 참조
  - https://kjwsx23.tistory.com/522

```tsx
// src\Screens\MovieHome\@types\index.d.ts
interface IMovie {
  id: number;
  title: string;
  title_english: string;
  title_long: string;
  summary: string;
  synopsis: string;
  background_image: string;
  background_image_original: string;
  date_uploaded: string;
  date_uploaded_unix: number;
  description_full: string;
  genres: Array<string>;
  imdb_code: string;
  language: string;
  large_cover_image: string;
  medium_cover_image: string;
  mpa_rating: string;
  rating: number;
  runtime: number;
  slug: string;
  small_cover_image: string;
  state: string;
  url: string;
  year: number;
  yt_trailer_code: string;
}
```

### 11) MovieHome 컴포넌트

```tsx
// src\Screens\MovieHome\index.tsx

import React, {useContext, useLayoutEffect, useEffect} from 'react';
import SplashScreen from 'react-native-splash-screen';
import {StackNavigationProp} from '@react-navigation/stack';
import Styled from 'styled-components/native';

import {UserContext} from '~/Context/User';

import BitCatalogList from './BigCatalogList';
import SubCatalogList from './SubCatalogList';

const Container = Styled.ScrollView`
  flex: 1;
  background-color: #141414;
`;

const StyleButton = Styled.TouchableOpacity`
  padding: 8px;
`;
const Icon = Styled.Image`
`;

// 화면이동하기 위한 navigation 함수 불러옴
type NavigationProp = StackNavigationProp<MovieNaviParamList, 'MovieHome'>;
interface Props {
  navigation: NavigationProp;
}


const MovieHome = ({navigation}: Props) => {
  const {logout} = useContext<IUserContext>(UserContext); // UserContext의 logout 함수 사용 src\Context\User\index.tsx
  // 로그아웃 버튼을 useLayoutEffect 리액트 훅 활용, 동기적 작동시 사용, 화면에 완전히 표시되기전 내용 실행되고 완료된 후 컴포넌트가 화면에 표시됨
  // navigation.setOptions 내비게이션 헤더의 로그아웃 버튼 표시, 추후에 표시, 동적으로 options 수정시 활용
  useLayoutEffect(() => {
    navigation.setOptions({
      headerRight: () => (
        <StyleButton
          onPress={() => {
            logout();
          }}>
          <Icon source={require('~/Assets/Images/ic_logout.png')} />
        </StyleButton>
      ),
    });
  }, []);

  useEffect(() => {
    SplashScreen.hide();
  }, []);

    // API를 통해 상세정보 가져오는데 영화 ID가 필요하며, navigation.navigate() 함수를 통해 전달
  return (
    <Container>
      <BitCatalogList
        url="https://yts.lt/api/v2/list_movies.json?sort_by=like_count&order_by=desc&limit=5"
        onPress={(id: number) => {
          navigation.navigate('MovieDetail', {
            id,
          });
        }}
      />
      <SubCatalogList
        title="최신 등록순"
        url="https://yts.lt/api/v2/list_movies.json?sort_by=date_added&order_by=desc&limit=10"
        onPress={(id: number) => {
          navigation.navigate('MovieDetail', {
            id,
          });
        }}
      />
      <SubCatalogList
        title="평점순"
        url="https://yts.lt/api/v2/list_movies.json?sort_by=rating&order_by=desc&limit=10"
        onPress={(id: number) => {
          navigation.navigate('MovieDetail', {
            id,
          });
        }}
      />
      <SubCatalogList
        title="다운로드순"
        url="https://yts.lt/api/v2/list_movies.json?sort_by=download_count&order_by=desc&limit=10"
        onPress={(id: number) => {
          navigation.navigate('MovieDetail', {
            id,
          });
        }}
      />
    </Container>
  );
};

export default MovieHome;
```

```tsx
// src\Screens\@types\index.d.ts
// MovieNaviParamList 로 Movienavigator 선언
type LoginNaviParamList = {
  Login: undefined;
};

type MovieNaviParamList = {
  MovieHome: undefined;
  MovieDetail: {
    id: number;
  };
};
```



### 12) BigCatalogList 컴포넌트

```tsx
// src\Screens\MovieHome\BigCatalogList\index.tsx

import React, {useState, useEffect} from 'react';
import {FlatList} from 'react-native';
import Styled from 'styled-components/native';

import BigCatalog from '~/Components/BigCatalog';

const Container = Styled.View`
    height: 300px;
    margin-bottom: 8px;
`;

interface Props {
  url: string;
  onPress: (id: number) => void;
}

const BigCatalogList = ({url, onPress}: Props) => {
  const [data, setData] = useState<Array<IMovie>>([]);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((json) => {
        console.log(json);
        setData(json.data.movies);
      })
      .catch((error) => {
        console.log(error);
      });
  }, []);

  return (
    <Container>
      <FlatList
        horizontal={true}
        pagingEnabled={true}
        data={data}
        keyExtractor={(item, index) => {
          return `bigScreen-${index}`;
        }}
        renderItem={({item, index}) => (
          <BigCatalog
            id={(item as IMovie).id}
            image={(item as IMovie).large_cover_image}
            year={(item as IMovie).year}
            title={(item as IMovie).title}
            genres={(item as IMovie).genres}
            onPress={onPress}
          />
        )}
      />
    </Container>
  );
};

export default BigCatalogList;
```

### 13) BigCatalog 컴포넌트

```tsx
// src\Components\BigCatalog\indes.tsx

import React from 'react';
import {Dimensions} from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.TouchableOpacity``;
const CatalogImage = Styled.Image``;
const InfoContainer = Styled.View`
  position: absolute;
  bottom: 0;
  width: 100%;
  align-items: flex-start;
`;
const LabelYear = Styled.Text`
  background-color: #E70915;
  color: #FFFFFF;
  padding: 4px 8px;
  margin-left: 4px;
  margin-bottom: 4px;
  font-weight: bold;
  border-radius: 4px;
`;
const SubInfoContainer = Styled.View`
`;
const Background = Styled.View`
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: #141414;
  opacity: 0.9;
`;
const LabelTitle = Styled.Text`
  font-size: 18px;
  font-weight: bold;
  color: #FFFFFF;
  padding: 8px 16px 4px 16px;
`;
const LabelGenres = Styled.Text`
  font-size: 12px;
  color: #FFFFFF;
  padding: 4px 16px 8px 16px;
`;

interface Props {
  id: number;
  image: string;
  year: number;
  title: string;
  genres: Array<string>;
  onPress?: (id: number) => void;
}

const BigCatalog = ({id, image, year, title, genres, onPress}: Props) => {
  return (
    <Container
      activeOpacity={1}
      onPress={() => {
        if (onPress && typeof onPress === 'function') {
          onPress(id);
        }
      }}>
      <CatalogImage
        source={{uri: image}}
        style={{width: Dimensions.get('window').width, height: 300}}
      />
      <InfoContainer>
        <LabelYear>{year}년 개봉</LabelYear>
        <SubInfoContainer>
          <Background />
          <LabelTitle>{title}</LabelTitle>
          <LabelGenres>{genres.join(', ')}</LabelGenres>
        </SubInfoContainer>
      </InfoContainer>
    </Container>
  );
};

export default BigCatalog;
```

### 14) SubCatalogList 컴포넌트

```tsx
// src\Screens\MovieHome\SubCatalogList\indes.tsx

import React, {useState, useEffect} from 'react';
import {FlatList} from 'react-native';

import Styled from 'styled-components/native';

const Container = Styled.View`
  margin: 8px 0px;
`;
const InfoContainer = Styled.View`
  flex-direction: row;
  justify-content: space-between;
  padding: 8px 16px;
`;
const Title = Styled.Text`
  font-size: 16px;
  color: #FFFFFF;
  font-weight: bold;
`;
const CatalogContainer = Styled.View`
  height: 201px;
`;
const CatalogImageContainer = Styled.TouchableOpacity`
  padding: 0px 4px;
`;
const CatalogImage = Styled.Image`
`;

interface Props {
  title: string;
  url: string;
  onPress: (id: number) => void;
}

const SubCatalogList = ({title, url, onPress}: Props) => {
  const [data, setData] = useState<Array<IMovie>>([]);

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((json) => {
        console.log(json);
        setData(json.data.movies);
      })
      .catch((error) => {
        console.log(error);
      });
  }, []);

  return (
    <Container>
      <InfoContainer>
        <Title>{title}</Title>
      </InfoContainer>
      <CatalogContainer>
        <FlatList
          horizontal={true}
          data={data}
          keyExtractor={(item, index) => {
            return `catalogList-${(item as IMovie).id}-${index}`;
          }}
          renderItem={({item, index}) => (
            <CatalogImageContainer
              activeOpacity={1}
              onPress={() => {
                onPress((item as IMovie).id);
              }}>
              <CatalogImage
                source={{uri: (item as IMovie).large_cover_image}}
                style={{width: 136, height: 201}}
              />
            </CatalogImageContainer>
          )}
        />
      </CatalogContainer>
    </Container>
  );
};

export default SubCatalogList;
```

### 15) 영화 상세 정보 데이터타입

```tsx
// src\Screens\MovieDetail\@types\index.d.ts

interface ICast {
  name: string;
  character_name: string;
  url_small_image: string;
}
interface IMovieDetail {
  id: number;
  title: string;
  title_english: string;
  title_long: string;
  cast: Array<ICast>;
  description_full: string;
  description_intro: string;
  genres: Array<string>;
  large_cover_image: string;
  large_screenshot_image1: string;
  large_screenshot_image2: string;
  large_screenshot_image3: string;
  like_count: number;
  rating: number;
  runtime: number;
  year: number;
}
```

### 16) MovieDetail 컴포넌트

```tsx
import React, {useState, useEffect} from 'react';
import Styled from 'styled-components/native';
import {RouteProp} from '@react-navigation/native';

import Loading from '~/Screens/Loading';
import BigCatalog from '~/Components/BigCatalog';
import CastList from './CastList';
import ScreenShotList from './ScreenShotList';

const Container = Styled.ScrollView`
  flex: 1;
  background-color: #141414;
`;
const ContainerTitle = Styled.Text`
  font-size: 16px;
  color: #FFFFFF;
  font-weight: bold;
  padding: 24px 16px 8px 16px;
`;
const DescriptionContainer = Styled.View``;
const Description = Styled.Text`
  padding: 0 16px;
  color: #FFFFFF;
`;
const SubInfoContainer = Styled.View``;
const InfoContainer = Styled.View`
  flex-direction: row;
  justify-content: space-between;
  padding: 0 16px;
`;
const LabelInfo = Styled.Text`
  color: #FFFFFF;
`;

type ProfileScreenRouteProp = RouteProp<MovieNaviParamList, 'MovieDetail'>;
interface Props {
  route: ProfileScreenRouteProp;
}

const MovieDetail = ({route}: Props) => {
  const [data, setData] = useState<IMovieDetail>();

  useEffect(() => {
    const {id} = route.params;
    fetch(
      `https://yts.lt/api/v2/movie_details.json?movie_id=${id}&with_images=true&with_cast=true`,
    )
      .then((response) => response.json())
      .then((json) => {
        console.log(json);
        setData(json.data.movie);
      })
      .catch((error) => {
        console.log(error);
      });
  }, []);

  return data ? (
    <Container>
      <BigCatalog
        id={data.id}
        image={data.large_cover_image}
        year={data.year}
        title={data.title}
        genres={data.genres}
      />
      <SubInfoContainer>
        <ContainerTitle>영화 정보</ContainerTitle>
        <InfoContainer>
          <LabelInfo>{data.runtime}분</LabelInfo>
          <LabelInfo>평점: {data.rating}</LabelInfo>
          <LabelInfo>좋아요: {data.like_count}</LabelInfo>
        </InfoContainer>
      </SubInfoContainer>
      <DescriptionContainer>
        <ContainerTitle>줄거리</ContainerTitle>
        <Description>{data.description_full}</Description>
      </DescriptionContainer>
      {data.cast && <CastList cast={data.cast} />}
      <ScreenShotList
        images={[
          data.large_screenshot_image1,
          data.large_screenshot_image2,
          data.large_screenshot_image3,
        ]}
      />
    </Container>
  ) : (
    <Loading />
  );
};

export default MovieDetail;
```

### 17) CastList 컴포넌트 ScreenShotList 컴포넌트

```tsx
// src\Screens\MovieDetail\CastList\index.tsx

import React from 'react';
import {FlatList} from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.View``;
const Title = Styled.Text`
  font-size: 16px;
  color: #FFFFFF;
  font-weight: bold;
  padding: 24px 16px 8px 16px;
`;
const CastContainer = Styled.View`
    padding: 0px 4px;
`;
const CastImage = Styled.Image``;
const LabelName = Styled.Text`
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 4px 2px;
  color: #FFFFFF;
  font-size: 14px;
  font-weight: bold;
  text-align: center;
`;
interface Props {
  cast: Array<ICast>;
}

const CastList = ({cast}: Props) => {
  return (
    <Container>
      <Title>배우</Title>
      <FlatList
        horizontal={true}
        data={cast}
        keyExtractor={(item, index) => {
          return `castList-${index}`;
        }}
        renderItem={({item, index}) => (
          <CastContainer>
            <CastImage
              source={{uri: (item as ICast).url_small_image}}
              style={{width: 100, height: 150}}
            />
            <LabelName>{(item as ICast).name}</LabelName>
          </CastContainer>
        )}
      />
    </Container>
  );
};

export default CastList;
```

```tsx
// src\Screens\MovieDetail\ScreenShotList\index.tsx
import React from 'react';
import {FlatList, Dimensions} from 'react-native';
import Styled from 'styled-components/native';

const Container = Styled.View`
  margin-bottom: 24px;
`;
const Title = Styled.Text`
  font-size: 16px;
  color: #FFFFFF;
  font-weight: bold;
  padding: 24px 16px 8px 16px;
`;
const ScreenShotImage = Styled.Image``;

interface Props {
  images: Array<string>;
}

const ScreenShotList = ({images}: Props) => {
  return (
    <Container>
      <Title>스크린샷</Title>
      <FlatList
        horizontal={true}
        pagingEnabled={true}
        data={images}
        keyExtractor={(item, index) => {
          return `screenShotList-${index}`;
        }}
        renderItem={({item, index}) => (
          <ScreenShotImage
            source={{uri: item}}
            style={{width: Dimensions.get('window').width, height: 200}}
          />
        )}
      />
    </Container>
  );
};

export default ScreenShotList;
```

