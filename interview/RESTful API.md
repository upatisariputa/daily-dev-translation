[REST API 제대로 알고 사용하기 : NHN Cloud Meetup](https://meetup.toast.com/posts/92)

## REST란?

REST는 Representational State Transfer라는 용어의 약자. 웹의 장점을 살릴수 있는 아키텍처

## REST 구성

- 자원(Resource) - URI
- 행위(Verb) - HTTP METHOD
- 표현(Representations)

## REST API 디자인 가이드

1. URI는 정보의 자원을 표현
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현

### REST API 중심 규칙

1. URI는 정보의 자원을 표현해야 한다.

```
GET /members/delete/1 => 제대로 적용 안됨 GET과 DELETE가 혼용
```

1. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현

```
DELETE /members/1
```

| POST   | URI를 요청하면 리소스를 생성                                               |
| ------ | -------------------------------------------------------------------------- |
| GET    | 리소스를 조회, 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져옴 |
| PUT    | 리소스 수정                                                                |
| DELETE | 리소스 삭제                                                                |

### URI 설계시 주의점

1. 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용

```
http://restapi.example.com/animals/mammals/whales
```

1. 마지막 문자로 슬래시 포함하지 않음

```
http://restapi.example.com/animals/insects/bees/  (x)
```

1. 하이픈(-)은 URI 가독성 높이는데 사용
2. 밑줄(\_)은 지양
3. URI 경로에는 소문자로 - 대문자에 따라 다른 리소스로 인식 되기 때문
4. 파일 확장자는 URI에 포함시키지 않음

```
http://restapi.example.com/members/soccer/345/photo.jpg (X)
```

### 리소스 간의 관계를 표현하는 방법

```
/리소스명/리소스 ID/관계가 있는 다른 리소스명

GET : /users/{userid}/devices
```

### 자원을 표현하는 Collection과 Document

```
http:// restapi.example.com/sports/soccer

sports라는 컬렉션과 soccor라는 도큐먼트로 표현
```

```
http:// restapi.example.com/sports/soccer/players/13

sports, players 컬렉션과 soccer, 13을 의미하는 도큐먼트
```

일반적으로 컬렉션은 복수형, 도큐먼트는 단수형으로 표현한다.

## HTTP 응답 상태 코드

| 200                     | 클라이언트의 요청을 정상적으로 수행                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| 201                     | 클라이언트가 어떠한 리소스를 생성을 요청, 해당 리소스가 성공적으로 생성됨 (POST를 통한 리소스 생성 작업시) |
| 400                     | 클라이언트의 요청이 부적절 할 경                                                                           |
| 401                     | 클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을 때                                            |
| (로그인 하지 않았을 때) |
| 403                     | 유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때                             |
| 405                     | 클라이언트가 요청한 리소스에서는 사용 불가능한 메소드를 이용했을 경우                                      |
| 301                     | 클라이언트가 요청한 리소스에 대한 URI가 변경 되었을 때 사용하는 응답코드                                   |
| 500                     | 서버에 문제가 있을 경우                                                                                    |
