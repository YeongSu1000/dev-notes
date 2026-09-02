## JSON으로 작업하기

> JavaScript Object Notation (JSON)은 JavaScript 객체 문법을 기반으로 구조화된 데이터를 표현하는 문자 기반의 표준 형식입니다. 웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용합니다(서버에서 클라이언트로 데이터를 전송하거나, 반대로 클라이언트에서 서버로 데이터를 전송할 때). 여기저기서 자주 보았을테니 여기선 JSON을 파싱, 데이터에 접근하고 JSON을 생성하는 등 Javascript로 JSON을 다루는 법에 대해 알아봅시다.
 
- **JSON**: 데이터를 구조적으로 표현하는 문자 기반의 표준 포맷
  - HTTP: 데이터를 주고받는 통신 규칙
  - JSON: 주고받을 데이터를 표현하는 형식
  - "JSON으로 통신한다" = JSON 형식의 데이터를 주고받는다. (HTTP를 사용한다면 HTTP Request/Response의 Body에 JSON을 담아 주고받을 수 있다.)

```text
HTTP
│
├─ Request
│    └─ Body에 JSON을 넣을 수 있음
│
└─ Response
     └─ Body에 JSON을 넣을 수 있음
```

---
## 아니, 대체 JSON이 뭐죠?
> JSON은 Douglas Crockford가 널리 퍼뜨린 **Javascript 객체 문법을 따르는 문자 기반의 데이터 포맷**입니다. JSON이 Javascript 객체 문법과 매우 유사하지만 딱히 Javascript가 아니더라도 JSON을 읽고 쓸 수 있는 기능이 다수의 프로그래밍 환경에서 제공됩니다.
- JSON은 이런 모습이다
    ```JSON
    {
      "name": "영수",
      "age": 20
    }
    ```
  - JavaScript 객체와 굉장히 비슷하게 생겼다.
- JavaScript에서는
    ```JavaScript
    const user = {
      name: "영수",
      age: 20
    };
    ```
  - 그래서 이름도 JavaScript Object Notation이고 모양도 JavaScript 객체와 비슷하다.
- 하지만 **JSON ≠ JavaScript** 객체
  - JSON은 **데이터를 표현하는 형식**
  - JavaScript 객체는 **JavaScript 프로그램 안에서 실제로 사용하는 객체** 

> JSON은 **문자열 형태**로 존재합니다 — 네트워크를 통해 전송할 때 아주 유용하죠. **데이터에 접근하기 위해서는 JSON 문자열을 프로그램에서 사용할 수 있는 객체나 데이터 구조로 변환해야 합니다**. JavaScript는 JSON 전역 객체의 JSON.parse()와 JSON.stringify()를 통해 JSON 문자열과 JavaScript 객체를 서로 변환할 수 있습니다.
- **문자열 형태**
    ```JSON
    {
      "name": "영수",
      "age": 20
    }
    ```
  - 이것은 사람 눈에는 그냥 JSON 데이터처럼 보인다.
  - 네트워크를 통해 전송되는 관점에서는 이것은 결국 문자 데이터다.
  - HTTP Request 에서
    ```http
    POST /users
    Content-Type: application/json
    
    {
    "name": "영수",
    "age": 20
    }
    ```
    이 Body에 들어있는 것은 JavaScript 객체가 아니다. 클라이언트에 있는 JavaScript 객체를 그대로 네트워크 선으로 보내는 게 아니다. JSON 형식의 **텍스트로 표현해서** 보낸다.


