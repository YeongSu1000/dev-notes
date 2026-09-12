참고 자료: [Container Overview](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)

## 컨테이너 개요

Configuration Metadata: 애플리케이션 개발자가 Spring 컨테이너에게 어떤 객체를 생성하고, 설정하고, 조립해야 하는지 알려주는 정보  
컨테이너는 어떤 컴포넌트를 생성하고, 설정하고, 조립해야 하는지에 대한 지시를 Configuration Metadata를 읽어서 얻음.  
대표적으로 다음과 같은 형태로 표현할 수 있음
- 어노테이션 기반
- 자바 기반
- XML 기반

Bean Definition: Configuration Metadata를 바탕으로, Spring이 관리할 Bean을 어떻게 만들고 구성할지에 대한 정의 정보  
Configuration Metadata를 바탕으로 Spring Container는 BeanDefinition이라는 형태의 Bean 구성 정보를 확보하고, 이를 바탕으로 Bean을 생성하고 구성한다.

> Configuration Metadata: Container에게 "무엇을 어떻게 관리할지" 알려주는 정보   
> Bean Definition: Bean을 어떻게 구성하고 관리할지에 대한 정의  
> Bean: Container가 관리하는 실제 객체
