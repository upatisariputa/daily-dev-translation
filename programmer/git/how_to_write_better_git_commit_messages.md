보통 회사 또는 팀에서 정해진 커밋메시지 규칙이 있겠지만 초보 개발자나 프리랜서 개발자라면 커밋메시지 규칙이 없을 수가 있다. 여기에 좋은 글이 있어 커밋메시지에 대해서 잠시 알아보겠다.

[How to Write Better Git Commit Messages – A Step-By-Step Guide](https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages/)

### 왜 더 나은 커밋메시지를 작성해야 하는가

대다수의 개발자들은 빠르게 작업을 진행하거나 귀찮은 경우가 많기 때문에 커밋메시지에
"test", "test1"... 등등의 커밋메시지를 작성하게 된다.
이렇게 작성하다보면 몇일 또는 수개월 뒤 자신의 작업 내용을 보기위해 깃헙 등 저장소를 들어가보면 자신이나 팀원들이 어떤 내용을 작업하고 푸시했는지 모른채 그 코드의 변경내역을 뒤져보았던 기억이 있을것이다.

### 미래로

좋은 커밋을 작성하면 후에 생길수 있는 귀찮은 일에 대비할 수 있고 시간을 절약할 수 있습니다. 당장의 잠깐의 귀찮음을 피한다면 말이죠.
대규모 프로젝트에서는 문서화가 필수적입니다. 협업을 하는경우 커밋메시지는 더 중요할 수 있습니다.

## 더 나은 커밋 메시지를 작성하는 5단계 - 영어로 작성

1. 대문자 사용및 구두점: 첫 번째 단어를 대문자로 표시하고 구두점을 사용하지 않음.
2. 어조: 제목에 명령형 어조를 사용 - **Add fix for dark mode toggle state**
3. 커밋 타입: 커밋 타입을 지정, 변경 사항을 설명하는 일관된 단어 집합을 사용
4. 길이: 첫 번째 줄은 50자 이하, 본문은 72자 이하
5. 내용: 직접적으로 작성, 보충 단어와 구들을 제거(though, maybe 등)

### 기자처럼 작성하기

커밋 목적을 위해 커밋메시지에 대해 what과 why가 있어야함

- 내가 왜 이러한 변경을 했는가
- 변경 사항이 어떤 영향을 미쳤는지
- 왜 변화가 필요했는지
- 변경사항의 참고사항은 무엇인지

변경한 코드가 미래의 자신 또는 타인에게 명확하게 이해될 것이라고 기대하지 않는게 좋다.

두개의 차이점을 보자

1. git commit -m 'Add margin'
2. git commit -m 'Add margin to nav items to prevent them from overlapping the logo'
   위 두가지중 어떤것이 이 커밋을 보는 사람에게 유용할지는 너무 자명하다.

## Convential Commit

D2iQ에서는 다음과 같이 일관된 커밋 메시지 구조를 사용합니다.

```
<type> [optional scope]: <description>

[optional body]

[optional footer(s)]
```

type에는

- `feat` - 변경 사항과 함께 새로운 기능 도입
- `fix` - 버그 수정
- `chore` - 수정 또는 기능과 관련 없음, src 또는 테스트 파일을 수정하지 않는 변경사항
- `refactor` - 버그를 수정하거나 기능을 추가하지 않는 리팩토링된 코드
- `docs` - README 또는 기타 마크다운 파일과 같은 문서 업데이트
- `build` - 빌드 시스템 또는 외부 종속성에 영향을 주는 변경사항
- `revert` - 이전 커밋을 되돌림

제목에는 간결하게 설명하기 위해 문자의 제한이 있고 소문자로
본문에는 추가 세부 정보를 제공
바닥글(푸터)를 사용해 JIRA를 연결

전체 기존 커밋 예

```
fix: fix foo to enable bar

This fixes the broken behavior of the component by doing xyz.

BREAKING CHANGE
Before this fix foo wasn't enabled at all, behavior changes from <old> to <new>

Closes D2IQ-12345
```

팀내에서 커밋메시지 린팅을 구성할수 있다. [Commitizen](https://commitizen-tools.github.io/commitizen/) 커밋메시지 린팅을 할 수 있는 도구

### 커밋 메시지 비교

**Good**
feat: improve performance with lazy load implementation for images
chore: update npm dependency to latest version
Fix bug preventing users from submitting the subscribe form
Update incorrect client phone number within footer body per client request

**Bad**
fixed bug on landing page
Changed style
oops
I think I fixed it this time?
빈 커밋 메시지
