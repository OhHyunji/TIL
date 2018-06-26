# href="javascript:;"

## a 태그를 사용해야 하는데 href 링크이동은 막고싶을 때

```
<a href="javascript:;" onclick=callFunction()>test</a>
```

## 참고 

예전에는 이렇게 쓰기도 했다.

```html
<a href="#" onclick=callFunction()>test</a>
```
그런데 이 방법은 문제가있다.

- onclick 함수 호출 후 이벤트 버블링으로 인해 anchor를 타서 브라우저 상단으로 이동한다.

href에 javascript 코드를 넣을 수 있다.

```
<a href="javascript:alert('hihi');" onclick=callFunction()>test</a>
```

이걸 응용해서 href 에 undefined 를 return 되게 하자.

```
<a href="javascript:void(0);" onclick=callFunction()>test</a>
```

그리고 이 방법을 한번 더 개선한 것이

```
<a href="javascript:;" onclick=callFunction()>test</a>
```

참고: [href="#"의 깔끔한 대안](http://hardworker.tistory.com/4)

참고: [a태그에서 href, onclick사용](http://thingsthis.tistory.com/130)