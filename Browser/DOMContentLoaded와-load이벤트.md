## DOMContentLoaded와 load 이벤트
➕ JQuery의 ready()와 load()와의 공통점과 차이점.

### 1. DOMContentLoaded
> `DOMContentLoaded` 이벤트는 초기 HTML 문서를 완전히 불러오고 분석했을 때 발생합니다. 스타일 시트, 이미지, 하위 프레임의 로딩은 기다리지 않습니다.
  - 쉽게 말해서 브라우저가 HTML 파싱을 다 하고 DOM 트리를 완성하는 즉시 발생한다. 
  - 브라우저 렌더링 과정 1단계 후라고 보면 됨.
- DOM이 준비된 것을 확인 후 원하는 DOM 노드를 찾아 ***핸들러**를 등록하는 등의 인터페이스 초기화 작업에 유용할 수 있다.
- `addEventListener를 한 시점이 이미 이벤트가 일어난 시점(DOM이 준비된)일 경우 호출되지 않음.
- JQuery의 `ready()`와 동일한 시점에 Trigger된다. 하지만 작동방식에는 차이가 있음. 아래에 추가 설명
- load 이벤트보다 먼저 발생, 빠른 실행 속도가 필요할 때 적합

### 2. load(onload)
> HTML로 DOM 트리를 완성되었을 뿐만 아니라, 이미지, 외부 스타일시트 같은 외부 자원도 모두 불러오는 것이 끝났을 때 발생.
- 이미지 사이즈를 확인할 때 등, 외부 자원이 모두 로드된 이후에 적용할 수 있는 작업
- 차례대로 읽는 인터프리터 언어적 특성으로 인해 js 작성 위치에 따라 오작동을 일으킬 때 이용
  - `window.onload()`를 오버라이딩(재정의)해주어 돔트리 완성 후 실행하게끔 함.
- js에서 페이지가 로드되면 자동으로 실행되는 전역 콜백함수이다.
- 동일한 문서에 오직 `onload`는 하나만 존재해야한다.
  - 외부 링크나 파일 include시 그 안에 `window.onload` 스크립트가 있으면 둘 중 하나만 적용
  - 중복시 마지막 선언이 실행, 외부 라이브러리에서 이미 선언된 경우 하나로 합피는 과정이 필요
  - html `<body>` 요소에 속성으로 지정될 수 있음

    ```html
    <body onload="실행될 코드">
    <!-- 위와 같이 사용될 경우, window.onload로 지정된 것은 무시됨 -->
    ```
- 불필요한 로딩시간이 추가될 수 있음
- JQuery의 `load()`와 동일

----
__`DOMContentLoaded`와 jQuery `document.ready()`차이점__ 
```js
document.addEventListener('DOMContentLoaded', function(){});

$(document).ready(function(){});
```
- HTML파싱 후 DOM이 준비되어 조작해도 되는 상태가 되어 호출되는 시점은 동일하다.
- `DOMContentLoaded`의 경우 `addEventListener`한 시점이 이미 DOM Tree가 구축된 이후라면 호출되지 않는다.
- `document.ready()`의 경우, 이미 호출 전에 `DOMContentLoaded`이벤트가 일어났더라도, ready안의 function이 호출된다.

__이벤트 처리기__
> 이벤트가 발생하는 것을 감지하는 역할. 이벤트가 발생했을 때 이벤트를 받은 다음 처리기 안에 있는 코드를 실행한다. 대개 특정 기능을 하는 함수가 실행 대상.
1. ***이벤트 핸들러** (Event handler)
  - 하나의 요소에 하나의 이벤트 처리 가능
    - (1) HTML 태그에 설정하는 방법
   
      ```html
        <button onclick="showSideNav()">
        <!-- 간단한 방법이지만 HTML과 JS코드가 섞여있어 유지보수가 힘들다. -->
      ```
    - (2) DOM 요소 객체의 이벤트 처리기 프로퍼티 설정하는 방법
   
      ```js
      let sideNav = document.getElementById("sideNav");
      let btn = document.getElementById("btn");
      btn.onclick = showSideNav();

      function showSideNav() {
        sideNav.style.display = "block";
      }
      // HTML과 JS를 분리 가능하나, DOM요소 객체에 하나의 이벤트 핸들러만 등록되는 단점
      ```
2. 이벤트 리스너 (Event Listener)
  - 하나의 요소에 복수의 이벤트 처리 가능.
  - `addEventListener` 메서드 사용, ↔️ `removeEventListener` 메서드로 삭제 가능
   
    ```js
    target.addEventListener(type, listener, useCapture);

    let btn2 = document.getElementById("btn2");

    // 같은 요소의 같은 이벤트에 이벤트 리스너 여러개 등록 가능
    // 버블링, 캡처링 단계 모두에서 활용 가능
    // 이벤트 전파 제어 가능
    // HTML 태그를 포함한 모든 DOM 노드에 이벤트 리스너 등록 가능
    ```
  -  구성요소
     - target : 이벤트 리스너를 등록한 DOM요소 or 객체, 노드
     - type : 이벤트 유형을 뜻하는 문자열, 기존 이벤트 처리기 이름에서 on을 생략(click, mouseover..)
     - listener : 이벤트가 발생했을 때 처리를 담당하는 콜백 함수의 참조
     - useCapture : 이벤트 단계
        1. true : 캡처링 단계
        2. false : 버블링 단계 (default)
   
  
  



----
### ✅ 참조
- [[Javascript] 자바스크립트 이벤트 핸들러와 리스너](https://tangoo91.tistory.com/31)
- [[Javascript] onload, ready](https://bogyum-uncle.tistory.com/206)
- [Window: load event - Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event)
- [DOMContentLoaded - Mozilla](https://developer.mozilla.org/ko/docs/Web/API/Window/DOMContentLoaded_event)
- [DOMContentLoaded, load, beforeunload, unload 이벤트](https://ko.javascript.info/onload-ondomcontentloaded)
- [[모던JS: 브라우저] 문서와 리소스 로딩](https://velog.io/@longroadhome/%EB%AA%A8%EB%8D%98JS-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%AC%B8%EC%84%9C%EC%99%80-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EB%A1%9C%EB%94%A9)
- [[JQuery ready(), load() 차이]](https://jmjmjm.tistory.com/14)
- [DOMContentLoaded와 jQuery document.ready()의 중요한 차이점](https://jizard.tistory.com/307)
