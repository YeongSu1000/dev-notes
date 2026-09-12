참고 자료: [Dependency Injection](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html)

## 의존성 주입
의존성 주입(DI)은 객체가 자기 Dependency를 직접 생성(new)하거나 찾아가는 것이 아니라, 외부에서 Dependency를 제공받는 방식이다. Spring에서는 이 역할을 컨테이너가 담당한다.

- 생성자의 인자
- 팩토리 메서드의 인자
- 객체가 생성된 후 또는 팩토리 메서드에서 반환된 후 객체의 인스턴스에 설정되는 프로퍼티

컨테이너는 Bean을 생성하고 의존관계를 구성하는 과정에서 이러한 방식으로 Dependency를 주입한다.

Java에서는 개발자가 객체를 만들고 Dependency도 만듬.  
→ 객체 생성과 연결을 개발자가 통제  
Spring에서는 컨테이너가 객체의 생성과 연결을 담당  
→ 제어의 일부가 객체 자신/애플리케이션 코드에서 컨테이너 쪽으로 넘어감 = IoC. IoC를 구현하는 중요한 방식 중 하나가 DI.


DI 원칙을 사용하면
- 코드가 더 깔끔해진다
- 객체에 의존성을 외부에서 제공함으로써 객체 간 결합도를 효과적으로 낮출 수 있다.
- 객체는 의존성을 직접 생성하거나 찾을 필요가 없다. 다만 자신이 어떤 의존성을 필요로 하는지는 알고 있어야 한다.
- 의존성이 인터페이스나 추상 클래스를 대상으로 하는 경우, 단위 테스트에서 Stub나 Mock 구현체를 사용할 수 있기에 테스트하기가 더욱 쉬워진다.

DI에는 크게 두 가지 방식이 존재
1. 생성자 기반 의존성 주입(Constructor-based DI)
2. Setter 기반 의존성 주입(Setter-based DI)

### Constructor Injection
생성자를 통해 Dependency를 전달하는 방식
```java
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) { // MemberService를 만들려면 반드시 MemberRepository를 받아야 함
        this.memberRepository = memberRepository;
    }
}
```

Spring 컨테이너에 `MemberRepository Bean`, `MemberService Bean` 두 Bean이 있다면, 컨테이너는 `MemberService`를 만들려면 생성자에 `MemberRepository`가 필요하다는 것을 알아냄.

Constructor Injection의 장점  
필수 Dependency를 반드시 받게 만들 수 있다. (`MemberRepository`없이 만들어질 수 없기때문)
```text
MemberService 생성 시점
        ↓
MemberRepository 반드시 존재
        ↓
완전히 초기화된 객체
```
- 객체를 immutable하게 만들기 쉽다.
- 필수 Dependency가 null이 되는 것을 방지
- 호출하는 코드에 완전히 초기화된 객체를 제공

### Setter Injection
```java
public class MemberService {

    private MemberRepository memberRepository;

    public void setMemberRepository(
            MemberRepository memberRepository) {

        this.memberRepository = memberRepository;
    }
}
```

컨테이너가 객체를 만든 다음:
```text
MemberService 생성
       ↓
setMemberRepository(repository)
       ↓
Dependency 주입 완료
```
하는 방식

Constructor Injection과 차이는 언제 Dependency가 전달되느냐
- Constructor Injection
    - ```java
    new MemberService(repository);
    ```
    - 생성할 때 전달
- Setter Injection
    - ```java
    MemberService service = new MemberService();

    service.setMemberRepository(repository);
    ```
    - 생성한 다음 전달


- 둘중 뭐가 더 좋은가?
    - 필수 의존성 → Constructor
    - 선택적 의존성 → Setter

> Constructor Injection: 생성자를 통해 Dependency를 전달  
> Setter Injection: 객체 생성 후 setter를 통해 Dependency를 전달  
> Spring은 일반적으로 필수 의존성에는 Constructor Injection을 권장

