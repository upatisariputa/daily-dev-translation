React로 작업을 하다보면 어떤 폴더구조로 작업을 하는게 효율적인지 의문을 가질때가 있다.
hook을 사용하는 사람이 있을것이고 Redux를 사용하는 사람도 있을것이다.
본인이 React를 사용함에 있어 어떤 방식으로 폴더구조를 구성하든지
처음에 폴더구조를 잘 구성해 놓아야 후에 작업 능률 하락 없이 작업 진행이 원할할 것이다.
이 블로그에서는 아키텍처 패턴에 대해서 설명해 놓았지만 본인만의 스타일이 있기 때문에 참고만 하는것이 좋을 것이다.
[React Architecture Patterns for Your Projects](https://blog.openreplay.com/react-architecture-patterns-for-your-projects)

React는 프론트엔드 환경에서 주장이 없는(un-opinionated) 프레임워크이다.
React는 어플리케이션을 구성하고 구조하는 방법을 제공하지 않는다.
어플리케이션을 구성하고 구조하는 것은 개발자나 팀이 상호작용하는 방식이다.
잘 짜여진 프로젝트는 개발자들이 자신의 구현에 더 깊이 생각하고 정리하도록 요구한다.
또한 잘 짜여진 프로젝트는 코드베이스를 쉽게 탐색, 수정 및 확장할 수 있다.

조직화된 코드베이스는 팀이 장기적으로 주어진 구조내에서 생산성을 향상시킬수 있다.

### 디렉토리 구조 탐색

항상 React 어플리케이션의 시작점(Starting point)를 가지는게 좋다.
이 시작점은 assets, components, context, services, hooks, pages, utils 등 다양한 폴더로 구성된다.
이들 폴더 각각은 디렉토리의 목적을 달성하기 위해 다양한 파일이 있다.

create-react-app을 사용하여 React 어플리케이션을 만들었을때 index.js, App.js가 기본적으로 src 폴더 내에 구성된다.
아래는 기본적인 폴더 구성 예이다.

```
└── /src
    ├── /assets
    ├── /components
    ├── /context
    ├── /hooks
    ├── /pages
    ├── /services
    ├── /utils
    └── App.js
    ├── index.js

```

여기서 디렉토리 이름을 코드베이스에있는 누구라도 이해할 수 있는 깔끔한 규칙을 따르는게 좋다.
/services 디렉토리는 /api로 대체 될수 있다.

### 공통 모듈

React의 한가지 이점은 모듈을 어떻게 나누든 상관이 없다.
React 앱에서 페이지를 개발할때는 모듈 형태로 분할 하는것을 고려하는것이 좋다.
이렇게 하면 복잡성을 줄이고 앱 전체에 걸쳐 재사용하거나 공유할 수 있는 구조를 만들 수 있다.

React 앱의 공유 가능한 코드는 자체 도메인으로 구분해야 한다.
공통 모듈은 재사용 가능한 `커스텀 component`, `커스텀 hook`, `비지니스 로직`, `상수(Constants)` 그리고 `유틸리티 함수` 등이다.
이러한 재사용가능한 것들은 둘 이상의 페이지, 컴포넌트에서 사용하기 위해 앱 내에서 공유된다.

### 자체 폴더에 사용자 지정 구성 요소 추가

재사용 가능한 컴포넌트의 예로는 버튼, 입력 필드 및 카드 등이 있다.
이러한 컴포넌트는 /Components의 서브 디렉토리에 구성된다.

```
└── /src
    ├── /components
    |   ├── /Button
    |   |   ├── Button.js
    |   |   ├── Button.styles.js
    |   |   ├── Button.test.js
    |   |   ├── Button.stories.js
    |   ├──index.js
```

버튼 컴포넌트의 예로 위와 같이 작성할 수 있다.

그리고 컴포넌트 index.js에는 아래와 같이 작성 가능하다.

```javascript
// /src/components/index.js
import { Button } from "./Button/Button";
import { InputField } from "./InputField/InputField";
import { Card } from "./Card/Card";

export { Button, Card, InputField };
```

### 커스텀 훅 만들기

재사용 가능한 React hook은 재사용 가능한 작업 부분과 같다.
커스텀 훅을 만드는것은 코드 복잡성을 줄이는데 도움이 될 수 있다.

예를 들어 로그인과 가입에 사용하는 비밀번호 가시성을 전환할 수 있는 로직을 만든다면
두개의 페이지에 동일한 로직이 들어가게 된다.
이럴경우 커스텀 훅을 만들어서 사용하는것이 코드 복잡성을 줄일수 있다.

```javascript
// /src/hooks/useTogglePasswordVisibility/index.js
export const useTogglePasswordVisibility = () => {
  const [passwordVisibility, setPasswordVisibility] = useState(true);
  const [rightIcon, setRightIcon] = useState("eye");
  const handlePasswordVisibility = () => {
    if (rightIcon === "eye") {
      setRightIcon("eye-off");
      setPasswordVisibility(!passwordVisibility);
    } else if (rightIcon === "eye-off") {
      setRightIcon("eye");
      setPasswordVisibility(!passwordVisibility);
    }
  };
  return {
    passwordVisibility,
    rightIcon,
    handlePasswordVisibility,
  };
};
```

커스텀 훅은 use를 사용한 네이밍 규칙이 있다.
이 네이밍 규칙은 엄격하지는 않지만 규칙을 따르는 것이 좋다.

```
└── /src
    ├── /hooks
    |   ├── /useTogglePasswordVisibility
    |   |   ├── index.js
    |   |   ├── useTogglePasswordVisibility.test.js
    |   ├──index.js
```

### 절대 경로 사용

jsonconfig.json 파일에 complierOption과 webpack.config.js 를 사용하여

```javascript
// some file
import { Button } from "../../components";
```

```javascript
import { Button } from "components";
```

```javascript
import { Button } from "@components";
```

`../` 를 사용하지 않고 깔끔하게 코드를 작성할 수 있다.

### UI와 비지니스 로직 분리

/page 디렉토리는 앱에 대한 UI와 React Component의 구성으로 페이지를 구성할 것이다.
당연히 UI와 로직이 결합 될수 밖에 없지만 이것을 분리한다면 더욱 깔끔한 코드를 작성할 수 있을 것이다.

UI와 로직을 분리하는 방법은 API 요청 하는 등의 커스텀 훅을 제작하는 것이다.

```javascript
import { useQuery } from "react-query";
export const moviesApi = {
  nowPlaying: () =>
    fetch(`${BASE_URL}/movie/now_playing?api_key=${API_KEY}&page=1`).then(
      (res) => res.json()
    ),
};
export const useNowPlayingMovies = () => {
  const { isLoading: nowPlayingLoading, data: nowPlayingData } = useQuery(
    ["movies", "nowPlaying"],
    moviesApi.nowPlaying
  );
  return {
    nowPlayingLoading,
    nowPlayingData,
  };
};
```

위와 같이 API 호출과 로직을 따로 만들어 페이지나 컴포넌트에서 재사용 하는 것이 좋다.

### Utils 디렉토리

Utils 디렉토리는 완전히 선택사항이다.
앱에 일부 글로벌 유틸리티 기능이 있는경우 UI 컴포넌트에서 분리하는것이 좋다.
Utils 디렉토리에는 앱 전체의 상수 등이 포함될 수 있다.