> 참고 : **문자열에서 네이티브 객체로 변환하는 것은 파싱**(Parsing)이라고 합니다. 네트워크를 통해 전달할 수 있게 객체를 문자열로 변환하는 과정은 **문자열화**(Stringification)이라고 합니다.
- 서버에서 JSON 문자열을 받았을 때
    ```JSON
    {
      "name": "영수",
      "age": 20
    }
    ```
  프로그램이 이를 그냥 문자로 가지고 있는 것과 <br>
    `"{ "name": "영수", "age": 20 }"`<br><br>  
    
  프로그램이 실제 데이터 구조로 해석해서
    ```text
    name → 영수
    age → 20
    ```
  이처럼 사용하는 것은 다르다.
  - 그래서 JSON 문자열을 프로그램이 사용할 수 있는 객체/데이터 구조로 해석하는 과정이 필요하다. 이것이 파싱이다.
  - JSON 문자열 안에 어떤 데이터가 들어있는지 분석해서 프로그램이 사용할 수 있게 만드는 것

- **반대 방향도 있다. 문자열화**
  - JavaScript 프로그램에 객체가 있을때
    ```javascript
    const user = {
        name: "영수",
        age: 20
    };
    ``` 
    이것을 서버로 보내고 싶으면 네트워크를 통해 전달할 JSON 형식으로 만들어야 한다. `JavaScript 객체 → JSON 문자열`
  - 이 과정을 문자열화(Stringification) 라고 한다.
  ```text
  파싱
  JSON 문자열 ─────────→ 프로그램 객체
   
  문자열화
  프로그램 객체 ───────→ JSON 문자열
  ```
> JSON 데이터를 .json 확장자를 가진 단순 텍스트 파일에 저장할 수 있습니다. MIME 타입은 application/json 입니다.

- HTTP와의 전체 흐름: 
    ```
    ┌───────── 클라이언트 ────────────┐
    │                               │
    │ JavaScript 객체                │
    │       ↓                       │
    │ JSON.stringify()              │
    │       ↓                       │
    │ JSON 문자열                    │
    └──────────────┬────────────────┘
                   │
              HTTP Request
                   │
                   ↓
                 서버
                   │
              JSON 문자열
                   ↓
              Java 객체
                   │
              서버의 처리
                   │
              Java 객체
                   ↓
              JSON 문자열
                   │
              HTTP Response
                   ↓
    ┌──────────────┴────────────────┐
    │ 클라이언트                      │
    │       ↓                       │
    │ JSON.parse()                  │
    │       ↓                       │
    │ JavaScript 객체                │
    └───────────────────────────────┘
    ```
  
> ### JSON 구조
> 위에서 설명했듯이 JSON은 JavaScript 객체 리터럴 문법과 유사한 구조를 가진 텍스트 기반의 데이터 형식입니다. JSON에는 JSON에는 **문자열, 숫자, 배열, 객체, 불리언, null** 등의 값을 포함할 수 있습니다. 이런 방식으로 여러분은 **데이터 계층**을 구축할 수 있습니다, 아래 처럼요.
>```JSON
> {
>  "squadName": "Super hero squad",
>  "homeTown": "Metro City",
>  "formed": 2016,
>  "secretBase": "Super tower",
>  "active": true,
>  "members": [
>    {
>      "name": "Molecule Man",
>      "age": 29,
>      "secretIdentity": "Dan Jukes",
>      "powers": [
>        "Radiation resistance",
>        "Turning tiny",
>        "Radiation blast"
>      ]
>    },
>    {
>      "name": "Madame Uppercut",
>      "age": 39,
>      "secretIdentity": "Jane Wilson",
>      "powers": [
>        "Million tonne punch",
>        "Damage resistance",
>        "Superhuman reflexes"
>      ]
>    },
>    {
>      "name": "Eternal Flame",
>      "age": 1000000,
>      "secretIdentity": "Unknown",
>      "powers": [
>        "Immortality",
>        "Heat Immunity",
>        "Inferno",
>        "Teleportation",
>        "Interdimensional travel"
>      ]
>    }
>  ]
>}
>```
> 이 객체를 JavaScript 프로그램에서 로드하고 파싱하여 superHeroes라는 이름의 변수에 저장하면 JavaScript object basics 문서에서 보았던 것처럼 점/브라켓 표현법을 통해 객체 내 데이터에 접근할 수 있게 됩니다. 아래와 같이요:
> ```javascript
> superHeroes.homeTown;
> superHeroes["active"];
> ```
> 하위 계층의 데이터에 접근하려면, 간단하게 프로퍼티 이름과 배열 인덱스의 체인을 통해 접근하면 됩니다. 예를 들어 superHeroes의 두 번째 member의 세 번째 power에 접근하려면 아래와 같이 하면 됩니다.
> ```javascript
> superHeroes["members"][1]["powers"][2];
> ```
> 1. 우선 변수 이름은 — superHeroes입니다.
> 2. members 프로퍼티에 접근하려면, ["members"]를 입력합니다.
> 3. members는 객체로 구성된 배열입니다. 두 번째 객체에 접근할 것이므로 [1]를 입력합니다.
> 4. 이 객체에서 powers 프로퍼티에 접근하려면 ["powers"]를 입력합니다.
> 5. powers 프로퍼티 안에는 위에서 선택한 hero의 superpower들이 있습니다. 세 번째 것을 선택해야 하므로 [2].

