# comment

팀원분과 주석에 대해 이야기를 나눴다.

- 주석은 최대한 안쓰고, 주석없이 코드가 읽히게 짜는것이 좋은것이겠지? (원래 나의 생각)
- 노노! 적당히 기준을 갖고 쓰면 좋다고 생각한다. (TIL)

## 함수 위에 description으로 쓴다.

```javascript
/*
* name을 가지고 user를 불러온다.
* param: name 찾을 user의 이름
*/
function getUserByName(name) {
	return users.find(user => user.name == name);
}
```

## body 안에 있는 주석은 별로다.

body 안에 주석이 있다는 것은

1. 함수명을 이상하게 작성했거나
2. 함수 위에있는 description이 충분하지 않다는 얘기.

## 코드는 간략하게.

함수 하나가 몇백줄? 노노!

- <클린코드>가 말하길, 함수는 5줄이어야 한다.
- if-else문이 딱 5줄 나온다.

무조건 함수는 5줄이어야해!! 이런건아니고,

- 나만의 기준을 만들자.
- 기준을 가지고 리팩토링해서, 코드를 다듬어나가자.
