## HTTP 요청 메서드
> HTTP는 **요청 메서드**를 정의하여, 주어진 리소스에 어떤 동작을 수행하고자 하는지 나타냅니다. 간혹 요청 메서드를 "HTTP 동사"라고 부르기도 합니다. 각각의 메서드는 서로 다른 의미를 가지지만, 일부 특성은 여러 메서드에서 공유되기도 합니다. 예를 들어 요청 메서드는 안전하거나, 캐시 가능하거나, 멱등적일 수 있습니다.
- **요청 메서드**: 클라이언트가 수행하고자 하는 동작
- **메서드가 필요한 이유**
  - 사용자가 `/users/10` 에 대해
    - 사용자 정보를 보고 싶은 건지
    - 사용자를 삭제하고 싶은 건지
    - 사용자 정보를 수정하고 싶은 건지
    - 새로운 정보를 넣고 싶은 건지
  - 알 수 없기에 메서드를 사용해서 요청의 의도를 표현한다.


> **GET**
> - GET 메서드는 **특정 리소스의 표현을 요청**합니다. GET을 사용하는 요청은 오직 **데이터를 받기만** 합니다.
- 리소스의 내용을 서버에 요청
    ```http
    GET /users/10 HTTP/1.1
    ```
    위 메시지의 의미는 "`/users/10`이라는 리소스를 가져오고 싶다." 이다.<br>
    서버는 처리한 다음 Response를 보낸다.
    ```http
    HTTP/1.1 200 OK
    Content-Type: application/json
    
    {
      "id": 10,
      "name": "영수"
    }
    ```

- **GET은 데이터를 받기만 한다**<br>
    받기만 한다 = 서버의 데이터를 변경하는 것을 목적으로 하지 않는다.
  - `GET /users/10` → 사용자 정보를 가져온다
  - `GET /products` → 상품 목록을 가져온다
  - `GET /posts/123` → 123번 게시글을 가져온다


- 보통 강의에서는 GET = 조회 라고 가르친다.
  - 유용한 암기법이지만 **원리를 이해하기에는 부족**하다.
  - 더 정확하게는 **GET은 특정 리소스의 표현(representation)을 요청하는 메서드다.**


- GET을 Spring Boot와 연결해서 보면
  ```Java
  @GetMapping("/users/{id}")
  public User getUser(@PathVariable Long id) {
      ...
  }
  ```
    - `@GetMapping` = **HTTP GET 요청을 이 Java 메서드와 연결하겠다.**
    - 클라이언트가 `GET /users/10` 을 보내면 Spring Boot가 해당 요청을 찾아서 `getUser(10)` 같은 Java 코드를 실행하게 된다.
    ```text
    HTTP
    GET
    ↓
    Spring Boot
    @GetMapping
    ↓
    Java 메서드
    ```



> **HEAD**
> - HEAD 메서드는 GET 메서드의 요청과 동일한 응답을 요구하지만, Response Body를 포함하지 않습니다.

- GET과 비슷하지만 Body를 받지 않는 메서드
- 서버에 `HEAD /image.jpg` 를 요청하면 이미지 자체를 받을 필요 없이 아래와 같은 헤더 정보를 확인할 수 있다.
  ```text
  파일이 존재하는가?
  크기가 얼마나 되는가?
  수정된 날짜가 언제인가?
  ```

> **POST**
> - POST 메서드는 특정 리소스에 데이터를 제출할 때 사용합니다. 이는 종종 **서버의 상태**의 변화나 **side effect를** 일으킵니다.

  - 서버에 데이터를 전달해서 어떤 처리를 해달라고 요청하는 것
  - 클라이언트가 메시지를 보내면
    ```text
    POST /users HTTP/1.1
    Content-Type: application/json
    
    {
      "name": "영수",
      "age": 25
    }
    ```
  - 서버는 이 데이터를 받아서 사용자를 생성할 수 있다.
    ```text
    클라이언트
       │
       │ POST /users
       │
       │ {"name":"영수","age":25}
       ▼
    서버
       │
       │ 사용자 생성
       ▼
    DB
    ```
  - **서버의 상태**
    - 서버의 상태의 변화: POST 요청으로 서버의 DB에 데이터가 들어와서 서버가 가지고 있던 데이터가 변경되는 것
  - **side effect**
    - 요청을 처리하면서 서버의 데이터나 외부 세계에 변화가 발생하는 것
    - `POST /orders` 를 보내면, 아래와 같은 일이 발생하는 것
    ```text
    주문이 DB에 저장
    재고 감소
    결제 처리
    이메일 발송
    ```
  - **POST 같은 요청은 서버에 실제 변화를 일으킬 수 있다.**
    
