# referrer 안남게 location 이동하기

TIL

```
document.referrer
- 현재페이지로 이동시킨 이전페이지 URI정보를 반환한다.
- 빈 문자열이면 첫페이지인거다.

a 태그 attribute로 location이동시 referrer 안남게 할 수 있다.
- rel="noreferrer"
```

## document.referrer

- 현재 페이지로 이동시킨 이전페이지 URI정보를 반환한다.
- 주소를 직접 타이핑했거나 북마크를 눌러서 들어온 경우, 빈 문자열을 반환한다. 
- 빈 문자열 == 첫페이지임을 알 수 있다.
- visibleBackButton 조건으로 활용할 수 있다.

참고: [MDN - document.referrer](https://developer.mozilla.org/ko/docs/Web/API/Document/referrer)

참고: [뒤로가기 히스토리가 없는것을 어떻게 알 수 있을까?](http://programmingsummaries.tistory.com/318)

## referrer 안남게 location 이동하기

중간에 낚아채서 location을 임의로 이동한 것인데 backButton이 노출되면 난감하다;

이런 경우 referrer 안남게 location을 이동시킬 수 있다.

```javascript 

function openWithoutReferrer(url) {
    var aHref = document.createElement("a");
    aHref.href = url;
    aHref.rel = "noreferrer";
    aHref.click();
  };
```

참고: [hide-referrer](https://opensoul.org/2014/05/15/hide-referrer/)

## \<a\>

rel

- specifies the relationship of the target object to the link object.

- the value is: [link types](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types)
	- **noreferrer**: when navivation to another page, prevent the browser to send this page address as referrer via the `Referer` HTTP Header.  

참고: [MDN-a](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)