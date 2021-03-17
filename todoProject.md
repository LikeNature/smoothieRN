# todoProject

![](C:\Users\user\Documents\GitHub\reactNativeApp\uml.PNG)

## 1. Context API

- https://github.com/facebook/flux/tree/master/examples/flux-todomvc

## 2. AsyncStorage

- 앱 내에서 간단하게 데이터를 저장할 수 있는 저장소
- windows.localStorage와 매우 유사
- Key-Value 저장소
- 0.59 버전 커뮤니티 라이브러리인 "react-native-community/react-native-async-storage" 사용
  - AsyncStorage: https://github.com/react-native-community/async-storage

## 3. 프로젝트 준비

```bash
react-native init toDoList
```

```bash
cd toDoList
npm install --save styled-components
npm install --save-dev typescript @types/react @types/react-native @types/styled-components babel-plugin-root-import
```

```json
// tsconfig.json 파일 생성 및 내용 추가
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

- 절대 경로로 컴포넌트 추가
  - babel.config.js

```js
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

- index.js 파일 수정

```js
import App from '~/App';
```

- debug 

```bash
# 설치
npm install -g react-devtools

# 프로젝트 폴더에서 실행
react-devtools
```

 ## 4. 개발

### 1) AsyncStorage 설치 및 설정

```bash
# 0.60 이후 버전에서 설치
npm install --save @react-native-community/async-storage
# 또는
react-native link @react-native-community/async-storage
```

### 2) Context

- @types/index.d.ts 파일을 만들고 해당 파일 안에 타입을 정의하면 프로젝트 전반에 걸쳐 타입 사용가능

- 실제 Context 파일 : \toDoList1\src\TodoListContext\index.tsx

- createContext 로 Context를 생성, useState로 생성한 State 데이터를 Context 안에 저장할 예정

- createContext 함수 초기값으로 데이터 추가위한 addTodoList 함수, 삭제를 위한 remvoeTodoList 할당

- 실제 구현은 Context 의 provider 컴포넌트에서 구현

-  Context 사용을 위해 공통 부모 컴포넌트에서 Context  Provider 사용

  - 공통 부모 컴포넌트에서 프로바이더  사용을 위해 Context 프로바이더 컴포넌트를 만들고 공통 부모 컴포넌트의 부모 컴포넌트로서 사용
  - TodoListContextProvider는 Context의 프로바이더 컴포넌트
    - 자식 컴포넌트를 children 매개변수를 통해 전달받음
  - Context 사용을 위해 컴포넌트 안에서 수정 가능한 데이터를 사용하기 위해 useState 사용

  ```tsx
  // useState로 만든 todoList는 수정불가
  const [todoList, setTodoList] = useState<Array<string>>([]); // todoList 선언, setTodoList 를 통해 추가 삭제
  ```

```tsx
// 새로운 list 변수 생성해, todoList 모든 데이터를 넣음(...todoList), 매개변수로 받은 새로운 데이터(todo) 추가
// 추가된 데이터를 setTodoList를 통해 State 값 변경
// 마지막으로 AsyncStorage 를 통해 데이터 물리적으로 저장
// 키 값은 모두 문자열, JSON.stringfy 함수로 문자열로 변경해 저장
const addTodoList = (todo: String) : void => {
    const list = [...todoList, todo];
    setTodoList(list);
    AsyncStorage.setItem('todoList', JSON.stringify(list));
};
```

```tsx
const removeTodoList = (index: number) : void => {
    let list = [...todoList];
    list.splice(index,1); 	// 매개변수(index)로 삭제하고자 하는 데이터 제거
    setTodoList(list);		// State에 제거된 데이터 저장
    AsyncStorage.setItem('todoList', JSON.stringify(list)); /// 물리적으로 저장된 값 업데이트
};
```

```tsx
// AsyncStorage에 저장된 데이터 불러와 Context 값을 초기화하기 위한 함수
// AsyncStorage 의 setItem과 getItem은 모두 Promise 함수

const initData = async () => {
    try {
        const list = await AsyncStorage.getItem('todoList');
        if (list !== null){
            setTodoList(JSON.parse(list));
        }
    } catch (e) {
        console.log(e);
    }
};
```

### 3) useEffect

```tsx
// 매개변수로 함수 전달
// 함수에서 데이터초기화 함수 호출
	//  두번째 매개변수에 빈배열 전달해 componentDidMount 와 같은 역할 수행
useEffect(() => {
    initData();
    return () =>  // return 함수는 componetWillUnmount 와 같은 역할
}, []);  // 두번째 매개변수([]) 없는 경우, componentDidMoount와 componentDidUpdate 역할 동시수행

```

```tsx
    useEffect(() => {
        initData();
        return () => 
    }, [todoList]);  // 두번째 매개변수 배열에 특정 변수 설정해 전달하면, 변수가 변경될 때만 이 함수가 호출됨
```

```tsx
// 여러번 사용가능
useEffect(() => {
    initData();
    return () => 
}, []);
useEffect(() => {
    initData();
    return () => 
});
```

### 4) 프로바이더 설정

```tsx
// App.tsx
import { TodoListContextProvider } from '~/Context/TodoListContext';

const App = () => {
  return (
  <TodoListContextProvider>
    <Container>
      <Todo />
    </Container>
  </TodoListContextProvider>
  );
};

```

