## float 
> CSS 속성(property) `float`은 한 요소가 보통 흐름으로부터 빠져 텍스트 및 인라인(inline)요소가 그 주위를 감싸는 자기 컨테이너의 좌우측을 따라 배치되어야 함을 지정하는 것.
```css
float: none;
float: left;
float: right;
```
- img 태그에 사용하는 걸 기본으로 했음
- 블록의 성격을 무시함
- 형제 구조로 이루어져있음
- 높이 값이 맞아야 함

### float 해제하기
1. `clear: both;`
- 텍스트나 모든 부동 요소 하단 아래로 넘어갈 만큼 길지 않다면, 제대로 된 정렬을 위해 `float`을 `clear`해야함.
- 정렬 되는 지 확인하고싶은 새 머릿글에 `clear` 속성을 추가하는 것
- 지정한 방향 `float` 여백 가장자리 아래가 될 때 까지 요소의 테두리 가장자리를 아래로 이동함. `float`되지 않은 블록의 상단 여백이 축소됨
- 요소에 `float` 요소만 포함되어있으면 높이가 줄어들어 아무것도 남지 않음.
    - 항상 크기를 조정할 수 잇도록 요소의 `display` 값을 `flow-root`로 설정한다.
2. `overflow: hidden;`
- 자손에 `float` 속성을 적용하면 부모의 `overflow`속성에 `hidden`키워드 적용
- 스크롤이 생겨야하는데 생기지 않게 막는 경우가 발생할 수 있음

----
### ✅ 참조
- [무엇이 제일 나은 float 해제 기법인가?](https://selosele.github.io/2020/07/21/clearfix/)
- [float - Mozilla](https://developer.mozilla.org/ko/docs/Web/CSS/float) 
- [clear - Mozilla](https://developer.mozilla.org/ko/docs/Web/CSS/clear)
