# 이은비

## :leaves: 스프링(Spring)

### 내가 스프링 프레임워크를 선호하는 이유

> 학습할 때 레퍼런스가 많다는 장점을 가진 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크이기 때문입니다.



## :leaves: 인터셉터(Interceptor)

> 인터셉터는 Spring이 제공하는 기술로써, Dispathcer Servlet이 Controller를 호출하기 전과 후에 요청 및 응답을 참조하거나 가공할 수 있는 기능을 제공합니다.

* 인터셉터는 Spring context에서 동작을 수행합니다.

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* 인터셉트는 다음과 같은 3개의 메서드를 가지고 있습니다.

1. preHandle : 컨트롤러가 호출되기 전 실행, 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공 및 추가하는 경우에 사용합니다.
2. postHandle : 컨트롤러를 호출된 후에 실행, 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용합니다.
3. afterCompletion : 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행, 요청 처리 중에 사용한 리소스를 반환할 때 사용하기에 적합합니다.

### :arrow\_forward: 필터(Filter) vs 인터셉터(Interceptor)

1. Filter

* 서블릿 컨테이너가 관리하기에 스프링에서 예외 처리를 진행하지 않습니다.
* 또한, request 및 response 객체를 조작할 수 있습니다.
* 주로 공통된 보안 및 인증 / 인가 관련 작업을 할 때 사용합니다.
* 또한, 모든 요청에 대한 로깅 또는 감사를 진행할 때 사용합니다.
* 그 밖에는 이미지 / 데이터 압축 및 문자열 인코딩에 사용합니다.

2. Interceptor

* 스프링 컨테이너가 관리하기에 스프링에서 예외 처리를 진행할 수 있습니다.
* 또한, request 및 response 객체를 조작할 수 없습니다.
* (:check) 주로 세부적인 보안 및 인증 / 인가와 같은 공통 작업을 진행할 때 사용합니다.
* 또한, API 호출에 대한 로깅 또는 감사에 사용합니다.
* 그 밖에는 Controller로 넘겨주는 정보(데이터)의 가공에 사용합니다.

> [https://mangkyu.tistory.com/173](https://mangkyu.tistory.com/173)

### (:check) Interceptor는 어디서 mocking이 일어날까?

### :arrow\_forward: JWT 토큰을 검증할 때, 둘 중 어느 곳에서 검증할까?

JWT 토큰은 로그인 과정을 진행할 때 검사하는 토큰입니다.&#x20;

따라서 공통된 보안 작업(Common Security Task)의 예로 볼 수 있으며, 이는 애플리케이션 전체에 걸쳐 일관되게 적용되어야 하는 보안 관련 작업입니다.

이러한 작업은 인증(Authentication)과 인가(Authorization) 과정에서 발생하며 모든 요청이 보안 검사를 거쳐야 하므로, JWT 토큰은 필터에서 처리되어야 합니다.

### (:check) 인터셉터가 아닌 필터에서 처리하는 이유?

### (:check) 헤더를 조작하기 어려운 이유?



## :leaves: JPA

### :arrow\_forward: 영속성 컨텍스트(Persistence Context)란?

> Entity를 영구 저장하는 환경이라는 뜻입니다. 즉, 애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 데이터베이스와 같은 역할을 진행합니다.

* 엔티티 매니저를 통해 엔티티를 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리합니다.

예시는 다음과 같습니다.

```
em.persist(member); // 엔티티 매니저를 통해 회원 엔티티를 영속성 컨텍스트에 저장
```

특징은 다음과 같습니다.

1. 엔티티 매니저를 생성할 때 영속성 컨텍스트는 하나 만들어집니다.
2. 엔티티 매니저를 통해서 영속성 컨텍스트에 접근하고 관리할 수 있습니다.

> [https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80](https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80)

### :arrow\_forward: N + 1을 해결하는 방법?

예시는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

* 위와 같이 Academy를 호출하여 그 안에 속한 Subject를 사용한다고 가정합니다.
  * 이때, `findAllSubjectName` 를 호출하면..
* Academy를 전체 조회하는 쿼리와 각각의 Academy가 본인들의 subject를 조회하는 쿼리 10개(subject가 10개라)가 발생하는 현상이 나타납니다.

위와 같이 하위 엔티티들에 대해 첫 쿼리 실행 시, 한 번에 가져오지 않고 Lazy Loading으로 필요한 곳에서 사용되어 쿼리가 실행될 때 발생하는 문제가 `n + 1` 문제 입니다.

* 즉, 하나의 `Academy` 를 조회하고 이와 연관된 모든 `Subject` 를 조회하기 위해 추가적으로 n번의 쿼리가 발생하는 현상입니다.

&#x20;이를 해결하기 위해서는 대표적인 해결 방법이 몇 가지 있습니다.

1. Join Fetch

> 조회 시 바로 가져오고 싶은 엔티티 필드를 지정 (`join fetch a.subjects`)

2. @EntityGraph

> \`@EntityGraph(attributePaths = "사용할 필드명")

* 단, Join Fetch는 Inner Join / Entity Graph는 Outer Join이라는 특징이 있습니다.
  * 따라서 공통적으로 `카타시안 곱` 이 발생할 수 있습니다.
  * 즉, 만약 한 `Academy` 가 여러 `Subject` 와 연관되어 있고, 이를 함께 조회하는 쿼리를 실행하면 결과 집합은 둘의 모든 조합을 포함하여 DB 상에서 `Academy` 의 데이터가 `Subject` 의 수만큼 중복되어 나타납니다.
* 따라서, `DTO` 를 통해 필요한 데이터만 선택적으로 가져오거나 `@Fetch(FetchMode.SUBSELECT)` 와 같은 전략을 사용하여 연관 엔티티를 조회하는 방법 등을 고려할 수 있습니다.

> [https://jojoldu.tistory.com/165](https://jojoldu.tistory.com/165)

### :arrow\_forward: save vs saveAndFlush

> save()는 엔티티를 영속성 컨텍스트(persistence context)에 저장합니다. 이때, 엔티티를 데이터베이스에 즉시 저장하는 것이 아니라, 트랜잭션이 종료될 때(= 트랜잭션의 커밋이 발생)할 때 데이터베이스에 반영이 됩니다.

1. save() : 엔티티를 영속성 컨텍스트에 저장하고, 트랜잭션이 커밋될 때까지 데이터베이스에 반영을 지연시킵니다.

* 동일한 트랜잭션 내에서 여러 작업을 수행하고 한 번에 데이터베이스에 반영하려는 경우에 적합합니다.

2. saveAndFlush() : 엔티티를 영속성 컨텍스트에 저장하고, 즉시 데이터베이스에 flush하여 변경 내용을 반영합니다.

* 변경 사항을 즉시 데이터베이스에 반영해야 할 때 사용됩니다.
  * 테스트 케이스에서 DB에 즉시 반영해 상태를 확인해야 할 경우 또는 특정 조건 ㅎ아래에서 즉시 데이터베이스 상태를 갱신해야할 때 유용합니다.

> [https://velog.io/@seongwop/Spring-JPA-%EB%A9%94%EC%86%8C%EB%93%9C-save-vs-saveAndFlush](https://velog.io/@seongwop/Spring-JPA-%EB%A9%94%EC%86%8C%EB%93%9C-save-vs-saveAndFlush)
