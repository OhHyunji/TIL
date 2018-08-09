# double not(!!)

TIL

```
var hasA = !!a
```

javascript 에서는 

* false, 0, undefined 등 => false로 처리하고
* true, 1, “aa” 등 => true로 처리한다.

그래서 바로 if문에 아래와같이 사용하는데,

```javascript
if(window.addEventListener) {
	// window.addEventListener가 있으면 뭔가 처리하라.
}
```

이럴 때 !! 를 사용하면 조금 더 명확하다.

```javascript
var hasAddEventListener = !!window.addEventListener
```

without !!

* hasAddEventListener would just point the respective functions,

with !!

* they are boolean value that lets the writer know if the functions exist.

참고: [JavaScript Double Negation (!!) Trick or Trouble?](https://www.sitepoint.com/javascript-double-negation-trick-trouble/)

## airbnb guide

Example1.

```javascript
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```

Example2.

```javascript
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```

참고: [Airbnb JavaScript Style Guide](http://airbnb.io/javascript/)