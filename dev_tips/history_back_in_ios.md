# ios, safari 에서 history.back()

자바스크립트에서 뒤로가기를 처리할 때 

```javascript
history.back() 
```
이렇게 많이쓴다.

브라우저별로 history.back을 처리하는 정책이 조금씩 다른데

### chrome

뒤로가기로 재진입 시, 자바스크립트 코드가 다시 실행된다.

### ios, safari

뒤로가기로 재진입 시, 자바스크립트 코드가 실행되지 않는다.

- 보다 빠른 반응을 위해 BFCache로 페이지를 캐시해놓고
- 저장해놓은 페이지를 바로 로드하기 때문이다.

### onpageshow 

그래서 페이지에 진입할때마다 스크립트를 실행하려면`onpageshow`를 활용해서 뒤로가기로 진입했을 때도 스크립트를 실행하도록 해야한다.

```javascript
window.onpageshow = function (event) {
	if (event.persisted) {
		//뒤로가기로 접근시
		console.log('from cache');
	}
};
```
- onpageshow 반대: onpagehide

### 주의

- onpageshow/onpagehide는 global window event 로 등록된다. 

### Dev Log

ios, safari에서 뒤로가기로 접근시 angular의 $scope도 그대로 유지된다. 


- `$scope.jsondata === undefined` 이면 데이터를 처음부터 새로불러오도록 했는데, 
- ios에서는 뒤로가기로 진입시 $scope도 초기화 되지 않아서, 기존 데이터의 다음페이지가 불려오고 있어서 오늘 픽스했다.

참고:
 
- [ios, safari history.back 문제 ](kdsr2z0.github.io/safari_javascript_cache/)
- [브라우저에서 뒤로가기 수행시 자바스크립트가 실행되지 않는 이유](http://programmingsummaries.tistory.com/380)
- 