## Position
> CSS `position` 속성은 문서 상에 요소를 배치하는 방법을 지정한다. 

아래 속성이 요소를 배치할 최종 위치를 결정함.
- `top`
- `right`
- `bottom`
- `left`

### position의 속성
**1. `static` 기본값**
- 요소를 일반적인 문서 흐름에 따라 배치.
- top, right, bottom, left, z-	index 속성이 아무런 영향을 줄 수 없음

**2. `relative`**
- 요소를 일반적인 문서 흐름에 따라 배치.
- 자기 자신을 기준으로, top, right, bottom, left의 값에 따라 오프셋을 적용
    - 다른 요소에는 영향을 주지 않음
    - 페이지 레이아웃에서 요소가 차지하는 공간은 `static`일 때와 같다.
- `z-index`의 값이 `auto`가 아니라면 쌓을 수 있음

**3. `absolute`**
- 요소를 일반적인 문서 흐름에서 제거하고, 페이지 레이아웃에 공간도 배정하지 않음.
- 가장 가까이 위치 지정이 된 조상 요소에 대해 상대적으로 배치함
    - 조상 중 위치 지정 요소가 없다면 초기 컨테이닝 블록을 기준으로 삼음
- 최종 위치는 top, right, bottom, left 값이 지정
- `z-index`의 값이 `auto`가 아니라면 쌓을 수 있음
    - 절대 위치 지정 요소의 바깥 여백은 서로 상쇄되지 않음

**4. `fixed`**
- 요소를 일반적인 문서 흐름에서 제거하고, 페이지 레이아웃에 공간도 배정하지 않음
- 뷰포트의 초기 컨테이닝 블록을 기준으로 배치
    - 단, 요소의 조상 중 하나가 `Transform`, `perspective`,`filter` 속성 중 어느 하나라도 `none`이 아니면 뷰포트 대신 그 조상을 컨테이닝 블록으로 삼습니다.
- 최종 위치는 top, right, bottom, left 값이 지정

**5. `sticky`**
- 요소를 일반적인 문서 흐름에 따라 배치
- 테이블 관련 요소를 포함해 가장 가까운, 스크롤 되는 조상과, 표 관련 요소를 포함한 컨테이닝 블록을 기준으로 배치
- top, right, bottom, left 값에 따라 오프셋 적용 
    - 다른 요소에는 영향을 주지 않음
- 스크롤 동작(overflow가 hidden, scroll, auto 혹은 overlay)이 존재하는 가장 가까운 조상에 달라 붙음
- 스크롤이 불가한 조상의 경우 sticky는 동작하지 않음.

### 배치 유형
- 위치 지정 요소 : `position`의 값이 `relative`, `absolute`, `fixed`, `sticky` 중 하나인 요소, 즉 값이 `static`이 아닌 모든 요소
- 상대 위치 지정 요소 : `position`의 값이 `relative`인 요소. 
- 절대 위치 지정 요소: `position`의 값이 `absolute`, `fixed`인 요소. 
    - 위치의 기준 점이 되는 조상요소 컨테이닝 블록 모서리로부터 거리를 지정. 


----
### ✅ 참조
- [position - Mozilla](https://developer.mozilla.org/ko/docs/Web/CSS/position)
