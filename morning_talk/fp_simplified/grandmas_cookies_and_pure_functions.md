# Grandma's Cookies(and Pure Functions)

앨빈아저씨가 대학생 새내기에 여자친구를 사귀었는데, 그 여자친구의 할머니께서 쿠키를 만들어서 주심.

이 쿠키가 앨빈아저씨 인생쿠키였고, 그 여자친구는 아내가 되었다. 아무튼 그때 그 쿠키를 잊지못해서 할머님께 레시피를 여쭤보고 그대로했는데 그 맛이 안났다. 

> I realized that I could decompile grandmother’s makeCookies recipe

그러다가 갑자기 레시피가 적힌 종이를 보고 scala로 decompile 해보는 이야기........;;

```scala
/**
 * Mix together some flour, butter, eggs, sugar,
 * and chocolate chips. Shape the dough into
 * little cookies, and bake them at 350 degrees
 * for 10 minutes.
 */
 
def makeCookies(ingredients: List[Ingredient]): Batch[Cookie] = { val cookieDough = mix(ingredients)
val betterCookieDough = combine(cookieDough, love)
val cookies = shapeIntoLittleCookies(betterCookieDough) bake(cookies, 350.DegreesFahrenheit, 10.Minutes)
}
```
여기까진 pure function 이었다. 

> What is love? Where does love come from?

그런데 갑자기 love를 어디다가 넣지!?!?!??! 혼돈에 빠진 앨빈아저씨.

## Love

> - love is a hidden input to the function.
> - love is a “free variable”

### hidden input 

function 을 아래와 같이 정의할 수 있다.

```
input => [function] => output
```

- input: front door
- output: back door

hidden input은 side door로 넣는것이다.

```
input => [function] => output
		hidden input
```

### free variable

- free variable 는 function 안에서 사용되는 변수인데,
- function의 parameter로 받지 않고, 
- function 안에서 정의하지도 않는다.

## Problems of the impure

그래서 makeCookies 는 이제 impure function이 되었다.

```
@impure
def makeCookies ...
```

love가 side door로 makeCookies에 들어오기 때문에, makeCookies를 호출할 때마다 다른 결과물을 얻을것이다.

- love의 상태는 makeCookies의 결과에 항상 영향을 주는데,
- makeCookies method signature를 봐서는 love가 영향을 주고있다는것을 알 방법이 없다!!

이것이 문제다.

이제 love에 대해 아래와 같이 생각해보자.

> - Does love have a default value?
> - How is love set before you call makeCookies?
> - What happens if love is not set?

이게 보통 impure function을 다룰 때 생각하게 되는 부분들이다. 이제 여기에 parallel/concurrent까지 고민하게되면 훠어어어얼씬 어려워질거다.

**Pure Function으로 짜면 이런걸 고민할 필요자체가 없다!!**

## Summary

> - love is a hidden input to _makeCookies_
> - _makeCookies_ output dose not depend on its declared inputs
> - You may get a different result every time you call _makeCookies_ with the same inputs
> - You can't read the _makeCookies_ signature to know its dependencies

이제 pure function 에 대한 얘기는 다했다. 다음시간 주제는 "What are the benefines of pure functions?"다. 다음시간에 만나요~~ 

