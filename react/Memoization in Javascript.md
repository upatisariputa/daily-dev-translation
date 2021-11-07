# Memoization in Javascript

[Memoization in JavaScript: A Guide](https://javascript.plainenglish.io/understanding-memoization-in-javascript-85182768045c)

### Memoization

메모제이션은 캐싱이다. 함수가 빠른 결과를 내기위해서는 같은 인풋일때는 캐싱해놓은 같은 아웃풋을 내주면 된다.

React는 useMemo, useCallback 이라는 함수를 사용해 memoization을 사용

### useMemo

```jsx
const result = useMemo(() => {
  return slowFunction(a);
}, [a]);
```

a에 따라 달라지는 slowFunction이란 함수를 memoize하기 위해 useMemo로 함수를 감싼다. a가 동일하게 유지되는 한 slowFunction은 실행되지 않고 cache된 값을 사용하게 된다. 이것은 useMemo가 가장 흔하게 사용되는 방식이나, 동등참조라는 케이스도 있다.

### 동등참조

기본적으로 두 값의 참조가 동일한지 여부를 정의, {}==={}는 false인데 동등참조로 체크하기 때문이다.

이 동등참조는 종속적 배열이 올때 중요해진다.

```jsx
function Component({ param1, param2 }) {
  const params = { param1, param2, param3: 5 };

  useEffect(() => {
    callApi(params);
  }, [params]);
}
```

useEffect가 잘 작동하는것 처럼 보이나, 매개 변수 객체가 새 객체로 만들어지기 때문에 실제로는 모든 렌더가 실행됨, 그러나 useMemo에서는 이 문제를 해결 가능하다.

```jsx
function Component({ param1, param2 }) {
  const params = useMemo(() => {
    return { param1, param2, param3: 5 };
  }, [param1, parma2]);
  useEffect(() => {
    callApi(parmas);
  }, [params]);
}
```

이 참조 동일성은 종속성 배열의 개체를 비교할때 유요하지만 종속성 배열의 함수를 사용해야 하는 경우 useCallbak 사용 가능

### useCallback

useCallback은 거의 useMemo에 기반하여 작동하지만, useCallback은 함수를 캐싱하는데 특화되어 있다.

```jsx
const handleReset = useCallback(() => {
  return doSomething(a, b);
}, [a, b]);
```

useMemo와 비슷하게 생겼지만, 실행될 때마다 useMemo는 함수를 호출하고 해당 함수를 리턴하지만 useCallback은 함수를 호출 하지 않고, 종속성이 변경 될때마다 전달된 함수의 새 버전을 반환, dependencies가 변경되지 않는 한 useCallback은 참조 동등성을 유지하는 이전과 동일한 함수 반환

```jsx
useCallback(() => {
	return a+b
}, [a,b])

useMemo(() => {
	return () => a+b
}, [a,b]
```

useCallback은 전달된 함수를 반환 한다. 반면 useMemo는 전달된 함수의 결과의 값을 반환 한다.

### 동등참조

```jsx
function Parent() {
  const [itmes, setItmes] = useState([]);
  const handleLoad = (res) => setItmes(res);
  return <Child onLoad={handleLoad} />;
}

function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad);
  }, [onLoad]);
}
```

handleLoad 함수는 Parent 컴포넌트가 랜더링 될때마다 다시 작성된다. 즉, Child Component의 useEffect는 onLoad가 참조 동일성이 다르다면 재 랜더 된다.

```jsx
function Parent() {
  const [items, setItems] = useState([]);
  const handleLoad = useCallback((res) => setItems(res), []);
  return <Child onLoad={handleLoad} />;
}
function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad);
  }, [onLoad]);
}
```

### React.memo

React.memo함수는 React.PureComponent와 비슷하게 행동한다. React.memo는 프롭스가 변경되기 전까지 리렌더 되지 않는다.

```jsx
React.memo(function Component(props){
	// Do something
}
```
