## 실행 컨텍스트 Execution Context
> 실행할 코드에 제공할 환경 정보들을 모아놓은 객체. (자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념.)
- 동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 콜 스택(Call stack)에 쌓아올렸다가, 가장 위에 쌓여있는 컨텍스트와 관련 있는 코드를 실행하는 식으로 전체 코드의 환경과 순서를 보장함.
  - 동일한 환경, 하나의 실행 컨텍스트를 구성할 수 있는 방법, 전역공간, `eval()` 함수, 함수 등이 있음.
  - 자동으로 생성되는 전역공간과 `eval()`을 제외하면 흔히 실행 컨텍스트를 구성하는 뱡법은 함수를 실행하는 것뿐이다.
JavaScript는 어떤 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 위로 끌어올리고(호이스팅), 외부 환경 정보를 구성하고, `this` 값을 설정하는 등의 동작을 수행.

### 스택 Stack
> 출입구가 하나뿐인 깊은 우물 같은 데이터 구조.
- 선입후출 구조 (First In Last Out)
- 예로 a, b, c, d 순으로 저장시 꺼낼 때는 반대로 d, c, b, a 순이다.
- 스택오버플로우 사이트에서의 그 스택임.

### 큐 Queue
> 양쪽이 모두 열려있는 파이프, 보통 한쪽은 입력만, 다른 한쪽은 출력만을 담당.
- 선입선출 구조 (First In First Out)

**실행 컨텍스트와 콜 스택**
```js
// ------------------------------ (1)
var a = 1;
function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }
  inner(); // ------------------- (2)
  console.log(a); // 1
}
outer(); // --------------------- (3)
console.log(a); // 1
```

1. 자바스크립트 코드를 실행하는 순간 **(1)** 전역 컨텍스트가 콜 스택에 담김.
  - 전역 컨텍스트라는 개념은 일반적인 실행 컨텍스트와 다를 것이 없음.
  - 최상단 공간은 코드 내부에서 별도의 실행 명령 없이 브라우저에서 자동으로 실행
  - JS 파일이 열리는 순간 전역 컨텍스트가 활성화된다고 이해
2. 전역 컨텍스트와 관련된 코드들을 순차 진행
3. **(3)** 에서 `outer`함수를 호출하면 JS 엔진은 `outer`에 대한 환경 정보를 수집해서 `outer` 실행 컨텍스트를 생성, 콜 스택에 담음
4. 콜스택 맨 위 `outer`실행 컨텍스트가 놓임. `outer` 함수 내부 코드 순차 진행, 전역 컨텍스트 관련 코드 실행 중단
5. **(2)** 에서 `inner`함수의 실행 컨텍스트가 콜 스택의 가장 위에 담기면 `outer` 컨텍스트와 관련된 코드 실행 중단, `inner` 함수 내부 코드를 순서대로 진행.
6. `inner`함수에서 a 변수에 3을 할당하고 실행 종료, 콜스택에서 `inner`실행 컨텍스트 제거
7. 중단했던 (2) 다음부터 이어서 실행, a 변수 값 출력 후 `outer`함수 실행 종료, 콜스택에서 제거
8. (3) 다음 줄부터 이어서 실행, a 변수 값 출력 후 전역 컨텍스트도 제거, 콜 스택은 빈 상태로 종료

실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장함.

이 객체는 JS 엔진이 활용할 목적으로 생성할 뿐 개발자가 코드를 통해 확인할 수는 없음. 여기에 담기는 정보는 아래와 같음.
- `VariableEnvironment` : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보, 선언 시점의 `LexicalEnvironment`의 스냅샷 snapshot으로 변경 사항은 반영되지 않음.
- `LexicalEnvironment` : 처음에는 `VariableEnvironment`와 같지만 변경 사항이 실시간으로 반영됨.
- `ThisBinding` : 식별자가 바라봐야 할 대상 객체

