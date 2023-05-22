## 불변객체

- 참조형 데이터의 '가변'은 데이터 자체가 아닌 내부 프로퍼티를 변경할 때만 성립
- 데이터 자체를 변경하고자 하면 기본형과 마찬가지로 **기존 데이터는 변하지 않음**
- 내부 프로퍼티를 변경할 때마다 새로운 객체를 재할당하기로 하거나, 자동화 도구를 활용한다면 객체는 불변성을 확보할 수 있음.
- 불변성을 확보할 필요가 있을 경우 불변 객체로 취급, 그렇지 않은 경우 기존 방식대로 사용.

### When?
> 값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 유지해야될 경우, 불변객체가 필요함.

1. 객체의 가변성에 따른 문제점
```js
var user = {
  name: 'Huijeong'
  gender: 'female'
};

var changeName = function (user, newName) {
  var newUser = user;
  newUser.name = newName;
  return newUser;
};

ver user2 = changeName(user, 'Nam');

if (user !== user2) {
  console.log('유저 정보가 변경되었습니다.');
}
console.log(user.name, user2.name);    // Nam Nam
console.log(user === user2);           // true
```
- 정보가 바뀐 시점에 알림을 보내야하거나, 바뀌기 전의 정보와 바뀐 후의 정보의 차이를 보여줘야 하는 등의 기능을 구현해야한다면?
  - 변경 전과 후에 서로 다른 객체를 바라보게 만들어야함.

2. 객체의 가변성에 따른 문제점의 해결방법
```js
var user = {
  name: 'Huijeong',
  gender: 'female'
};

var changeName = function (user, newName) {
  return {
    name: newName,
  `  gender: user.gender
  };
};

var user2 = changeName(user, 'Nam');

if (user !== user2) {
  console.log('유저 정보가 변경되었습니다.')  // 유저 정보가 변경되었습니다.
}
console.log(user.name, user2.name);    // Huijeong Nam
console.log(user === user2);           // false
```

- `changeName` 함수가 새로운 객체를 반환하도록 수정했으나, 변경할 필요가 없는 기존 객체의 프로퍼티(gender)도 하드코딩으로 입력이됨.
- 대상 객체에 정보가 많을수록, 변경해야할 정보가 많을수록 사용자가 입력하는 수고가 늘어남.

3. 기존 정보를 복사해서 새로운 객체를 반환하는 함수 (얕은 복사)
```js
var copyObject = function (target) {
  var result = {};
  for (var prop in target) {
    result[prop] = target[prop];
  }
  return result;
};
//copyObject는 for in 문법을 이용해 result 객체에 target 객체의 프로퍼티들을 복사하는 함수.
var user = {
  name: 'Huijeong',
  gender: 'female'
};

var user2 = copyObject(user);
user2.name = 'Nam';

if (user !== user2) {
  console.log('유저 정보가 변경되었습니다.')  // 유저 정보가 변경되었습니다.
}
console.log(user.name, user2.name);    // Huijeong Nam
console.log(user === user2);           // false
```
- '얕은 복사만을 수행한다.'는 점이 아쉬움.

### 얕은 복사와 깊은 복사
- 얕은 복사 (shallow copy): 바로 아래 단계의 값만 복사하는 방법
  - 중첩된 객체에서 참조형 데이터가 저장된 프로퍼티를 복사할 때 그 주솟값만 복사한다.
  - 원본과 사본 모두 동일한 참조형 데이터의 주소를 가리켜, 한 가지 수정시 둘다 수정됨.
- 깊은 복사 (deep copy): 내부의 모든 값을 하나하나 찾아서 전부 복사하는 방법

1. 중첩된 객체에 대한 얕은 복사
```js
var user = {
  name: 'Huijeong'
  urls: {
    portfolio: 'http://github.com/havenothorn',
    blog: 'https://velog.io/@chocoallergie',
    facebook: 'http://facebook.com/abc'
  }
};
var user2 = copyObject(user);

user2.name = 'Nam';
console.log(user.name === user2.name);                      // false

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio);  // true

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog);            // true 
```
- 한 단계 더 들어간 프로퍼티는 주솟값만 그대로 참조하여 원본과 사본 전부 바뀌는 걸 알 수 있음.
2. 중첩된 객체에 대한 깊은 복사
```js
// 1번과 공통된 코드는 생략

