## async 와 defer
> TL;DR
> - DOM을 따라 반드시 순서대로 실행되어야 한다면 `<script>`
> - DOM이나 다른 스크립트에 의존성이 없고, 실행 순서가 중요하지 않은 경우라면 `<script async>`
> - DOM이나 다른 스크립트에 의존성이 있고, 실행 순서가 중요한 경우라면 `<script defer>`

```html
<script src="./script.js"></script>
<script src="./script.js" async></script>
<script src="./script.js" defer></script>
```

- DOM Tree와 CSSOM Tree가 결합되어 Render Tree 구축 과정 중에 렌더 블로킹이 일어날 수 있다.
  - 렌더 블로킹이 일어나는 이유 중, 자바스크립트로 인한 렌더 블로킹 상황을 찾아보았다.
- 자바스크립트는 DOM Tree에서 요소를 추가하고 제거하여 콘텐츠를 수정하거나 각 요소의 CSSOM 속성을 수정하거나, 사용자 입력을 처리하는 등의 많은 작업을 수행할 수 있다.
- DOM 파싱을 진행하다 `<script>`태그를 만나게 되면 DOM 파싱을 중지하고 자바스크립트 엔진에 제어권한을 넘긴다.
- 인라인 스크립트를 실행하게되면 DOM 파서 블로킹을 하게 되고 결과적으로 렌더 블로킹도 같이 일어나 초기 렌더링이 지연된다.

```html
<script>
  // `null` 의 `innerHTML` 에 접근할 수 없으므로 에러 발생.
  console.log(document.getElementById("hello").innerHTML);
</script>

<div id="hello">안녕하세요</div>

<script>
  // `안녕하세요` 출력.
  console.log(document.getElementById("hello").innerHTML);
</script>
```
- 예시처럼 스크립트를 `<body>`태그 안, 맨 아래 놓는 경우도 페이지 콘텐츠 출력을 막지 않아 하나의 방법이 될 수 있다. 
- Bootstrap 같은 라이브러리 문서에도 위처럼 권장.
- 스크립트 해석 도중 사용자가 웹과 상호작용을 시도할 때, 제대로 동작하지 않음.
  - 겉보기엔 완성된 웹사이트에 접속해있지만 백그라운드에서 JS를 해석하고 실행 중이기 때문 
  - 모바일처럼 인터넷 연결이 원활하지 않은 환경에서 자주 발생 가능

- 브라우저가 스크립트 파일을 병렬로 불러오는 방식으로 DOM 렌더 과정을 막지 않게 선언할 수 있음.
  - 이를 가능하게 하는 키워드는 `<script>`속성으로 `async`, `defer`이다.

## async
- `async` 스크립트는 DOM 렌더 과정을 방해하지 않도록 병렬로 로드함.
- 브라우저가 DOM을 구성하는 동시에 백그라운드에서 스크립트를 불러올 수 있다.
- 오직 파일을 불러오는 것만 병렬로 실행한다는 것이 중요.
  - 실행 순서가 보장되지 않음
- 서로 다른 시간이 걸리는 스크립트가 있다면 먼저 로드가 되는 스크립트가 실행.
  - 서로 의존성이 있다면 제대로 동작이 안됨.
- 완전히 비동기로 불러오기 때문에 DOM이 모두 로드된 경우 발생하는 `DOMContentLoaded` 이벤트 콜백으로 로드를 보장할 수 없음.  
- 파일의 로딩을 마치고, 즉시 DOM 렌더를 멈추며 스크립트 파일 해석을 시작. 
  - 스크립트의 해석이 얼마나 오래 걸릴지는 스크립트 파일의 *오버헤드에 달려잇음.
  - *오버헤드 : 특정 기능을 수행하는데 추가적으로 드는 간접적인 시간, 메모리 등.
- 즉, 때문에 `async 스크립트는 DOM에 직접 접근하지 않거나, 다른 스크립트에 의존적이지 않은 스크립트들을 독립적으로 실행해야할 때 효과적임.
```html
<!-- Google Analytics 같은 서드파티 스크립트는 기존의 어플리케이션과 완전히 독립적으로 동작하므로 async가 어울립니다 -->
<script async src="https://google-analytics.com/analytics.js"></script>
```

## defer
- `defer` 스크립트도 `async`와 같이 DOM 렌더를 방해하지 않고 병렬로 로드
- `async` 스크립트와는 다르게, `defer` 의 경우 모든 DOM이 로드된 후에 실행.
- 선언한대로 실행 순서가 보장
- `defer` 는 먼저 로드한 스크립트라 하더라도 실행하는 시점을 지연시키는 것이기 때문에, `DOMContentLoaded` 이벤트 발생 전에 이미 실행된 상태.
- 때문에 DOM의 모든 엘리먼트에 접근할 수 있고, 실행 순서도 보장하기에 가장 범용적으로 사용할 수 있는 속성.
- 서로 의존성이 있는 경우에도 사용하기 좋음.
- `src` 속성이 없는 경우 `defer`의 효과는 무력화
- inline script를 `defer`처럼 DOM 작업이 완료된 후 스크립트를 실행하고 싶다면 `DOMContentLoaded`에 `addEventListener`를 추가하여 사용하면 된다.

```js
addEventListener("DOMContentLoaded", (event) => {});
onDOMContentLoaded = (event) => {};
```

----
### ✅ 참조
- [렌더 블로킹](https://velog.io/@soorokim/Render-Blocking)
- [스크립트 실행 시점을 조절하는 async/defer](https://wormwlrm.github.io/2021/03/01/Async-Defer-Attributes-of-Script-Tag.html) 
- [defer, async](https://ko.javascript.info/script-async-defer)
- [왜 Async보다는 Defer를 써야할까](https://yceffort.kr/2020/10/defer-than-async)
- [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event)