- JSON은 여러 종류의 데이터를 담을 수 있다.
  - 문자열, 숫자, 배열, 불리언, 다른 객체를 포함할 수 있다.
    ```text
    "squadName"   → 문자열
    "formed"    → 숫자
    "active" → 불리언
    ```
  - 객체 안에 또 객체를 넣거나, 배열 안에 객체를 넣는 것도 가능하다.


- 가장 바깥쪽 `{}`
  ```JSON
  {
    "squadName": "Super hero squad",
    "homeTown": "Metro City",
    "formed": 2016,
    "active": true,
    "members": [...]
  }
  ```
  `{}`로 둘러싸여 있다. 이것은 **객체**(object)이고, 객체 안에는 **프로퍼티 이름: 값** 형태로 데이터가 들어간다.

  전체적으로 다음과 같은 구조다.
  ```text
  객체
  ├── squadName
  ├── homeTown
  ├── formed
  ├── active
  └── members
  ```

- members
  ```JSON
  "members": [
  {
  "name": "Molecule Man",
  "age": 29
  },
  {
  "name": "Madame Uppercut",
  "age": 39
  }
  ]
  ```
  `members`의 값이 배열이라는 것이 중요하다. 배열은`[]`로 표현한다. 따라서 `"members": [...]`를 보면 `members`프로퍼티의 값으로 배열이 들어있다고 읽으면 된다.

- 배열 안에 객체가 있다.
  ```JSON
  [
    {
      "name": "Molecule Man",
      "age": 29
    },
    {
      "name": "Madame Uppercut",
      "age": 39
    }
  ]
  ```
  배열 안을 보면 `[]`안에 `{}`가 있다.
  - members는 객체 여러 개를 담고 있는 배열이다.
  - 첫번째 객체는
    ```JSON
    {
      "name": "Molecule Man",
      "age": 29
    }
    ```
  - 두번째 객체는
    ```JSON
    {
      "name": "Madame Uppercut",
      "age": 39
    }
      ```
    이런 식으로 **데이터 안에 데이터가 들어가는 구조**를 만들 수 있다.
- powers
  ```JSON
  {
    "name": "Molecule Man",
    "age": 29,
    "powers": [
      "Radiation resistance",
      "Turning tiny",
      "Radiation blast"
    ]
  }
  ```
  첫 번째 멤버를 보면
  ```text
  powers
    ↓
  배열
    ↓
  문자열
  문자열
  문자열
  ```
  이다. 즉 `powers`는 문자열들을 담은 배열이다.