### VariableEnvironment
- `VariableEnvironment`에 담기는 내용은 `LexicalEnvironment`와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다름.
- 실행 컨텍스트를 생성할 때 `VariableEnvironment`에 정보를 먼저 담고 그대로 복사해서 `LexicalEnvironment`를 만들고 이후엔 이를 주로 사용함.
- `LexicalEnvironment`와 `VariableEnvironment`의 내부는 `environmentRecord`와 `outer-EnvironmentReference`로 구성되어 있음.
- 초기화 과정엔 완전히 동일, 이후 코드 진행에 따라 달라질 것임.

### LexicalEnvironment
- 컨텍스트를 구성하는 환경 정보들을 사전에서 접하는 느낌으로 모아놓은 것.
  - 예: 현재 컨텍스트 내부에는 a,b,c와 같은 식별자들이 있고 그 외부 정보는 D를 참조하도록 구성되어있다.

#### `environmentRecord`와 호이스팅
- `environmentRecord`에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장됨
- 컨텍스트를 구성하는 함수에 지정된 매개변수 식별자, 선언한 함수가 있을 경우 그 함수 자체, var로 선언된 변수의 식별자 등이 식별자에 해당.
- 컨텍스트 내부 전체를 처음부터 끝까지 쭉 훑어나가며 **순서대로** 수집함.
- **참고** : 전역 실행 컨텍스트는 전역 객체(global object, JS 구동 환경이 별도로 제공하는 객체)를 활용함. 
- JS 엔진은 식별자들을 최상단으로 끌어올린 다음 코드를 실행함. (Hoisting 호이스팅)
  - 변수 정보를 수집하는 과정을 더욱 이해하기 쉬운 방법으로 대체한 가상의 개념.
- `environmentRecord`는 현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지에만 관심이 있고, 각 식별자에 어떤 값이 할당될 것인지는 관심이 없음.
  - 변수명만 끌어올리고(선언), 할당 과정은 원래 자리에 둠.
  - 변수는 선언부와 할당부를 나누어 선언부만 끌어올리는 반면, 함수 선언은 함수 전체를 끌어올림.

#### 함수 선언문(function declaration)과 함수 표현식(function expression)
둘 모두 함수를 새롭게 정의할 때 쓰이는 방식. 

- 함수 선언문은 function 정의부만 존재하고 별도의 할당 명령이 없는 것.
  - 반드시 함수 명이 정의되어 있어야함.
- 함수 표현식은 정의한 function을 별도의 변수에 할당하는 것을 말함.
  - 함수 명을 정의한 함수 표현식 : 기명 함수 표현식
  - 정의하지 않은 함수 표현식 : 익명 함수 표현식 (일반적 함수 표현식)

**함수를 정의하는 세 가지 방식**
```js
function a() { /* ... */ }          // 함수 선언문. 함수명 a가 곧 변수명.
a(); // 실행 OK.

var b = function () { /* ... */ }   // (익명) 함수 표현식. 변수명 b가 곧 함수명.
b(); // 실행 OK.

var c = function d () { /* ... */ } // 기명 함수 표현식. 변수명은 c, 함수명은 d.
c(); // 실행 OK.
d(); // 에러!
// c 함수 내부에서는 c()로 호출하든 d()로 호출하든 잘 됨. 따라서 함수 내부 재귀함수를 호출하는 용도로 쓸 수 있었음. 
```

**참고**
- 기명 함수 표현식의 경우 외부에서는 함수명으로 함수를 호출할 수 없음. 오직 함수 내부에서만 접근할 수 있음.
- 용도? 과거 익명 함수 표현식의 경우 `undefined`, `unnamed`라는 값이 나왔었음. 
  - 기명 함수 표현식이 디버깅시 어떤 함수인지를 추적하기에 익명 함수 표현식보다 유리.
  - 이제는 모든 브라우저들이 익명 함수 표현식의 변수명을 함수의 name 프로퍼티에 할당하고 있음

**함수 선언문과 함수 표현식 (1) - 원본코드**
```js
console.log(sum(1, 2));
console.log(multiply(3, 4));

function sum (a, b) {             // 함수 선언문 sum
  return a + b;
}

var multiply = function (a, b) {  // 함수 표현식 multiply
  return a * b;
}
```


