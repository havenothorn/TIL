# Parsing
__파싱 구분__

파싱은 어휘 분석과 구문 분석이라는 두 가지로 구분할 수 있다.

1. 어휘 분석
- 자료를 유효한 토큰으로 분해하는 과정. 토큰은 유효하게 구성된 단위의 집합체로 용어집이라고도 할 수 있다.
  - 공백과 줄 바꿈 같은 의미 없는 문자 제거
2. 구문 분석
- 언어의 구문 규칙에 따라 문서 구조를 분석, 파싱 트리 생성 및 적용하는 과정.

파서는 어휘 분석기로 새 토큰을 받고 구문 규칙과 일치하는지 확인한다. 
규칙에 맞다면 토큰에 해당하는 노드가 파싱 트리에 추가되고 파서는 또 다른 토큰을 요청한다.

### HTML 파싱
- HTML 파서는 HTML 마크업을 파싱 트리로 변환한다.
- HTML의 어휘와 문법은 W3C에 명세로 정의되어 있다. 
- 문맥 자유 문법이 아니다.
- 일반적인 파싱 기술을 사용할 수 없기 때문에 브라우저는 HTML 파싱을 위해 별도의 파서를 생성한다.
- [HTML 파싱 참고](https://html.spec.whatwg.org/multipage/parsing.html)

파싱 알고리즘은 토큰화와 트리 구축 두 단계로 되어 있다.
1. 토큰화 (Tokenizer)
- 어휘 분석으로서 입력 값을 토큰으로 파싱한다. HTML에서 토큰은 시작 태그, 종료 태그, 속성 이름과 속성값이다.
- 토큰을 인지해서 트리 생성자로 넘기고 다음 토큰을 확인하기 위해 다음 문자를 확인한다. 
  - 입력의 마지막까지 이 과정을 반복
- 토큰화 알고리즘의 결과물은 HTML 토큰이다.
2. 트리 구축 (TreeBuilder)
- 파서가 생성되면 문서 객체가 생성된다. 
- 트리 구축이 진행되는 동안 문서 최상단에서는 DOM 트리가 수정되고 요소가 추가된다.
- 토큰화에 의해 발행된 각 노드는 트리 생성자에 의해 처리 된다.

__파싱 이후__

- 브라우저는 문서와 상호작용할 수 있게 되고 문서 파싱 이후에 실행되어야하는 "지연"모드 스크립트를 파싱한다.
- 문서 상태는 '완료'가 되고 '로드' 이벤트가 발생한다.
- DOM Tree 생성

__DOM(Document Object Model) 문서 객체 모델__
- 파싱트리는 DOM 요소와 속성 노트의 트리로서 출력 트리가 된다. 
- 이것은 HTML 문서의 객체 표현이고 외부를 향하는 자바스크립트와 같은 HTML 요소의 연결 지점이다.
- 콘텐츠들의 위계관계, 전체적인 구조를 나타내는 자료구조.
- 트리의 최상위 객체는 문서이다.
- DOM은 마크업과 1:1의 관계를 맺는다.

### CSS 파싱
- HTML과는 다르게 CSS는 문맥 자유 문법이다.
  - 공식적인 명세가 있다.
- 파싱 기술이 사용 가능하다. 
- 일반적으로 CSS 링크 코드가 HTML 코드 내에 삽입되어 있기에 HTML 파싱 도중에 CSS 파싱이 시작됨.
- 전체 파일을 모두 다운로드 할 때까지 파싱을 시작할 수 없음.
- 각 CSS 파일은 스타일 시트 객체로 파싱되고 각 객체는 CSS 규칙을 포함한다.
- 파싱 후 CSSOM(Cascading Style Sheets Object Model) Tree 생성
  - 컨텐츠들이 브라우저에 어떻게 보일지 나타냄. 
  - 스타일, 규칙, 선택자 등의 정보가 노드에 들어감.
- DOM Tree와 CSSOM Tree는 Render Tree를 만드는 데 사용됨.

__CSS 꼬리물기 질문__
1. 스타일 규칙(Rule Set)
  - Rule set은 HTML 페이지 안의 특정요소들을 어떻게 렌더링 할 것인지 브라우저에게 알려주는 CSS문장.
  - 선택자(Selector), 선언(Declaration) 내의 속성(Property), 속성값(Property-value)
    - 선언의 집합을 선언 블록이라 칭함.
  - Rule Set의 집합이 스타일 시트를 이루게 됨.
2. 선택자의 종류
  - 전체 선택자(Universal Selector)
    - HTML 내부 모든 태그를 선택
    - `* {...}`
  - 태그 선택자(Type Selector)
  - 클래스 선택자(Class Selector)
  - ID 선택자(ID Selector)
  - 복합 선택자(Combinator)
    - 하위 선택자(Descendant Combinator), 자식 선택자(Child Combinator)
    - 인접 형제 선택자, 일반 형제 선택자
  - 속성 선택자(Attribute Selectors)
  ```
  /* CSS */
  /* E[attr]형식 */
  a[href] { background: yellowgreen; color: black; }

  /* E[attr="val"]형식 */
  input[type="text"] { width: 150px; border: 1px solid #000; }

  /* E[attr$="val"]형식 */
  a[href$=".xls"] { background: darkgreen; }

  <!-- HTML -->
  <a href="one.html">E[attr]형식</a>
  <input type="text" name="name">
  <a href="one.xls">E[attr$="val"]형식</a>
  ```
3. 선택자 우선순위
- CSS 명시도(Specificity) 계산법
```
!important > id [100] > class [10] > tag [1] > * [0]
```

----
### ✅ 참조
- [브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)
- [브라우저 작동방식 탐구 - CSS 파서(Parser) 구현하기](https://url.kr/xqogke)
- [브라우저 동작 과정](https://yozm.wishket.com/magazine/detail/1338/)
- [CSS: 선택자 이해](https://www.nextree.co.kr/p8468/)