- 전체 구조는 대략 이렇게 생겼다.
  ```text
  객체
  │
  ├─ squadName → 문자열
  ├─ homeTown  → 문자열
  ├─ formed    → 숫자
  ├─ active    → 불리언
  │
  └─ members → 배열
               │
               ├─ 객체
               │   ├─ name → 문자열
               │   ├─ age → 숫자
               │   └─ powers → 배열
               │                 ├─ 문자열
               │                 ├─ 문자열
               │                 └─ 문자열
               │
               └─ 객체
                   ├─ name → 문자열
                   ├─ age → 숫자
                   └─ powers → 배열
  ```
  
- 이것이 위의 `이런 방식으로 여러분은 데이터 계층을 구축할 수 있습니다.` 문장에서 말하는 "데이터 계층"이다.
- 계층은 객체 안에 배열이 있고, 배열 안에 객체가 있고, 그 객체 안에 다시 배열이 있는 식의 중첩 구조


- **JSON을 JavaScript 프로그램에서 로드하고 파싱**
  - `const superHeroes = /* JSON을 파싱한 결과*/;`
  - 그러면 `superHeros`는 JavaScript에서 사용할 수 있는 객체가 된다.
  - 그래서 `superHeros.homeTown;` 이라고 하면 `superHeroes` → `homeTown` → `Metro City`를 얻는다.
  - 또는 `superHeroes["active"]` → `true`다.
- JavaScript 객체에 접근하는 두 가지 방법
  - 점 표기법
    - `superHeroes.homeTown`
  - 브라켓 표기법
    - `superHeroes["homeTown"]`
  - 둘 다 기본적으로 같은 프로퍼티를 가리킨다.
    - `superHeroes` → `homeTown` → `Metro City`
    
- ```javascript
  superHeroes["members"][1]["powers"][2];
  ```
  ```text
  superHeroes
  ↓
  members
  ↓
  [1] 두 번째 멤버
  ↓
  powers
  ↓
  [2] 세 번째 능력
  ↓
  "Superhuman reflexes"
  ```
  
> ### JSON에서의 배열
> 앞서 JSON 텍스트는 기본적으로 JavaScript의 오브젝트와 비슷하게 생겼다고 언급하였습니다. 그리고 그것은 대부분 맞습니다. "대부분 맞다"라고 말한 이유는 JavaScript의 배열 또한 JSON에서 유효하기 때문입니다.
>```JSON
> [
>   {
>     "name": "Molecule Man",
>      "age": 29,
>      "secretIdentity": "Dan Jukes",
>      "powers": [
>         "Radiation resistance", 
>         "Turning tiny", 
>         "Radiation blast"
>       ]
>    },
>   {
>      "name": "Madame Uppercut",
>      "age": 39,
>      "secretIdentity": "Jane Wilson",
>      "powers": [
>         "Million tonne punch",
>         "Damage resistance",
>         "Superhuman reflexes"
>      ]
>    }
> ]
>```
> 위 예제는 완벽히 올바른 형태의 JSON입니다. **JSON을 파싱한 결과인 배열의 요소에 접근할 때 배열 인덱스를 사용**하면 됩니다. [0]["powers"][0] 와 같이 말이죠.

- JSON은 `{}`로 시작하지 않아도 된다.
  - 위 JSON은 맨 바깥이 `{}`가 아니라 `[]`이지만 정상적인 JSON이다.

- 구조를 살펴보면
  ```text
  배열
  ├── 객체 1
  │   ├── name
  │   ├── age
  │   ├── secretIdentity
  │   └── powers
  │
  └── 객체 2
      ├── name
      ├── age
      ├── secretIdentity
      └── powers
  ```
  - 멤버 객체 두 개를 가지고 있는 배열이다.


- 배열과 객체를 구분
  - `{}`와 `[]`를 보고 역할을 구분한다.

