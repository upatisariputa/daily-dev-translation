# 13 reasons why you should use Nextjs

이전에 회사의 렌딩페이지를 next.js로 작업을 한 적이 있었다.
SEO 때문에 next.js로 작업을 해 보았지만 react에 비해 여러가지 이점이 있었다.
물론 React가 아닌 Next.js로 작업해야 해서 SSR과 같은 작업은 어색하긴 했었다.
그 이점들을 잘 정리해 놓은 블로그가 있으므로 간단히 알아 보았다.

[13 reasons why you should use Nextjs](https://dev.to/thatanjan/13-reasons-why-you-should-use-nextjs-2d40)

- SEO: Next.js 를 사용하면 서버렌더링을 사용하기 때문에 검색어 최적화를 할 수 있다.
- Server Side Rendering : Client Side Rendering은 큰 번들 크기, SEO, 초기 로드 지연과 같은 문제가 발생할 수 있으나 Next.js는 Server Side Rendering이다.
- Static Site Generation : 정적사이트 생성, 블로그와 같은 배포후 콘텐츠 변경이 거의 필요가 없다면 Next.js는 빌드시 정적 html을 생성할 수 있다.
- Client Side Rendering: Next.js는 Server Side Rendering 뿐만아니라 Client Side Rendering도 수행 가능 하다
- 이미지 최적화 : Next.js Image 컴포넌트를 사용하면 화질을 좋게 유지하면서 이미지 크기를 작게, Lazy Loading, 반응형 이미지 크기들을 쉽게 사용 가능
- 내장 라우팅: 라우터를 사용하기 위하여 외부 패키지를 설치 할 필요가 없음
- 내장 CSS 지원 : CSS파일을 직접 가져오기, node_modules 디렉토리에서 스타일 가져오기 등을 사용 할 수 있음
- API Routes: Next.js에는 API routes라는 기능이 있다. api 디렉토리에 작성해서 간단한 api 작업을 수행 할 수 있다.
- 국제화 라우팅 : Next.js는 국제화 라우팅(i18n)을 기본으로 내제하고 있다. 각 국가에 대해 언어를 제공 가능 하다.
- 코드분할 : Next.js는 기본적으로 코드를 별도의 번들로 분할 하여 속도를 향상 시킴
- Static Assets : 정적 파일을 저장할 수 있는 디렉토리가 있음
- File System Routing: 모든 페이지 파일을 저장하는 페이지 디렉토리가 있다.
- 고도의 구성 가능 : webpack을 알고 있다면 next.js를 단일 config 파일로 app을 구성할 수 있다.
