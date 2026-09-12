참고 자료: [Introduction to the Spring IoC Container and Beans](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html)

## Spring IoC 컨테이너와 Bean 소개

의존성 주입(DI): IoC의 한 형태(IoC를 구현하는 대표적인 방법). 객체가 의존성을 생성하거나 찾는 대신 IoC 컨테이너가 Bean을 생성할 때 의존성을 주입함.

(스프링 환경)객체는 의존성을 다음과 같은 방법을 통해서만 정의함
- 생성자의 인자
- 팩토리 메서드의 인자
- 객체가 생성되거나 팩토리 메서드에서 반환된 후 객체의 프로퍼티에 설정되는 값

Spring IoC 컨테이너가 객체를 생성하고, 필요한 의존성을 연결해준다.
Spring 컨테이너가 관리하는 객체를 Bean이라고 부른다.
- `"MemberService를 bean으로 등록한다"` = MemberService 객체를 Spring이 관리하도록 만든다.

ApplicationContext: Spring IoC 컨테이너의 대표적인 인터페이스. Bean을 생성/조립/관리

Spring이 없으면:
```text
개발자가 직접

객체 생성
   ↓
객체 연결
   ↓
객체 사용
```
Spring을 사용하면:
```text
Spring Container

객체 생성
   ↓
객체 관리
   ↓
객체들의 의존성 연결
   ↓
개발자는 객체를 사용
```

> IoC: 객체를 직접 생성하고 조립하던 제어권이 개발자(애플리케이션 코드)에서 프레임워크(Spring IoC container)로 넘어가는 구조.  
> Dependency: 객체가 사용하기 위해 필요한 다른 객체  
> DI: 객체가 필요한 의존성을 직접 만들지 않고 외부에서 전달받는 구조 (Dependency를 외부에서 전달)   
> Container: 객체들을 생성하고 연결하는 역할  
> Bean: Spring IoC 컨테이너에 의해 생성되고, 조립되며, 관리되는 객체  
> ApplicationContext: Spring IoC Container를 나타내는 인터페이스

```Java
    public class MemberRepository{
        public void save(){
            System.out.println("회원 저장");
        }
    }

    public class MemberService{
        private final MemberRepository repository;

        public MemberService() {
            this.repository = new MemberRepository(); // MemberService가 MemberRepository를 필요로 한다.
                                                      // 따라서 MemberRepository는 MemberService의 Dependency(의존성)이다.
        }

        public void join() {
            repository.save();
        }
    }
```
`MemberService`가 직접 `new MemberRepository()`를 실행하고 있다.
Repository 구현을 `MySqlMemberRepository`이나 `JpaMemberRepository`로 바꾸려면 Service가 객체를 직접 생성하고 있기 때문에 Service의 코드까지 바꿔야 한다.
```java
public class MemberService {
    private final MySqlMemberRepository repository; // 1. 변수 타입을 MySqlMemberRepository로 변경

    public MemberService() {
        this.repository = new MySqlMemberRepository(); // 2. new 키워드 뒤의 생성자도 MySqlMemberRepository로 변경
    }

    public void join() {
        repository.save();
    }
}
```



```java
public interface MemberRepository {
    void save();
}

public class MySqlMemberRepository implements MemberRepository {
    @Override
    public void save() {
        System.out.println("MySQL 저장");
    }
}

public class JpaMemberRepository implements MemberRepository {
    @Override
    public void save() {
        System.out.println("JPA 저장");
    }
}

public class MemberService {

    private final MemberRepository repository;

    public MemberService(MemberRepository repository) { // Service 내부에서 더이상 new MemberRepository() 하지않고 생성자를 통해 받음
        this.repository = repository; // 외부에서 의존성을 주입받음 (DI)
    }

    public void join() {
        repository.save();
    }
}

public static void main(String[] args) {
    MemberRepository repository = new JpaMemberRepository(); // 사용할 구현체 직접 생성

    MemberService service = new MemberService(repository); // MemberService 생성 시점에 의존성 주입(DI)
    service.join();
    // Spring은 이 역할을 IoC Container라는 구조를 통해 담당한다.
}
```