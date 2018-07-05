# $rootScope

TIL
 
```
angular에서 route(api path → 컴포넌트)로 이동하기 전에 뭔가 처리가 필요할 때(hook):
$rootScope.$on 에 이벤트를 걸자.
```

## $rootScope

- `$rootScope`는 inject해서 쓸 수 있다.

## $rootScope.$on(name, listener)

Listens on events of a given type. 

```javascript
 $rootScope.$on('$locationChangeSuccess', function (object, newLocation, oldLocation) {
 	//do something
 }                                                    
```
### name 

- angular 에서 정의한 이벤트들을 지정할 수 있다.
- `$location`의 경우 `$locationChangeStart`, `$locationChangeSuccess` 이벤트를 hook 할 수 있다.

참고: [angular docs - $location](https://docs.angularjs.org/api/ng/service/$location)

### listener

- listener function format is: `function(event, args...)`
- event: the object passend into the listener has following attributes:
	- targetScope
	- currentScope
	- name: name of the event.
	- stopPropagation
	- preventDefault
	- defaultPrevented
	
참고: [angular docs - $rootScope.$on](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$on)

