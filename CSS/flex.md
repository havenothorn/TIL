## Flex 플렉스
> `display: flex;로 지정, Flexible Box, Flexbox라고도 부름.
- 레이아웃 배치 전용 기능으로 고안. 
 - `float`이나 `inline-block` 등을 이용한 기존 방식보다 훨씬 편리
- 부모 요소인 div.container는 Flex Container(플렉스 컨테이너)
- 자식 요소인 div.item들은 Flex Item(플렉스 아이템)
- 컨테이너가 Flex의 영향을 받는 전체 공간이고, 설정된 속성에 따라 각각의 아이템들이 어떤 형태로 배치되는 것

```html 
<!-- flex를 사용하기 위한 기본 구조-->
<!-- container(부모) 역할을 할 태그-->
<container>
  <!-- item(자식) 역할을 할 태그 -->
	<item></item> 
	<item></item>
</container>
```
### Flex의 속성
#### 1. Container 역할을 하는 태그에 부여될 속성
`display:flex`, `flex-direction`, `flex-wrap`, `flex-flow`, `justify-content`, `align-items`, `align-content`

- `display:flex`: 선언시 flex 아이템들은 가로 방향으로 배치되고, 자신이 가진 내용물의 width만큰만 차지.
	- inline 요소처럼. height는 컨테이너의 높이만큼 각각 늘어남.
- `flex- direction` : container에 지정, item 정렬 방향 지정, 기본값 row.
- `align-items` : 수직방향 위치, 간격 조절
- `justify-content` : 수평방향 위치, 간격 조절 (메인 축 방향 - 오뎅꼬치 방향)
- `align-content` : item간의 수직방향 위치, 간격 조절 (수직 축 방향 - 오뎅 뜯는 방향)

#### 2. Item 역할을 하는 태그에 부여될 속성
`order`, `flex-grow`, `flex-shrink`, `flex-basis`, `flex`, `align-self`

- `order` :  content 내부 item 순서 변경 가능. (음수도 가능)
- `flex-grow` : container를 나눌 비율을 지정함. 전체 1을 줄 경우 전부 공평하게 나눔. Item 각각을 다르게 지정하여 비율 다르게 가능. 0을 주면 아무런 변화도 없음 여백 0으로 설정됨.
- `flex-shrink` : grow의 반대, 0으로 준 경우 창의 크기를 줄여도 크기가 줄어들지 않음. 1, 2로 차이를 줄 경우 2를 받은 크기가 더 빨리 줄어들음
- `flex-basis` : item에 지정, 정렬된 direction의 기본 크기를 지정 


----
#### ✅ 참조
- [생활코딩 flex 강의](https://opentutorials.org/course/2418/13526)
- [1분코딩 - 이번에야말로 CSS Flex를 익혀보자](https://studiomeal.com/archives/197)
- [display 속성 - Mozilla](https://developer.mozilla.org/en-US/docs/Web/CSS/display)
- [CSS Flexible Box Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)