**함수 선언문과 함수 표현식 (2) - 호이스팅을 마친 상태**
```js
var sum = function sum (a, b) {   // 함수 선언문은 전체를 호이스팅한다.
    return a + b;
};
var multiply;                     // 변수는 선언부만 끌어올린다.
console.log(sum(1, 2));
console.log(multiply(3, 4));

multiply = function (a, b) {  // 변수의 할당부는 원래의 자리에 남겨둔다.
  return a * b;
};
```
- 함수 선언문은 전체를 호이스팅한 반면, 함수 표현식은 변수 선언부만 호이스팅했음.
  - 함수도 하나의 값으로 취급할 수 있다는 것이 바로 이런 것.
  - 함수를 다른 변수에 값으로써 '할당'한 것이 곧 함수 표현식.
- 전역 컨텍스트가 활성화 될 때, 전역공간에 선언된 함수들이 모두 가장 위로 끌어올려짐.
- 동일한 변수명에 서로 다른 값을 할당할 경우 나중에 할당한 값이 먼저 할당한 값을 덮어씌움(override).
  - 코드를 실행하는 중에 실제로 호출되는 함수는 오직 마지막까지 할당한 함수, 맨 마지막에 선언된 함수 뿐임.
- 함수 표현식으로 정의하는 것이 상대적으로 안전하다.
- 협업을 위해선 전역공간에 함수를 선언하거나 동명의 함수를 중복 선언하는 경우는 없어야만 한다.
- 하지만 전역공간에 동명의 함수가 여럿 존재하는 상황이더라도 모든 함수가 함수 표현식으로 정의되어 있다면 혼란을 방지할 수 있다.

### 스코프, 스코프 체인, outerEnvironmentReference
> 스코프 scope란, 식별자에 대한 유효범위. 

- 어떤 경계 A의 외부에서 선언한 변수는 A의 외부뿐 아니라 A의 내부에서도 접근이 가능하지만, A의 내부에서 선언한 변수는 오직 A의 내부에서만 접근할 수 있다.
- ES5까지의 자바스크립트는 전역공간을 제외하면 **오직 함수에 의해서만** 스코프가 생성된다.
- '식별자의 유효범위'를 안에서부터 바깥으로 차례대로 검색해나가는 것을 **스코프 체인 scope chain**이라고 한다.
- `LexicalEnvironment`의 두 번째 수집 자료인 `outerEnvironmentReference`가 이를 가능케 함.

#### 스코프 체인 scope chain
`outerEnvironmentReference`는 현재 호출된 함수가 선언될 당시의 `LexicalEnvironment`를 참조한다.

**선언될 당시**, 선언하다는 행위가 일어날 수 있는 시점이란? 콜 스택 상에서 어떤 실행 컨텍스트가 활성화된 상태일 때뿐이다.

어떤 함수를 선언(정의)하는 행위 자체도 하나의 코드에 지나지 않으며, 모든 코드는 실행 컨텍스트가 활성화 상태일 때 실행되기 때문.

`outerEnvironmentReference`는 연결리스트 linked list 형태를 띤다. 

선언 시점의 `LexicalEnvironment`를 계속 찾아 올라가면 마지막엔 전역 컨텍스트의 `LexicalEnvironment`가 있을 것이고, 
각 `outerEnvironmentReference`는 오직 자신이 선언된 시점의 `LexicalEnvironment`만 참조하고 있으므로 가장 가까운 요소부터 
차례대로만 접근할 수 있고 다른 순서로 접근하는 것은 불가능.

이런 구조적 특성 때문에 여러 스코프에서 동일한 식별자를 선언한 경우엔 **무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에게만 접근 가능**하게 됨.

> 스코프, 스코프 체인의 경우 따로 주제를 옮겨서 정리하는 것으로!

----
#### 참조 ✅
- 📚 코어 자바스크립트, 정재남
