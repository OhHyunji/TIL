# app url scheme

app url scheme을 만들어서 앱을 실행시킬 수 있다.


```
kakaotalk://
naversearchapp://
```

모든 앱들은 이러한 고유 값을 가지고 있다. 이러한 문자열을 `url scheme`이라 부른다.

url scheme을 통해 앱이 실행되는 방식은 다음과 같다.

1. 링크 클릭 시 url scheme이 sytem에 전달된다.
2. system에서 전달된 url scheme을 보고 실행가능한 앱이 있는지 확인한다.
3. 해당 url scheme을 받을 수 있는 앱이 있으면 그 앱을 실행시키고, 이 url을 전달한다. 
	* 앱이 실행되면서 url에 포함된 내용을 참조해서 특정 기능을 수행한다.
4. 해당 url을 받을 수 있는 앱이 없으면 마켓으로 이동한다. 

> web application routes의 앱 버전이다. webapp의 url과 비슷한 기능으로, 뒤에오는 path를 가지고 앱 내에 이동하고싶은 곳으로 이동할 수 있다.

참고: [네이버 앱 URL Scheme](https://developers.naver.com/docs/utils/mobileapp/)