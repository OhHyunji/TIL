# react-router history

TIL

```
this.props.history.action 을 활용해서
- 이 페이지에 '뒤로가기'로 접근한 것인지
- 그냥 접근한것인지 알 수 있다.
```

## history.action

action: string

- PUSH, REPLACE, POP
	- 뒤로가기: POP
	- 바로 접근: PUSH
- `componentDidMount()`에서 뒤로가기로 들어온 경우에는 `fetch()`를 하지 않도록 하면 불필요한 request를 줄일 수 있다.

참고: [react-router history](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/history.md)