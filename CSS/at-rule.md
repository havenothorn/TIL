## @-rule(at-rule)
> CSS가 동작하는 방법을 지시하는 CSS문. 
```css
/* General structure */
@identifier (RULE);

/* Example: tells browser to use UTF-8 character set */
@charset “utf-8”
```
1. `@import`규칙
> CSS파일에서 다른 CSS파일을 추가하는 방법

```html 
<head>
  <title>link 태그를 사용한 스타일시트 추가(기본)</title>
  <link rel="stylesheet" href="stylesheet1.css">
  <link rel="stylesheet" href="stylesheet2.css">
</head>
```
```html
<head>
  <title>@import 규칙을 사용한 스타일시트 추가</title>
  <style>
    @import url(stylesheet1.css);
    @import url(stylesheet2.css);
  </style>
</head> 
```
2. `@font-face`규칙
> 지원하지 않는 폰트는 자체적으로 지원해야함. 웹폰트를 생성할 때 사용하는 규칙

```css
@font-face {
font-family : 'font-name';
src: url('/content/file.eot');
src: local('🤓'), url('/content/file.woff') format('woff'),
  url('/content.file.ttf') format('truetype');
}
* {
  font-family: 'font-name';
}
```
3. `@media`규칙
> 다양한 특정 장치에서 HTML 문서가 적절한 형태를 갖추게 만들어주는 규칙.
- 미디어 쿼리를 함께 사용.
- 음성 장치부터 점자는 물론 프린터와 TV에 맞게 스타일시트를 사용할 수 있음.
```html
<!-- 데스크탑 브라우저에서 적용되게끔 -->
<link rel="stylesheet" href="desktop.css" media="screen"> 
<!-- 프린트시 적용되게끔 -->
<link rel="stylesheet" href="print.css" media="print">
```
```css
/* 화면 너비 0픽셀 ~ 767픽셀 */
@media screen and (max-width: 767px) {
}
/* 화면 너비 768픽셀 ~ 959픽셀 */
@media screen and (min-width: 768px) and (max-width: 959px) {
}
/* 화면 너비 960픽셀 ~ 무한 */
@media screen and (min-width: 960px) {
}
/* portrait: 뷰포트가 세로일 때 */
@media screen and (orientation: portrait){
}
/* landscape: 뷰포트가 가로일 때 */
@media screen and (orientation: landscape){
}
```
