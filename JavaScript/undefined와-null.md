## undefined와 null
- 자바스크립트에 '없음'을 나타내는 두 개의 값, `undefined`와 `null`

### undefined
> 사용자가 명시적으로 지정할 수도 있지만, 주로 JavaScript 엔진이 자동으로 부여한다.
> 
- JS엔진은 사용자가 어떤 값을 지정할 것이라고 예상되는 상황에 실제로 그렇게 하지 않았을 때 `undefined`를 반환한다.
1. 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 얂은 식별자에 접근할 때.
2. 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
3. return 문이 없거나, 호출되지 않는 함수의 실행 결과

#### 자동으로 `undefined`를 부여하는 경우
```js
var a;
console.log(a);         //  1. undefined. 값을 대입하지 않은 변수에 접근

var obj = { a: 1; }
console.log(obj.a);     //  1
console.log(obj.b);     //  2. 존재하지 않는 프로퍼티에 접근
console.log(b);         //  c.f) ReferenceError: b is not defined

var func = function() {};
var c = func();         //  3. 반환(return) 값이 없으면 undefined를 반환한 것으로 간주
console.log(c);         //  undefined
```
#### undefined와 배열
```js
var arr1 = [];
arr1.length = 3;
console.log(arr1);      // [empty x 3]

var arr2 = new Array(3);
console.log(arr2);      // [empty x 3]

var arr3 = [undefined, undefined, undefined];
console.log(arr3);      // [undefined, undefined, undefined]
```
- '비어있는 요소'와 `undefined`를 할당한 요소는 출력 결과가 다름.
- '비어있는 요소'는 순회와 관련된 많은 배열 메서드들의 순회 대상에서 제외됨.
#### 빈 요소와 배열의 순회
```js
var arr1 = [undefined, 1];
var arr2 = [];
arr2[1] = 1;

arr1.forEach(function (v, i) { console.log(v, i); })        // undefined 0 / 1 1
arr2.forEach(function (v, i) { console.log(v, i); })        // 1 1

arr1.map(function (v, i) { return v + i; })                 // [NaN, 2]
arr2.map(function (v, i) { return v + i; })                 // [empty, 2]

arr1.filter(function (v) { return !v; });                   // [undefined]
arr2.filter(function (v) { return !v; });                   // []

arr1.reduce(function (p, c, i) { return p + c + i; }, '');  // undefined011
arr2.reduce(function (p, c, i) { return p + c + i; }, '');  // 11
```
- 배열은 무조건 `length` 프로퍼티의 개수만큼 빈 공간을 확보할 것이라고 생각하기 쉽지만, 
  실제로는 객체와 마찬가지로 특정 인덱스에 값을 지정할 때 비로소 빈 공간을 확보하고 인덱스를 이름으로 지정하고 데이터의 주솟값을 지정하는 등의 동작을 한다.
- 즉, 값이 지정되지 않은 인덱스는 '아직 존재하지 않는 프로퍼티'로 취급되는 것.
- 사용자가 명시적으로 `undefined`를 부여한 경우 '비어있음'을 의미하지만 하나의 값으로 동작, 프로퍼티나 배열의 요소는 고유의 키값(프로퍼티 이름)이 실존하게 됨.
  - 순회의 대상이 될 수 있다는 것.
  - 실존하는 데이터
- JS엔진이 반환해주는 `undefined`는 해당 프로퍼티 내지 배열의 키값(인덱스) 자체가 존재하지 않음을 의미.
  - 문자 그대로 '값이 없음'을 나타냄 
  - **`var` 변수는 `environmentRecord`가 인스턴스화될 때 생성되면서 `undefined`로 초기화 된다.**
  - ES6에서 등장한 `let`, `const`는 `undefined`를 할당하지 않은 채로 초기화를 마치며,
    이후 실제 변수가 평가되기 전까지는 해당 변수에 접근할 수 없음.
- 사용자가 직접 `undefined`를 할당하지 않는 것을 권장

### null
> '비어있음'을 명시적으로 나타내기 위한 용도로 만든 데이터 타입

- 명시적으로 나타낼 땐 `null`을 사용하고, `undefined`는 오직 '값을 대입하지 않은 변수에 접근하고자 할 때 JS엔진이 반환해주는 값'으로만 존재하도록 한다.
- 주의할 점: `typeof null`은 `object`로 반환된다.
  - 자바스크립트의 대표적인 자체 버그이다.
  
#### undefined와 null의 비교
```js
var n = null;
console.log(typeof n);        // object

console.log(n == undefined);  // true
console.log(n == null);       // true

console.log(n === undefined); // false
console.log(n === null);      // true
```
- 동등 연산자 equlity operator(==)로 비교할 경우 `null`과 `undefined`는 같다고 판단된다.
- 일치 연산자 identity operator(===)를 써야 정확히 판별 가능.

----
#### ✅ 참조
- 📚 '코어 자바스크립트', 정재남
