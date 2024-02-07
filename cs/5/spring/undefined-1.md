# 이은비

## :leaves: 스프링과 스프링 부트의 차이점

> 스프링(Spring)은 프레임워크이며, 스프링 부트(Spring Boot)는 스프링 프레임워크를 기반으로 한 도구입니다. 예를 들어, 스프링은 설정 파일을 작성해야 하는 반면, 스프링 부트는 자동 설정을 통해 간편하게 개발할 수 있습니다.

* 스프링은 스프링 프레임워크의 핵심 모듈을 모아서 만든 프레임워크입니다.
  * 개발자가 직접 설정 파일을 작성하여 스프링 컨테이너를 구성하고, 필요한 빈 객체를 등록하여 빈 객체 간의 의존성을 설정해야 합니다.
* 반면, 스프링 부트는 스프링 프레임워크를 보다 쉽게 사용할 수 있도록 만든 프레임워크입니다.
  * 프로젝트의 설정과 라이브러리 의존성을 자동으로 처리해주는 기능을 제공합니다.
  * 또한, 내장 서버를 제공하여 쉽게 웹 애플리케이션을 실행할 수 있습니다.

> [https://www.inflearn.com/blogs/3315](https://www.inflearn.com/blogs/3315)

### 스프링 부트 3.x를 사용한 이유

* 스프링의 릴리즈 주기를 생각했을 경우, 대략 1 \~ 3년의 지원 기간을 가지므로 당시에 가장 최신 버전이었던 3.1을 선택하게 되었습니다.
* 또한, 이와 맞물려 자바 LTS 버전 중 가장 긴 보장 기간 + 최신 버전인 Java 17을 사용하게 되었습니다.

> [https://kghworks.tistory.com/137](https://kghworks.tistory.com/137)

## 동작 원리

## Dispatcher Servlet

> 디스패처 서블릿(Dispatcher Servlet)은 적합한 컨트롤러와 메서드를 찾아 요청을 위임합니다.

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

동작 과정은 다음과 같습니다.

1. 클라이언트의 요청을 디스패처 서블릿(Dispatcher Servlet)이 받습니다.
2. 요청 정보를 통해 요청을 위임할 컨트롤러(Controller)를 찾습니다.
3. 요청을 컨트롤러로 위임할 핸들러 어댑터(Handler Adapter)를 찾아서 전달합니다.
4. 핸들러 어댑터가 컨트롤러로 요청을 위임합니다.
5. 비즈니스 로직을 처리합니다.
6. 컨트롤러가 반환값(Response Entity)을 반환합니다.
7. 핸들러 어댑터가 반환값을 처리합니다.
8. 서버의 응답을 클라이언트로 반환합니다.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

> [https://jeong-pro.tistory.com/96](https://jeong-pro.tistory.com/96)
>
> [https://tall-developer.tistory.com/17](https://tall-developer.tistory.com/17)

### Dispatcher Servlet - 동기 vs 비동기

> Dispatcher Servlet은 동기로 지원하고 있지만, @Async와 같은 어노테이션을 통해 비동기로 처리할 수도 있습니다. 예를 들어, 이메일을 비동기적으로 처리하고 싶을 경우 위 어노테이션을 통해 진행합니다.

> [https://velog.io/@akfls221/%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-NonBlocking-Blocking%EA%B3%BC-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%9D%98-%EA%B4%80%EA%B3%84](https://velog.io/@akfls221/%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-NonBlocking-Blocking%EA%B3%BC-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%9D%98-%EA%B4%80%EA%B3%84)
>
> [https://hyokeun0419.tistory.com/105](https://hyokeun0419.tistory.com/105)

## 톰캣의 동작 과정

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

동작 과정은 다음과 같습니다.

1. 클라이언트의 요청이 HTTP request로써 서블릿 컨테이너(Servlet Container)에 전송됩니다.
2. 서블릿 컨테이너는 HttpServletRequest, HttpServletResponse 두 객체를 생성합니다.

* 이때, 사용자가 요청한 url을 분석하여 어느 서블릿에 대한 요청인지 확인합니다.

3. 해당 서블릿을 요청한 적이 없거나, 현재 메모리에 생성된 인스턴스가 없다면 새로 생성합니다. (= init())
4. 컨테이너는 이후service() -> POST, GET 여부에 따라 doGet() 또는 doPost() 호출
5. 실행된 메서드는 동적인 페이지를 생성한 후, HttpServletResponse 객체에 응답을 보냅니다.
6. 응답을 완료한 후, HttpServletRequest / HttpServletResponse 두 객체를 소멸시킵니다.

> [https://girinprogram93.tistory.com/68](https://girinprogram93.tistory.com/68)

## DI (Dipendency Injection)

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### @Autowired vs @Resources vs @Inject

> 의존 객체 자동 주입 (Automatic Dependency Injection)을 위해 사용하는 어노테이션으로서, 의존 객체를 찾는 방식이 다릅니다.

* 스프링 설정파일에서 혹은 태그로 의존 객체 대상을 명시하지 않아도 스프링 컨테이너가 자동적으로 의존 대상 객체를 찾아 해당 객체에 필요한 의존성을 주입하기 위해 사용하는 어노테이션입니다.

### @Autowired

> 스프링에서 지원하는 어노테이션으로서,   객체의 타입 → 객체의 이름 → @Qualifier → 실패 순으로 의존 객체를 찾습니다.

* 주로 멤버 변수, setter, 생성자, 일반 메서드에 적용합니다.

### @Resources

> 자바 EE에서 지원하는 어노테이션으로서, 객체의 이름 → 객체의 타입 → @Qualifier → 실패 순으로 의존 객체를 찾습니다.

* 주로 멤버 변수, setter에 사용합니다.

### @Inject

> 자바에서 지원하는 어노테이션으로서, 객체의 타입 → @Qualifier → 객체의 이름 → 실패 순으로 의존 객체를 찾습니다.

* 주로 멤버 변수, setter, 생성자, 일반 메서드에 적용 가능합니다.

> [https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4](https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4)

### DI의 3가지 방법

1. 필드 주입 (Field Injection)

> 의존성을 주입하고 싶은 필드에 @Autowired 어노테이션을 붙여서 진행합니다.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

2. 세터 주입 (Setter Injection)

> setter()에 @Autowired 어노테이션을 붙여 의존성을 주입합니다.

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

3. 생성자 주입 (Constructor Injection)

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

> [https://isoomni.tistory.com/entry/TISPRING-IOC-DI-%EC%A0%95%EC%9D%98-%EC%9E%A5%EC%A0%90](https://isoomni.tistory.com/entry/TISPRING-IOC-DI-%EC%A0%95%EC%9D%98-%EC%9E%A5%EC%A0%90)

### DI의 장점

> DI(의존성 주입)를 통해서 모듈 간의 결합도가 낮아지고 유연성이 높아집니다.

## IoC (Inversion of Control, ★)

> "제어의 역전"으로서, 메서드나 객체의 호출 작업을 개발자가 아닌 외부에서 결정되는 것을 의미합니다.

스프링(Spring)에서 객체를 생성하는 순서는 다음과 같습니다.

1. 객체 생성

* 이때, 개발자가 아닌 스프링에 설정된 방식대로 자동으로 객체를 생성합니다.

2. 의존성 객체 주입

* 이때, 제어를 스프링에게 위임하여 스프링이 만들어 놓은 객체를 주입합니다.
  * 즉, 스프링 컨테이너가 객체를 주입시킵니다.

3. 의존성 객체 메서드 호출

* 스프링에 의해 의존성 객체가 주입된 후, 해당 객체의 메서드는 주입받은 클래스에 의해 호출될 수 있습니다.

> [https://velog.io/@gillog/Spring-DIDependency-Injection](https://velog.io/@gillog/Spring-DIDependency-Injection)

### 라이브러리 vs 프레임워크

> 라이브러리는 라이브러리를 가져다가 사용하고 호출하는 측에 주도성을 가진 반면, 프레임워크는 틀 안에 제어 흐름에 대한 주도성을 내포하고 있다고 볼 수 있습니다.

> [https://webclub.tistory.com/458](https://webclub.tistory.com/458)

## &#x20;Web Server vs WAS(Web Application Server)

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

> &#x20;웹 서버(WS, Web Server)는 이미지나 단순 HTML과 같은 정적인 리소스를 처리하며 WAS만을 이용할 때보다 빠르고 안정적으로 기능을 수행합니다.
>
> 반면, 웹 어플리케이션 서버(WAS, Web Application Server)는 JSON과 같은 동적인 데이터를 위주로 처리하며 DB와 연결되어 사용자와 데이터를 주고 받고 조작이 필요한 경우 활용합니다.

> [https://yozm.wishket.com/magazine/detail/1780/](https://yozm.wishket.com/magazine/detail/1780/)

## 테스트

### 목 객체 (Mock Object)

> 목 객체(Mock Object)란 "가짜 객체"로써, 단위 테스트 시 외부와 단절되어 있는 환경  - 즉, 외부의 영향을 받지 않으며 순수한클래스 및 메서드 수준에서 테스트를 진행하기 위해 사용합니다.

> [https://dingdingmin-back-end-developer.tistory.com/entry/Springboot-Test-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1-1](https://dingdingmin-back-end-developer.tistory.com/entry/Springboot-Test-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1-1)

> [https://mangkyu.tistory.com/145](https://mangkyu.tistory.com/145)
