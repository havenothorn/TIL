## Flex
> `display: flex;로 지정, 
```html 
<!-- flex를 사용하기 위한 기본 구조-->
<!-- container(부모) 역할을 할 태그-->
<container>
  <!-- item(자식) 역할을 할 태그 -->
	<item></item> 
	<item></item>
</container>
```

### Container 역할을 하는 태그에 부여될 속성
`display`, `flex-direction`, `flex-wrap`, `flex-flow`, `justify-content`, `align-items`, `align-content`

### Item 역할을 하는 태그에 부여될 속성
`order`, `flex-glow`, `flex-shrink`, `flex-basis`, `flex`, `align-self`

----

- `flex- direction` : container에 지정, item 정렬 방향 지정, 기본값 row.
- `flex-basis` : item에 지정, 정렬된 direction의 기본 크기를 지정 
- `flex-grow` : container를 나눌 비율을 지정함. 전체 1을 줄 경우 전부 공평하게 나눔. Item 각각을 다르게 지정하여 비율 다르게 가능. 0을 주면 아무런 변화도 없음 여백 0으로 설정됨.
- `flex-shrink` : grow의 반대, 0으로 준 경우 창의 크기를 줄여도 크기가 줄어들지 않음. 1, 2로 차이를 줄 경우 2를 받은 크기가 더 빨리 줄어들음
- `align-items` : 수직방향 위치, 간격 조절
- `justify-content` : 수평방향 위치, 간격 조절
- `align-content` : item간의 수직방향 위치, 간격 조절
- `order` :  content 내부 item 순서 변경 가능. (음수도 가능)
----
#### ✅ 참조
- [생활코딩 flex 강의](https://opentutorials.org/course/2418/13526)
- [display 속성 - Mozilla](https://developer.mozilla.org/en-US/docs/Web/CSS/display)
- [CSS Flexible Box Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)