- REST API에서 사용
  - 서버가 사용자 목록을 JSON으로 보내준다고 할때
    ```text
    [
      {
        "id": 1,
        "name": "영수"
      },
      {
        "id": 2,
        "name": "일수"
      },
      {
        "id": 3,
        "name": "이수"
      }
    ]
    ```
  - `GET /users`를 보내면 서버가
    ```http
    HTTP/1.1 200 OK
    Content-Type: application/json
    ```
    과 함께
    ```JSON
    [
      {
        "id": 1,
        "name": "영수"
      },
      {
        "id": 2,
        "name": "철수"
      }
    ]
    ```
    같은 JSON을 응답할 수 있다.
  - 그러면 클라이언트는 이 JSON을 파싱해서 배열로 사용하고, 그 안의 객체에 접근하게 된다.
  - 지금 보는 JSON 배열/객체 구조가 나중에 실제 REST API 응답을 읽는 방법으로 연결된다.


- JSON을 파싱한 결과인 배열의 요소에 접근할 때 배열 인덱스를 사용
  - **파싱된 버전**
  - `[0]["powers"][0]` 처럼 접근하는 것은 JSON 문자열 자체에 접근하는 것이 아니라, JSON을 파싱해서 만들어진 JavaScript 배열/객체에 접근하는 것

> ### Other notes
> - **JSON은 데이터 포맷**(데이터를 표현하기 위한 포맷)입니다. 메서드는 담을 수 없습니다.
> - JSON은 문자열과 프로퍼티의 이름 작성 시 **큰따옴표를 사용**해야 합니다. 작은 따옴표는 사용불가합니다.
> - 콤마나 콜론을 잘못 배치하는 사소한 실수로 인해 JSON파일이 잘못되어 작동하지 않을 수 있습니다. JSONLint같은 어플리케이션을 사용해 JSON 유효성 검사를 할 수 있습니다.
> - JSON은 객체나 배열뿐만 아니라 문자열, 숫자 같은 단일 값 하나도 표현할 수 있습니다. 즉, 배열이나 오브젝트 외에도 단일 문자열이나 숫자 또한 유효한 JSON 값이 됩니다.
> - JavaScript에서 오브젝트 프로퍼티가 따옴표를 생략할 수 있는 경우가 있지만, JSON에서는 프로퍼티 이름을 큰따옴표로 작성해야 합니다.

- **JSON은 데이터 포맷이다.**
  - **데이터 포맷**: 데이터를 어떤 규칙으로 표현하고 저장하고 주고받을지를 정해놓은 형식
  - JSON은 기본적으로 `"무엇을 가지고 있는가? (데이터)"` 를 표현하는 형식이다.
  - `"무엇을 실행할 것인가? (동작(메서드))"` 를 표현하는 형식이 아니다.


- **JSON은 큰따옴표를 사용한다.**
  - JavaScript 에서는
    ```javascript
    const user = {
        name: "영수",
        age: 25
    };
    ```
    이렇게 프로퍼티 이름에 따옴표를 생략할 수 있다.
  - JSON에서는 프로퍼티 이름을 큰따옴표로 감싸야 한다.


- **콤마와 콜론을 조심해야 한다.**


- **JSON은 생각보다 데이터 타입의 범위가 넓다**
  - 앞에서 본 `{}`,`[]`의 형태만 가능한 것은 아니다.
  - 숫자 하나, 문자열 하나, 불리언도 가능하다.
  - null은 **값이 없음**을 표현하는 JSON 값이다.
    ```text
    JSON 값
    ├── 객체
    ├── 배열
    ├── 문자열
    ├── 숫자
    ├── 불리언
    └── null
    ```
    
- **JavaScript 객체와 차이**
  - JavaScript 객체 
    ```JavaScript
    const user = {
    name: "영수",
    age: 25,
  
        sayHello() {
            console.log("Hello");
        }
    };
    ```
    - 프로퍼티 이름에 따옴표를 생략할 수 있는 경우가 있음
    - 메서드를 포함할 수 있음
  - JSON
    ```text
    {
      "name": "영수",
      "age": 20
    }
    ```
    - 프로퍼티 이름은 큰따옴표로 작성해야 함
    - 메서드를 포함할 수 없음
    - 데이터를 표현하는 데 집중함