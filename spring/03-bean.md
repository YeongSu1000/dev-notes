참고 자료: [Bean Overview](https://docs.spring.io/spring-framework/reference/core/beans/definition.html)

## Bean 개요
Bean은 Spring IoC Container가 생성하고 관리하는 객체이다.  
일반 Java 객체라고 해서 자동으로 Bean이 되는 것은 아니다.  
Spring Container에 Bean으로 등록되어야 Container가 관리한다.


Spring에게 MemberRepository를 Bean으로 관리하라고 할때, 컨테이너는 아무 정보 없이 `new MemberRepository();`를 호출할 수 없다.  
그래서 컨테이너에는 Bean을 만들기 위한 설정 정보가 필요하다. 이 설정 정보를 컨테이너 내부에서는 BeanDefinition 이라는 객체로 표현한다.  
BeanDefinition은 실제 Bean 객체가 아니라, Bean을 어떻게 생성하고 관리할지에 대한 메타데이터/설정 정보
- BeanDefinition = Bean을 어떻게 생성하고 관리할지에 대한 설정 정보
- Bean = 그 설정을 바탕으로 Container가 생성하고 관리하는 실제 객체

BeanDefinition 안에는 다음과 같은 정보가 들어갈 수 있다.
```text
BeanDefinition
│
├── 어떤 클래스인가?
├── Bean 이름은 무엇인가?
├── Scope는 무엇인가?
├── 생성자 인자는 무엇인가?
├── 어떤 다른 Bean이 필요한가?
├── 초기화 방법은 무엇인가?
├── 소멸 방법은 무엇인가?
└── 기타 설정
```
컨테이너는 단순히 `MemberRepository` 라는 객체가 있다. 정도만 알고 있는게 아니라, 그 객체를 어떻게 구성하고 관리할지에 대한 여러 정보를 가지고 있다. 클래스 정보 하나만으로는 Bean을 완전히 정의할 수 없기 때문이다.

Bean 생성  
BeanDefinition을 참조하여 컨테이너가 실제 객체를 생성하는 3가지 방법
1. Constructor
2. Static Factory Method
3. Instance Factory Method

- Constructor
    - 가장 단순한 생성 방식
    - 생성자를 이용한 Bean 생성은 Spring 전용 방식 등 특정한 방식이 아닌 일반적인 Java 클래스라면 Bean 클래스를 지정하는 것만으로 거의 모두 Spring에서 사용 가능