var user2 = copyObject(user);
user2.urls = copyObject(user.urls);

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio);  // false

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog);            // false
```
- 어떤 객체를 복사할 때 객체 내부의 모든 값을 복사해서 완전히 새로운 데이터를 만들고자 할 때, 객체의 프로퍼티 중에서 
그 값이 **기본형 데이터일 경우에는 그대로 복사**하면 되지만 **참조형 데이터는 다시 그 내부의 프로퍼티들을 복사**해야함.
  - 이 과정을 참조형 데이터가 있을 때마다 재귀적으로 수행해야만 깊은 복사가 됨.

3. 객체의 깊은 복사를 수행하는 범용 함수
```js
var copyObjectDeep = function(target) {
  var result = {};
  if (typeof target === 'object' && target !== null) {  //  typeof null이 object 반환값을 가지기 때문에 이 조건을 덧붙임.
    for (var prop in target) {
      result[prop] = copyObjectDeep(target[prop]);
    }
  } else {
    result = target; 
  }
}
```
3-1. 깊은 복사 결과 확인
```js
var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2]
  }
};
var obj2 = copyObjectDeep(obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);   // { a: 1, b: { c: null, d: [1, 3] } }
console.log(obj2);  // { a: 3, b; { c: 4,    d: {0:1, 1:2} }}
```

4. 그 외
- `hasOwnProperty` 메서드를 활용해 프로토타입 체이닝을 통해 상속된 프로퍼티를 복사하지 않게끔 할 수 있음.
- ES5의 `getter/setter`를 복사하는 방법
  - ES6의 `Object.getOwnPropertyDescriptor` 또는 ES2017의 `Object.getOwnPropertyDescriptors`
- 객체를 JSON 문법으로 표현된 문자열로 전환 후 다시 JSON 객체로 바꾸는 방법
  - 메서드(함수)나 숨겨진 프로퍼티인 `__proto__`나 `getter/setter`등과 같이 JSON으로 변결할 수 없는 프로퍼티들은 무시함.
  - `httpRequest`로 받은 데이터를 저장한 객체를 복사할 때 등 순수한 정보만 다룰 때 활용하기 좋음.

4-1. JSON을 활용한 간단한 깊은 복사
```js
var copyObjectViaJSON = function (target) {
  return JSON.parse(JSON.stringify(target));
};
var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2],
    func1: function () { console.log(3); }
  },
  func2: function () { console.log(4); }
};
var obj2 = copyObjectViaJSON(obj);

obj2.a = 3;
obj2.b.c = 4;
obj.b.d[1] = 3;

console.log(obj);   // { a: 1, b: { c: null, d: [1, 3], func1: f() }, func2: f()}
console.log(obj2);  // { a: 3, b: { c: 4,    d: [1, 2 ]}}
```

### 정리
- 자바스크립트 데이터 타입에는 기본형과 참조형이 있다. 기본적으로 기본형은 불변값이고, 참조형은 가변값이다.
- 변수는 변경 가능한 데이터가 담길 수 있는 공간이고, 식별자는 변수의 이름을 말한다.
- 변수를 선언하면 컴퓨터는 우선 메모리의 빈 공간에 식별자를 저장하고, 그 공간에 자동으로 `undefined`를 할당한다.
  1. 이후 그 변수에 기본형 데이터를 할당 시, 별도의 공간에 데이터를 저장하고 그 공간의 주소를 변수의 값 영역에 할당한다.
  2. 참조형 데이터 할당 시, 컴퓨터는 참조형 데이터 내부 프로퍼티들을 위한 변수 영역을 별도로 확보해서 확보된 주소를 변수에 연결한다. 
    앞서 확보한 변수 영역에 각 프로퍼티의 식별자를 저장하고, 각 데이터를 별도의 공간에 저장해서 그 주소를 식별자들과 매칭시킴.
- 할당과정에서 기본형과 참조형에 차이가 생긴 이유는 참조형 데이터가 여러 개의 프로퍼티(변수)를 모은 '그룹'이기 때문.
  - 이 차이로 인해 참조형 데이터를 '가변값'으로 여기게 됨.
- 참조형 데이터를 불변값으로 사용하는 방법
  - 내부 프로퍼티 일일이 복사하기 (깊은 복사)
  - 라이브러리 사용

----
#### ✅ 참조
- 📚 '코어 자바스크립트', 정재남
