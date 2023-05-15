## z-index 
> CSS `z-index` 속성은 위치 지정 요소와, 그 자손 또는 하위 플렉스 아이템의 Z축 순서를 지정합니다. 더 큰 z-index 값을 가진 요소가 작은 값의 요소 위를 덮습니다.

위치 지정 요소(`position이 static 외의 다른 값인 요소)의 박스에 대해, `z-index`속성은 다음 항목을 지정한다.
1. 현재 쌓임 맥락에서 자신의 위치
2. 자신만의 쌓임 맥락 생성 여부

### 쌓임 맥락?
> Stacking context는 가상의 z축을 사용한 HTML요소의 3차원 개념화. 
- z축은 사용자 기준이며 사용자는 뷰포트 혹은 웹페이지를 바라보고 있을 것으로 가정함.
- 각각의 HTML 요소는 자신의 속성에 따른 우선순위를 사용해 3차원 공간을 차지함.
- `z-index` 속성 값에 영향을 받음

### z-index 적용
- 엘리먼트에 position 속성을 지정하고 z-index 속성을 지정해야한다. 
- 하나의 정수 값을 가질 수 있다.(양수, 음수 전부 가능)
- 유효 범위는 전역이 아니다.
- 웹 접근성을 고려할 때 대체 택스트를 화면에서 숨기는 방법 중 하나로 z-index에 음수를 적용하는 게 있는데 position 속성을 추가해야되는 관계로 권장은 하지 않는다.
    - [대체 텍스트 가리기 좋은 방법](https://boostcourse.org/web344/lecture/47663/?isDesc=false) 

----
### ✅ 참조 
- [z-index Mozilla](https://developer.mozilla.org/ko/docs/Web/CSS/z-index)
- [z-index 적용 Mozilla](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Adding_z-index)