### 5) Todo 컴포넌트

- \src\Screens\Todo\index.tsx

```tsx
import React from 'react';
import Styled from 'styled-components/native';
import TodoListView from './TodoListView';
import AddTodo from './AddTodo';

const Container = Styled.View`
    flex: 1;
`;

interface Props {}

const Todo = ({ } : Props) => {
    return (
        <Container>
            <TodoListView>
            <AddTodo />
            </TodoListView>
        </Container>
    );
};

export default Todo;
```

- 이 책에서는 종속되는 컴포넌트는 종속된 컴포넌트 하위 폴더에 생성함

#### 6) TodoListView 컴포넌트

```tsx
import React from 'react';
import Styled from 'styled-components/native';

import Header from './Header';
import TodoList from './TodoList';

const Container = Styled.SafeAreaView`
    flex: 1;
`;

interface Props {}

const TodoListView = ({ }: Props) => {
    return (
        <Container>
            <Header>
            <TodoList />
            </Header>
        </Container>
    );
};

export default TodoListView;
```

##### 7) Header 컴포넌트

```tsx
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.SafeAreaView`
    height: 40px;
    justify-content: center;
    align-items: center;
`;

const TitleLabel = Styled.Text`
    font-size: 24px;
    font-weight: bold;
`;

interface Props {}

const Header = ({ }: Props) => {
 return (
     <Container>
        <TitleLabel>Todo List</TitleLabel>
     </Container>
 );   
};

export default Header;
```

##### 8)  Context  데이터를 사용하는 TodoList 컴포넌트

```tsx
import React, {useContext} from 'react';
import { FlatList } from 'react-native';
import Styled from 'styled-components/native';

import { TodoListContext } from '~/Context/TodoListContext';  

import EmptyItem from './EmptyItem';
import TodoItem from './TodoItem';

const Container = Styled(FlatList)`
`;

interface Props {}

const TodoList = ({ }: Props ) => {
    const {todoList, removeTodoList} = useContext<ITodoListContext>(  // TodoListContext를 useState 초기값으로 설정, todoList 변수와 removeTodoList 함수 불러옴
        TodoListContext
    );

    return (
        // FlatList 컴포넌트 사용
        <Container
            data={todoList}  // 리스트 뷰에표시할 데이터 배열
            keyExtractor={(item, index) = > {  // keyExtractor: 컴포넌트 키값 설정, FlatList에서 반복적으로 표시하는 Item에 키값 설정하기 위한 Props
                // return `todo-${index}`;
            }}
            ListEmptyComponent = { <EmptyItem />}  // 주어진 배열에 데이터 없을 경우 표시되는 컴포넌트
            renderItem={() => ( 	// 주어진 배열에 데이터 사용해 반복적으로 표시될 컴포넌트
                <TodoItem 
                    text={item as string}
                    onDelete={() => removeTodoList(index)}
                />
            )}
            contentContainerStyle={todoList.length === 0 && {flex: 1}} //표시할 데이터 없는 경우, ListEmptyComponent 표시, 전체화면 표시위해 flex:1 설정
        />
    );
};

export default TodoList;

```

###### 9) TodoList 의 EmptyItem 컴포넌트

```tsx
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.View`
    flex: 1,
    align-items: center;
    justify-content: center;
`;

const Label = Styled.Text``;

interface Props {}

const EmptyItem = ({ }: Props) => {
    return (
        <Container>
            <Label>하단에 "+" 버튼을 눌러 새로운 할일 등록</Label>
        </Container>
    );
};

export default EmptyItem;
```

###### 10) TodoItem 컴포넌트

```tsx
import React from 'react';
import Styled from 'styled-components/native';

const Container = Styled.View`
    flex-direction: row;
    background-color: #FFF;
    margin: 4px 16px;
    padding: 8px 16px;
    border-radius: 8px;
    align-items: center;
`;

const Label = Styled.Text`
    flex: 1;
`;

const DeleteButton = Styled.TouchableOpacity``;
const Icon = Styled.Image`
    width: 24px;
    height: 24px;
`;

interface Props {
    text: string;			// TodoList 컴포넌트로부터 할일 데이터 전달받은 매개변수 설정
    onDelete: () => void;		// 삭제함수 전달받기위한 설정
}

const TodoItem = () => {
    return (
        <Container>
            <Label>{text}</Label>		<!-- 할일 데이터 전달받아 화면에 표시 -->
            <DeleteButton onPress={onDelete}>		<!-- 삭제함수 삽입  -->
                <Icon source={require('~/Assets/Images/remove.png')}/>
            </DeleteButton>
        </Container>
    );
};

export default TodoItem;
```

### 11) AddTodo 컴포넌트

```tsx
import React, {useState} from 'react';

import AddButton from './AddButton';
import TodoInput from '/TodoInput';

interface Props {}

const AddTodo = () => {
    const [showInput, setShowInput] = useState<boolean>(false);
    return (
        <>
            <AddButton onPress={() => setShowInput} />
            {showInput && <TodoInput hideToolInput={()=>setShowInput(false)} />}
        </>
    );
};

export default AddTdoo;
```