`public MemberService(MemberRepository repository)` 를 보면 `MemberService`는 `MemberRepository`가 필요하다는 것을 알 수 있다.  
그런데 이것만 가지고는 아직 Spring이 실제로 `MemberRepository`라는 어떤 객체를 넣어야 하는지 결정된 것은 아니다.  
개발자가 자바 코드로 직접 new 하여 만들 수도 있고, 컨테이너가 가지고 있는 Bean 중에서 찾아서 넣을 수도 있다. 즉 DI를 실제로 수행하려면 컨테이너가 Dependency를 해결(resolve)해야 한다.

### Dependency Resolution
필요한 Dependency를 어떤 Bean이나 값으로 충족할지 결정하고, 이를 주입할 수 있도록 해결하는 과정

```text
MemberService 생성 필요
        ↓
생성자 확인
        ↓
MemberRepository가 필요함
        ↓
MemberRepository에 해당하는 Bean을 찾아야 함
        ↓
찾은 Bean을 생성자에 전달
```

### Type Matching
```java
public class ThingOne {

    public ThingOne(
        ThingTwo thingTwo,
        ThingThree thingThree
    ) {
    }
}
```
컨테이너 입장에서 생성자를 보면
```text
ThingOne
  └── 생성자
       ├── ThingTwo 필요
       └── ThingThree 필요
```
컨테이너가 `ThingTwo`타입에 해당하는 Bean을 찾고, `ThingThree`타입에 해당하는 Bean을 찾는다.  
생성자 인자 해석에서 특별한 모호성이 없다면 인자의 타입을 이용해 매칭한다.

### Dependency Graph
```text
OrderController
       ↓
OrderService
       ↓
OrderRepository
       ↓
DatabaseClient
```

Spring이 `OrderController`를 만들려면
```text
OrderController
    ↓
OrderService 필요
    ↓
OrderRepository 필요
    ↓
DatabaseClient 필요
```
를 따라가야 한다.  
하나의 Bean을 생성하기 위해 그 Bean의 Dependency와 그 Dependency의 Dependency까지 연쇄적으로 해결해야 할 수 있다. 이러한 의존관계가 연결된 구조를 Dependency Graph라고 볼 수 있다.

Controller 하나 만들었을 뿐인데 여러 객체가 생기는 이유?  
`OrderController`가 정상적으로 동작하려면 `OrderService`가 필요하고, `OrderService`가 정상적으로 동작하려면 `OrderRepository`가 필요하다.  
그래서 컨테이너가 Controller를 생성하는 과정에서 연쇄적으로 필요한 객체들을 준비하게 된다. - 객체 전체의 의존관계를 구성하는 과정

### Bean 생성 시점
컨테이너가 설정 정보를 읽었다고 해서 모든 Bean이 무조건 그 순간 바로 생성되는 것은 아님  
Bean의 Scope나 Lazy 설정 등에 따라 생성 시점이 달라질 수 있다.  
`ApplicationContext 생성` ≠ `모든 Bean 객체가 실제로 생성됨`
> 컨테이너에는 Bean을 만들기 위한 정보가 있고, 실제 Bean 객체 생성은 설정된 생성 시점에 이루어진다.

컨테이너가 Dependency를 해결하는 과정에서 필요한 Bean이 없거나 의존관계를 해결할 수 없으면 문제가 발생할 수 있다.
Bean의  생성 시점에 따라 문제가 발견되는 시점도 달라질 수 있다.
> ApplicationContext가 정상적으로 만들어졌으니까 모든 Bean 생성 문제도 완전히 검증된 것이다 (X)

### Circular Dependency
`class A{ A(B b) }`, `class B{ B(A a) }`  
Bean A가 Bean B를 필요로 하고, Bean B가 다시 Bean A를 필요로 하는 경우
- Constructor Injection에서는 객체를 생성하기 위해 Dependency가 먼저 필요하므로 순환 의존성을 해결하기 어렵다.
- Setter Injection은 객체 생성과 Dependency 주입이 분리되어 있기 때문에 순환 의존성이 발생할 수 있다.

> Java 기능: `public MemberService(MemberRepository repository)` 생성자를 정의하는 것.  
> DI 설계 원리: MemberRepository를 MemberService 외부에서 전달  
> Spring IoC Container 기능: 컨테이너가 MemberRepository Bean을 찾아서 MemberService 생성자에 전달
