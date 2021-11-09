[Using the new JavaScript .at() method - LogRocket Blog](https://blog.logrocket.com/using-new-javascript-at-method/)

JavaScript **.at()** 메소드는 8월달에 ECMA International TC39에서 도입되었다.

JavaScript에서 요소를 선택하는것은 흔히 발생하는 작업이지만, .at() 메소드가 도입되기전에 배열(array)의 처음, 끝 또는 문자열에서 요소나 문자를 선택하는 기존의 메소드와 기술들이 있다.

대괄호 표기법 **[ ] 은 일반적으로 특정인덱스의 요소를 가져오는데 사용 되었음. 그러나 배열의 마지막 요소에 접근 하려고 음수를 사용하는 것은 불가능 했고 Python에서 주로 사용 되었다.**

결과적으로 개발자는 **slice()** 와 **length**를 사용하여 배열의 끝의 요소를 가져왔다.

### 인덱싱 가능한 객체 프로토 타입

.at() 메소드는 인덱싱객체의 프로토타입에(prototype of indexable object) 위치해있다.

인덱스 항목들을 공식화할 수 있는 이러한 개체에는 Array, String 또는 TypedArray과 같은 클래스들이 있다.

그러므로 .at() 메소드는 이러한 인덱싱 객체들에게 바로 사용 될 수 있다.

### 배열 요소를 가져오는 기존 방법

```jsx
const arr = [1, 2, "3", 4, 5, true, false];
```

대괄호 표기법을 사용하여 []에서 arr 배열의 특정 인덱스의 요소를 가져올수 있다. arr[0] 은 arr의 첫번째 요소를 반환 한다. 그러나 배열의 길이를 알수 없다면 length 프로퍼티 또는 slice() 메소드를 사용해야 한다.

### length 프로퍼티 사용

```jsx
arr[arr.length - 1]; // arr의 마지막을 요소를 가져옴
arr[arr.length - N]; // arr에서 N번째 뒤에 있는 요소를 가져옴

const arr = [1, 2, "3", 4, 5, true, false];
const lastItem = arr[arr.length - 1]; // flase
```

위의 작업으로 배열의 마지막 요소를 가져올수 있다. 그러나 불필요하게 코드가 길어지고, 함수의 리턴값으로 사용하기 위해서는 변수에 값을 저장해야 한다는 번거로움이 있다.

```jsx
function appendNumber(arr, N) {
  arr.push(N);
  return arr;
}

const tempArr = appendNumber([1, 2, "three", 4, 5, true, false], 6);
console.log(tempArr[tempArr.length - 1]); // Expected Output: 6
```

### Slice()

```jsx
arr.slice(-1)[0];
```

slice()와 음수를 사용하여 배열의 마지막 요소를 가져온다.

```jsx
const arr = [1, 2, "three", 4, 5, true, false];

console.log(arr.slice(-1)[0]); // Expected Output: false
```

length와 달리 slice()를 사용하면 함수의 리턴값을 강제로 저장하지 않아도 된다.

```jsx
function appendNumber(arr, N) {
  arr.push(N);
  return arr;
}

console.log(appendNumber([1, 2, "three", 4, 5, true, false], 6).slice(-1)[0]); // 6
// tempArr이 사라졌다.
```

### 왜 음수를 사용하여 배열의 인덱싱을 하지 않는가?(arr[-1])

간단히 생각하면 JavaScript에서 배열은 객체이다.

즉, 배열도 아래와 같이 생각할 수 있다.

```jsx
const arr = {
  0: 1,
  1: 2,
  3: "three",
  // ...
};

console.log(arr[0]); // Expected Output: 1
```

arr[-1]을 했을 경우 -1의 값은 없으므로 JavaScript는 undefined를 반환한다.

그러나 객체의 키로 -1을 지정 하면 키 -1에 대한 값이 반환된다.

```jsx
const arr = {
  "-1": "valid",
};

console.log(arr[-1]); // Expected Output: valid
```

### .at() 구문

.at()은 반환될 값의 인덱스를 받는다. 음수의 인덱스를 전달하면 배열 또는 문자열의 끝에서 계산해서 찾은 요소를 반환 하고 그렇지 않으면 undefined를 반환한다.

### .at() 사용법

```jsx
const arr = [1, 2, "three", 4, 5, true, false];

console.log(arr.at(0)); // Expected Output: 1
console.log(arr.at(2)); // Expected Output: "three"
console.log(arr.at(-1)); // Expected Output: false
console.log(arr.at(-3)); // Expected Output: 5
```

양수의 인덱스를 받을 경우 해당 인덱스를 반환 하고, 음수를 받을 경우 마지막부터 인덱싱해서 반환 한다.

문자열에서도 동일하다.

### 함수의 리턴값으로의 작업

length와는 달리 함수의 반환 값으로 변수에 저장하는 것을 강요하지 않는다.

```jsx
function appendNumber(arr, N) {
  arr.push(N);
  return arr;
}

console.log(appendNumber([1, 2, "three", 4, 5, true, false], 6).at(-1));
// Expected Output: 6
```

### .at() 메소드는 소수가 포함된 숫자도 인식

```jsx
const arr = [1, 2, "three", 4, 5, true, false];
console.log(arr.at(0.6)); // Expected Output: 1
console.log(arr.at(-3.6)); // Expected Output: 5
```

위와 같이 소수도 인식 하므로 소수점 이전의 값을 고려하여 해당 인덱스의 값을 반환한다.

### 지원

아직 많은 브라우저에서는 지원하지 않는 경우가 많으므로 JavaScript 단독으로 사용 시 주의 해야한다.
