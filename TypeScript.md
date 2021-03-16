# TypeScript: 정적 타입 검사자 (TypeScript: A Static Type Checker)



## 구문 (Syntax)

TypeScript는 JS의 구문이 허용되는, JavaScript의 *상위 집합* 언어입니다. 구문은 프로그램을 만들기 위해 코드를 작성하는 방법을 의미합니다. 

- 런타임(runtime) :런타임은 프로그램이 실행되고 있는 때 존재하는 곳을 말한다. 즉, 컴퓨터 내에서 프로그램이 기동되면, 그것이 바로 그 프로그램의 런타임이다. 단순히 말해서 런타임이란 프로그래밍 언어가 구동되는 환경이라고 이해를 하면 된다. JavaScript라면 Web Browser에서 작동하는 JavaScript 측면이 있고Node.js라는 환경에서 구동되는 측면이 존재한다.

## 런타임 특성 (Runtime Behavior)

TypeScript는 JavaScript의 *런타임 특성*을 가진 프로그래밍 언어입니다. 예를 들어, JavaScript에서 0으로 나누는 행동은 런타임 예외로 처리하지 않고 `Infinity`값을 반환합니다. 논리적으로, TypeScript는 JavaScript 코드의 런타임 특성을 **절대** 변화시키지 않습니다.

즉 TypeScript가 코드에 타입 오류가 있음을 검출해도, JavaScript 코드를 TypeScript로 이동시키는 것은 같은 방식으로 실행시킬 것을 **보장합니다**

## 삭제된 타입 (Erased Types)

개략적으로, TypeScript의 컴파일러가 코드 검사를 마치면 타입을 *삭제해서* 결과적으로 "컴파일된" 코드를 만듭니다. 즉 코드가 한 번 컴파일되면, 결과로 나온 일반 JS 코드에는 타입 정보가 없습니다.

타입 정보가 없는 것은 TypeScript가 추론한 타입에 따라 프로그램의 *특성*을 변화시키지 않는다는 의미입니다. 결론적으로 컴파일 도중에는 타입 오류가 표출될 수 있지만, 타입 시스템 자체는 프로그램이 실행될 때 작동하는 방식과 관련이 없습니다.

마지막으로, TypeScript는 추가 런타임 라이브러리를 제공하지 않습니다. TypeScript는 프로그램은 JavaScript 프로그램과 같은 표준 라이브러리 (또는 외부 라이브러리)를 사용하므로, TypeScript 관련 프레임워크를 추가로 공부할 필요가 없습니다.

## TypeScript 설치하기 (Installing TypeScript)

TypeScript를 설치하는 방법에는 크게 두 가지가 있습니다:

- npm을 이용한 설치 (Node.js 패키지 매니저)
- TypeScript의 Visual Studio 플러그인 설치

Visual Studio 2017과 Visual Studio 2015 Update 3는 기본적으로 Typescript를 포함하고 있습니다. 만약 TypeScript를 Visual Studio과 함께 설치하지 않았다면 [이곳에서 다운로드](https://www.typescriptlang.org/#download-links)할 수 있습니다.





