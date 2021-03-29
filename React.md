## ESLint 와 Prettier 적용하기

### 1. ESLint

- ESLint는 문법 검사도구, Prettier는 코드 스타일 자동 정리도구
- VS Code 상단메뉴에서 보기 > 문제 클릭

### 2. Prettier

- F1 누르고, format 입력



## 3장 컴포넌트

### 1. 클래스형 컴포넌트

- ES6 이전에는 자바스크립트에 class 가 없었음.
- 클래스형 컴포넌트에서는 render 함수가 꼭 있어야 하고, 그 안에서 보여주어야 할 JSX 를 반환

#### 함수형 컴포넌트 장점

- 클래스형 컴포넌트보다 선언하기가 편함
- 메모리 자원도 클래스형 컴포넌트보다 덜 사용
- 프로젝트 완성하여 빌드한 후 배포할 때도 결과물의 파일 크기가 더 작음(별차이 없음)

#### 함수형 컴포넌트 단점

- state와 라이프사이클 API 사용이 불가능하다는 단점이 있지만, 리액트 v16.8 업데이트 이후 Hooks 기능이 도입되면서 해결

### 2. 첫 컴포넌트 생성

#### ES6의 화살표 함수

```tsx
// 기존 function 이용한 함수 선언 방식과 다름
/*
일반 function() 함수는 자신이 종속된 객체를 this 로 가리키며, 화살표 함수는 자신이 종속된 인스턴스를 가리킴
따로 { } 가 없으면 연산한 값을 그대로 반환한다는 의미
*/
```

### 3. Props

#### (1) JSX 내부에서 props 렌더링

- props 값은 컴포넌트 함수의 파라미터로 받아와 사용가능
- props 렌더링은 JSX 내부에 { } 기호 감싸기

```tsx
import React form 'react';
const MyComponent = props => {
	return <div> 제이름은 {props.name} </div>;
};
export default MyComponent;
```

#### (2)  컴포넌트 사용시 props 값 지정

```tsx
import React form 'react';
import MyComponent form './MyComponent';
const App = () => {
  return<MyComponent name="React" />;  
};
export default App;
```

#### (3) props 기본값 설정: defaultProps

```tsx
import React form 'react';
const MyComponent = props => {
	return <div> 제이름은 {props.name} </div>;
};
MyComponent.defaultProps = {
	name:'기본 이름
};
export default MyComponent;
```

#### (4) 태그 사이 내용을 보여주는 children

```tsx
// App.js
import React form 'react';
import MyComponent form './MyComponent';
const App = () => {
  return<MyComponent>children 리엑트</MyComponent>;
};
export default App;
```

```tsx
// MyComponent.js
import React form 'react';
const MyComponent = props => {
  return(
      <div>
          제이름은 {props.name}입니다.<br/>
          children 값은 {props.children}
      </div>
  );
};
MyComponent.defaultProps = {
	name: '기본 이름'
};
export default MyComponent;
```

#### (5) 비구조화 할당 문법을 통해 props 내부 값 추출하기

```tsx
// MyComponent.js
import React form 'react';
const MyComponent = ({name, children}) => {
  return(
      <div>
          제이름은 {name}입니다.<br/>
          children 값은 {children}
      </div>
  );
};
MyComponent.defaultProps = {
	name: '기본 이름'
};
export default MyComponent;
```

#### (6) propTypes 를 통한 props 검증

```tsx
// MyComponent.js
// name 값은 무조건 문자열 형태로 전달해야 된다는 것을 의미
import React from 'react';
import PropsTypes from 'prop-types';
const MyComponent = ({name, children}) => {
  return(...);
};
MyComponent.defaultProps = {
	name: '기본 이름'
};
MyComponent.propTypes = {
	name: PropTypes.string
};
export default MyComponent;
```

##### 1) isRequired를 사용해 필수 propTypes 설정

```tsx
import React from 'react';
import PropsTypes from 'prop-types';

const MyComponent = ({name, favoriteNumber, children}) => {
	return (
  		<div>
            제 이름은 {name} <br/>
            chidren은 {children} <br/>
            좋아하는 숫자 {favoriteNumber}
         </div>
     );
};

MyComponent.defualtProps = {
	name: '기본 이름'
};

MyComponent.propTypes = {
	name: PropTypes.string,
    favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

##### 2) PropTypes 종류

| PropTypes                                              | 의미                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| array                                                  |                                                              |
| arrayOf(다른 PropsType)                                | 특정 PropsTypes로 이루어진 배열. 예를들어 arrayOf(PropsTypes.number)는 숫자로 이루어진 배열 |
| bool                                                   | true 혹은 false                                              |
| func                                                   | 함수                                                         |
| number                                                 | 숫자                                                         |
| object                                                 | 객체                                                         |
| string                                                 | 문자열                                                       |
| symbol                                                 | ES6의 Symbol                                                 |
| node                                                   | 렌더링할 수 있는 모든 것                                     |
| instanceOf(클래스)                                     | 특정 클래스의 인스턴스                                       |
| oneOf(['dog','cat'])                                   | 주어진 배열 요소 중 값 하나                                  |
| oneOfType([React.PropTypes.string, PropTypes.number])  | 주어진 배열 안의 종류 중 하나                                |
| objectOf(React.PropTypes.number)                       | 객체의 모든 키 값이 인자로 주어진 PropType인 객체            |
| shape({name: PropTypes.string, num: PropTypes.number}) | 주어진 스키마를 가진 객체                                    |
| any                                                    | 아무 종류                                                    |

#### (7) 클래스형 컴포넌트에서 props 사용하기

- 함수형 컴포넌트의 defaultProps 와 render 함수에서 this.props 동일

```tsx
import React, {Component} from 'react';
import PropsTypes from 'prop-types';

class MyComponent extends Component {
	render(){
    	const {name, favoriteNumber, children } = this.props; // 비구조화 할당
        return (
        	<div>
                제 이름은 {name} </br>
            	children 값은 {children} <br/>
    			좋아하는 숫자 {favoriteNumber}
             </div>
        );
    }
}

MyComponent.defualtProps = {
	name: '기본 이름'
};

MyComponent.propTypes = {
	name: PropTypes.string,
    favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

```tsx
// class 내부에서 지정
import React, {Component} from 'react';
import PropsTypes from 'prop-types';

class MyComponent extends Component {
    static defaultPropst = {
    	name: '기본 이름'
    };
    static propTypes = {
    	name: PropTypes.string,
        favoriteNumber: PropTypes.number.isRequired
    };
	render(){
    	const {name, favoriteNumber, children } = this.props; // 비구조화 할당
        return (
        	<div>
                제 이름은 {name} </br>
            	children 값은 {children} <br/>
    			좋아하는 숫자 {favoriteNumber}
             </div>
        );
    }
}

export default MyComponent;
```

### 4. State

- state 는 컴포넌트 내부에서 바뀔 수 있는 값을 의미
- props를 바꾸려면 부모 컴포넌트에서 바꾸어줘야 함
- 두 가지 종류
  - 클래스형 컴포넌트 : state
  - 함수형 컴포넌트: useState 함수를 통한 state

#### (1) 클래스형 컴포넌트의 State

```tsx
export default class Counter extends Component {
  constructor(props){
    super(props);
    this.state = {number: 0};
  };


  render(){
    const {number} = this.state;
    return (
      <Text>
        {number} {'\n'}
        <Button onClick={()=>this.setState({number: number+1})}>
        button
        </Button>
      </Text>
    );
  }
}
```

##### 1) state 객체 안에 여러 값이 있을 때

