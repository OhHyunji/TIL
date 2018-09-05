# phantomJS

TIL

```
- phantomJS을 사용하면 html을 image로 변환할 수 있다.
- phantomJS는 Webkit을 사용하는 렌더링 엔진이다.
```

## phantomJS is

- using Webkit

- a real layout and rendering engine

=> It can capture a web page as a screenshot.

- because PhantomJS can render anything on the web page

=> It can be used to convert HTML content styled with CSS but also SVG, images and Canvas elements.

참고: [Screen Capture with PhantomJS](http://phantomjs.org/screen-capture.html)

## 이걸 어디다가 쓰나

- 쿠폰 이미지에 받는사람이름, 아이템명이 적혀있다고 할 때
- 템플릿(=껍데기)는 있고 안에 내용만 조금씩 다를거다.

이런 경우

1. CouponImageUrl = "couponRendererUrl"

2. couponRenderer서버로 요청이 가면

3. couponRenderer서버에서 coupon서버로 쿠폰정보 달라고 요청을 보내고

4. coupon서버는 coupon.receiverName, coupon.itemName을 가지고 html을 만들어서 return한다.

5. couponRenderer 서버는 `phantomJS`를 활용해서 html => image로 변환한다.

6. couponRendererUrl로 요청을 보낸 곳에서는 image를 전달받는다. 


