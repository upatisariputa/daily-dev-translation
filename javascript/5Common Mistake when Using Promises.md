JavaScript로 작업을 하다보면 비동기처리를 해야할 경우가 많다.
비동기 작업을 할때 흔히 하는 실수를 정리해 놓은 자료가 있어 한번 살펴보겠다.
[5Common Mistakes when Using Promises](https://blog.bitsrc.io/5-common-mistakes-in-using-promises-bfcc4d62657f)

Promise는 JavaScript에서 비동기 작업을 처리하는 방법을 제공한다.
콜백만을 사용하던때와 비교하여 훨씬 나은 비동기 솔루션이다.

### Promise 지옥을 피해라

일반적으로 Promise는 콜백지옥을 피하기 위해 사용된다. 그러나 Promise를 남용하면 Promise 지옥이 발생할 수 있다.

```
userLogin('user').then(function(user){
    getArticle(user).then(function(articles){
        showArticle(articles).then(function(){
            //Your code goes here...
        });
    });
});
```

위의 예에서 세개의 중첩된 Promise를 발견할수 있다.
`userLogin`, `getArticle`, `showArticle`
보다시피 복잡성은 코드 줄에 비례하여 증가하고 읽기 힘들다.

이것을 피하려면 getArticle을 첫번째 then과 두번째 then사이에서 처리해야 한다.

```
userLogin('user')
  .then(getArticle)
  .then(showArticle)
  .then(function(){
       //Your code goes here...
});
```

### Promise 안에서 try/catch를 사용

일반적으로 오류처리를 위해서 try/catch를 사용한다.
그러나 Promise 객체 안에서 try/catch를 사용하는것은 권장되지 않는다.

오류가 있는경우 Promise 객체가 catch범위에서 자동으로 오류를 처리하기 때문

```
new Promise((resolve, reject) => {
  try {
    const data = doThis();
    // do something
    resolve();
  } catch (e) {
    reject(e);
  }
})
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

Promise 자체에서 try/catch 없이 범위 내의 모든 오류를 포착한다.
실행중 발생한 모든 예외가 reject로 반환되어 나온다.

```
new Promise((resolve, reject) => {
  const data = doThis();
  // do something
  resolve()
})
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### Promise 블록 내에서 비동기 함수 사용

Async/Await은 다중의 Promise를 처리하기 위한 더 advanced한 구문이다.
함수 선언전에 async를 사용하면 Promise를 반환하고,
await은 Promise가 resolve되거나 reject 될때까지 기다린다.

**그러나 Async 함수를 Promise 블록 안에 넣으면 몇가지 부작용이 있음**

당신이 Promise블록안에 async작업을 수행하고 싶어서,
async를 추가하면 당신의 코드를 에러가 난다.
만일 catch() 블록을 사용하거나 try/catch 블록내에서 promise를 사용한다면,
당산은 이 에러를 즉시 처리할 수 없다.

```
// This code can't handle the error
new Promise(async () => {
  throw new Error('message');
}).catch(e => console.log(e.message));



(async () => {
  try {
    await new Promise(async () => {
      throw new Error('message');
    });
  } catch (e) {
    console.log(e.message);
  }
})();
```

Promise 블록내에서 async가 발생한경우, Promise 블록 밖으로 async 로직을 외부에 놓으려고 합니다.
그러나 경우에 따라 async 함수가 필요한 경우가 있습니다. 이 경우 try/catch 블록으로 수동으로 관리할 수 밖에 없다.

```
new Promise(async (resolve, reject) => {
  try {
    throw new Error('message');
  } catch (error) {
    reject(error);
  }
}).catch(e => console.log(e.message));


//using async/await
(async () => {
  try {
    await new Promise(async (resolve, reject) => {
      try {
        throw new Error('message');
      } catch (error) {
        reject(error);
      }
    });
  } catch (e) {
    console.log(e.message);
  }
})();
```

### Promise 생성 직후 Promise 블록 실행

아래 코드의 경우 HTTP 요청을 호출하는 코드 스니펫을 넣으면 즉시 실행된다.

```
const myPromise = new Promise(resolve => {
  // code to make HTTP request
  resolve(result);
});
```

그 이유는 코드 스니펫이 Promise 생성자에 래핑되어 있기 때문이다.
그러나 여러분들중 일부는 myPromise가 실행될때 then 메서드 이후에만 트리거 될 수 있다.

**그러나 그렇지 않다. Promise가 생성되면 콜백이 즉시 실행된다.**

즉, myPromise를 설정한 다음 줄에 도착할 때, HTTP 요청이 이미 실행중이거나, 최소 예약된 상태일 가능성이 높다.

그러나 Promise를 나중에 resolve 하려면 어떻게 해야 하는가?
HTTP 요청을 바로 처리하고 싶지 않다면?
그렇게 하도록 하는 Promise 마법 매커니즘이 있는가?

답은 개발자가 기대하는 것보다 명확하다.
함수는 시간-소비의 매커니즘이다.
개발자가 명시적으로 ()을 호출한 경우에만 함수가 실행 된다.

단순히 함수를 정의한 것으로는 아무것도 할 수 없다.
그래서, Promise를 지연 실행시키는 가장 좋은 방법은 이것을 함수로 둘러싸는 것이다.

```
const createMyPromise = () => new Promise(resolve => {
  // HTTP request
  resolve(result);
});
```

**HTTP 요청과 함꼐, Promise 생성자와 콜백함수는 오직 함수가 호출 될때만 실행 된다.
그리고 필요할때만 수행되는 지연 Promise가 있다.**

### Promise.all()을 불필요하게 사용하지 않음

서로 관련이 없는 Promise가 여러개 있는경우 모두 동시에 resolve할 수 있다.
\*\*Promise는 동시에 발생하지만, 한번에 하나씩 기다리면 오래걸린다.
당신은 Promise.all()을 사용하여 시간을 절약할 수 있다.

| Promise.all()은 당신의 친구이다!! ㅋㅋㅋ

```

const { promisify } = require('util');
const sleep = promisify(setTimeout);

async function f1() {
  await sleep(1000);
}

async function f2() {
  await sleep(2000);
}

async function f3() {
  await sleep(3000);
}


(async () => {
  console.time('sequential');
  await f1();
  await f2();
  await f3();
  console.timeEnd('sequential');
})();
```

Promise.all()을 사용하면 실행시간이 단축

```
(async () => {
    console.time('concurrent');
    await Promise.all([f1(), f2(), f3()]);
    console.timeEnd('concurrent');
  })();
```
