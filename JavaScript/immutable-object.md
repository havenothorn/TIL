## 불변객체

- 참조형 데이터의 '가변'은 데이터 자체가 아닌 내부 프로퍼티를 변경할 때만 성립
- 데이터 자체를 변경하고자 하면 기본형과 마찬가지로 **기존 데이터는 변하지 않음**
- 내부 프로퍼티를 변경할 때마다 새로운 객체를 재할당하기로 하거나, 자동화 도구를 활용한다면 객체는 불변성을 확보할 수 있음.
- 불변성을 확보할 필요가 있을 경우 불변 객체로 취급, 그렇지 않은 경우 기존 방식대로 사용.

### When?
> 값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 유지해야될 경우, 불변객체가 필요함.

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

