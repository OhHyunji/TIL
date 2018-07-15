# React Component Lifecycle

## 컴포넌트 초기 생성

컴포넌트가 브라우저에 나타나기 전

### constructor

```javascript
constructor(props) {
	super(props);
}
```

- 생성자. 컴포넌트가 새로 만들어질때마다 호출된다.

### <s>componentWillMount</s>

- 컴포넌트가 화면에 나타나기 직전에 호출된다.
- 기존에 serverSide에서 호출하는 용도로 사용됬었는데 React **v16.3에서 deprecated**되었다. v16.3 이후부터는 `UNSAFE_componentWillMount()`라는 이름으로 사용된다.
- 기존에 이 위치에서 처리하던 부분은 `constructor`나 `componentDidMount`에서 처리한다.

### componentDidMount

- 컴포넌트가 화면에 나타나면 호출된다.
- 여기서는 주로 
	1. D3처럼 DOM을 사용해야하는 외부 라이브러리 연동을 하거나, 
	2. fetch를 통해 ajax요청을 하거나,
	3. DOM 속성을 읽거나 변경하는 작업을 한다.

## 컴포넌트 업데이트

props의 변화, state의 변화 => 컴포넌트 업데이트

### <s>componentWillReceiverProps</s>

```javascript
componentWillReceiveProps(nextProps) {
  // this.props 는 아직 바뀌지 않은 상태
}
```

- 컴포넌트가 새로운 props를 받으면 호출된다.
- 여기서는 주로
	1. state가 props에 따라 변해야하는 로직을 작성한다.
	2. 새로받게될 props는 nextProps로 조회할 수 있다.
	3. 이 때 this.props는 prevProps 값이 들어있다.
- 이 API도 **v16.3에서 deprecated**된다. v16.3 이후부터는 `UNSAFE_componentWillReceiveProps()`라는 이름으로 사용된다.

### [new] static getDerivedStateFromProps

v16.3 이후에 새로 생겼다.

```javascript
static getDerivedStateFromProps(nextProps, prevState) {
	//setState가 아니라,
	//특정 props값이 바뀔 때 설정하고싶은 state값을 return한다.
	
	//Example1.
	if(nextProps.value !== prevState.value) {
		return {value: nextProps.value};
	}
	
	//Example2.
	return null;
	//==따로 업데이트 할 것 없음!	
}
```

- 여기서는 주로
	1. props로 받아온 값을 state로 동기화하는 작업을 처리한다.

### shouldComponentUpdate

```javascript
shouldComponentUpdate(nextProps, nextState) {
	//return true;		//업데이트 함. 
	//return false; 	//업데이트 안함.
}
```

- 이 API는 **컴포넌트를 최적화하는 작업**에서 매우 유용하게 사용된다.
	- 리액트는 변화가 발생하는 부분만 업데이트해서 성능이 잘나온다.
	- 변화가 발생하는 부분만 감지가히 위해서 virtual DOM에 한번 그린다.
	- 즉, 현재 컴포넌트의 상태가 업데이트되지 않아도 **부모 컴포넌트가 리렌더링 되면 자식컴포넌트들도 렌더링된다.** (렌더링된다. == `render()`함수가 호출된다.)
	- 이 함수는 기본적으로 true를 반환한다. 우리가 따로 작성해줘서 조건에 따라 false를 반환하면 => 해당 조건에서는 render 함수를 호출하지 않는다.
		- 리렌더링 할 필요 없는데 자동으로 리렌더링 되는것은 cpu자원 낭비하고있는 것이니, 렌더링이 굳이 불필요한 상황을 명시헤서 false를 반환한다.

### <s>componentWillUpdate</s>

- 이 API는 `shouldComponentUpdate`에서 true를 반환했을 때만 호출된다.
- 여기서는 주로
	1. 애니메이션 효과를 초기화하거나
	2. 이벤트 리스너를 없애는 작업을 한다.
- 이 함수가 호출되고난 다음에는 `render()`가 호출된다.
- 이 API도 **v16.3 이후 deprecated**된다.

### [new] getSnapshotBeforeUpdate()

- 이 API는 아래 시점에서 호출된다.
	1. render()
	2. **getSnapshotBeforeUpdate()**
	3. 실제 DOM에서 변화 발생
	4. componentDidUpdate
- 이 API를 통해서 DOM 변화가 일어나기 직전의 DOM상태를 가져오고,
- 여기서 return하는 값은 componentDidUpdate의 세번째 param으로 전해진다.

### componentDidUpdate

- 컴포넌트에서 render() 를 호출하고난 다음에 호출된다.
- 이 시점에서는 this.props, this.state가 바뀌어있다.
- params를 통해 이전값인 prevProps, prevState를 조회할 수 있다.
- snapshot은 세번째 param으로 받아온다.

```javascript
componentDidUpdate(prevProps, prevState, snapshot) {
	...
}
```

## 컴포넌트 제거

컴포넌트가 더이상 필요하지 않게 되면 호출된다.

### componentWillUnmount()

- 여기서는 주로
	1. 등록했던 이벤트를 제거하고
	2. setTimeout을 걸었다면 clearTimeout으로 제거한다.
	3. 이밖에 사용한 외부라이브러리 중 dispose 기능이 있다면 여기서 호출한다.

## 컴포넌트 에러발생

render()에서 에러가 발생하면 리액트 앱이 크래시난다. 그런 상황에 아래 API를 유용하게 사용할 수 있다.

### componentDidCatchㅎ
```
componentDidCatch(error, info) {
	this.setState({
		error: true
	});
}
```
	
- 에러가 발생시 componentDidCatch API를 활용해서 render에서 this.state.error 값에 따라 에러페이지를 띄울 수 있다.
- 주의: 
	- 컴포넌트 자신의 render()함수에서 발생하는 에러는 잡을 수 없지만, 
	- 자식 컴포넌트의 내부에서 발생하는 에러들은 잡일 수 있다.
- 보통 렌더링 부분에서 오류가 발생하는 것은 사전에 방어해야한다.

```
//존재하지 않는 함수를 호출하려고 할 때
this.props.onClick();

//배열이나 객체가 올 줄 알았는데 존재하지 않을 때
this.props.object.value
this.props.array.length

//이런 것들은 render함수에서 아래와 같이 방어할 수 있다.
render() {
	if(!this.props.object || !this.props.array || this.props.array.length===0) return null;
	//object, array를 사용하는 코드
}
```

참고: [react life cycle]()