> **PUT**
> - PUT 메서드는 대상 리소스를 요청에 담긴 내용으로 전체 교체하는 데 사용합니다.

- 리소스의 내용을 새로운 내용으로 바꿔달라는 요청
  - 아래와 같은 사용자가 있을 경우 
  - ```JSON
    {
    "id": 10,
    "name": "영수",
    "age": 25
    }
    ```
  - 클라이언트가 다음과 같이 보내면
  - ```http
    PUT /users/10
    Content-Type: application/json
    
    {
    "name": "영수",
    "age": 26
    }
    ```
  - 10번 사용자 리소스를 내가 보낸 내용으로 전체 교체해달라는 의미가 된다.


- PUT과 POST 둘다 서버 데이터를 변경 할 수 있기에 헷갈릴 수 있다. 
  - 단순하게 비교하면
  - `POST` → 새로운 리소스를 만들 때 흔히 사용
  - `PUT`→ 특정 리소스를 지정해서 교체할 때 사용

> **DELETE**
> - DELETE 메서드는 특정 리소스를 삭제합니다.

- `DELETE /users/10` = `/users/10` 리소스 삭제 요청

> **CONNECT**
> - CONNECT 메서드는 대상 리소스로 지정된 서버와의 터널을 설정합니다.

- 서버와의 터널을 만드는 데 사용
  - 주로 프록시를 통한 터널링과 관련된다.

> **OPTIONS**
> - OPTIONS 메서드는 대상 리소스에 대해 사용할 수 있는 통신 옵션을 확인하는 데 사용합니다.

- 이 리소스에 어떤 통신/요청이 가능한지 확인
  - 웹 개발에서 CORS와 관련해서 만나게 될 수 있다.

> **TRACE**
> - TRACE 메서드는 요청이 서버까지 전달되는 과정에서 요청이 어떻게 변경되었는지 확인하기 위한 진단 목적의 메서드입니다.

> **PATCH**
> - PATCH 메서드는 **리소스의 일부를 수정**하는 데 쓰입니다.

- PUT이 리소스를 통째로 교체 하는 개념이라면
- PATCH는 리소스의 일부만 변경하는 개념이다.
  - 기존 사용자 : 
    ```JSON
    {
    "name": "영수",
    "age": 25,
    "email": "a@example.com"
    }
    ```
  - 사용자의 이름만 바꾸고 싶다면 : 
  - ```http
    PATCH /users/10
    Content-Type: application/json
  
    {
    "name": "철수"
    }
    ```
  - 처럼 일부만 변경하도록 요청할 수 있다.

---
- 메서드는 서버에서 실제로 어떤 Java 코드를 실행할지 자체를 결정하는 것은 아니다.
- HTTP가 `GET /users/10` 을 요청하면 HTTP 차원에서 `/users/10` 리소스를 GET 방식으로 요청한다.
  - Spring Boot가 그걸 받아서 `@GetMapping("/users/{id}")`에 연결하는 것이다.
  - ```text
    HTTP의 세계
    GET /users/10
          ↓
    Spring Boot의 세계
    @GetMapping("/users/{id}")
          ↓
    Java 메서드 실행
    ```
    
- **Spring이 HTTP Method를 만든 게 아니다.**
  - HTTP에 원래 GET, POST, PUT, DELETE 등의 Method가 있고, Spring이 그것을 Java 코드와 연결해주는 것이다.

**핵심**
```text
HTTP Request
│
├── Method
│      │
│      ├── GET
│      │     └── 리소스를 가져오고 싶다
│      │
│      ├── POST
│      │     └── 데이터를 제출해서 서버가 처리하도록 한다
│      │
│      ├── PUT
│      │     └── 리소스를 전달한 내용으로 교체한다
│      │
│      ├── PATCH
│      │     └── 리소스의 일부를 수정한다
│      │
│      └── DELETE
│            └── 리소스를 삭제한다
│
├── Path
├── Headers
└── Body
```
Spring Boot 에서는 다음과 같이 연결된다.
```text
GET     → @GetMapping
POST    → @PostMapping
PUT     → @PutMapping
PATCH   → @PatchMapping
DELETE  → @DeleteMapping
```