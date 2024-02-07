# 고청천

## :leaves: 기타

### :arrow\_forward: 스프링을 선택한 이유

오랜 시간 인기를 끌며, 다양한 비지니스 문제를 해결하기 위해 개선 발전 된 프로젝트라 선택하였습니다. 안정적인 프로젝트 개발을 위해 성숙한 기술을 선택하는 것이 중요하다 생각하고 있습니다. 이런 관점에서 스프링을 선택 하였습니다.

## :leaves: Interceptor

### :arrow\_forward: 인터셉터와 필터 선택 기준

필터는디스페처 서블릿에 요청이 도달하기 전/ 후 별도의 부가 작업을 할 수 있는 기능을 제공합니다.&#x20;

인터셉터는 디스페처 서블릿이 컨트롤러를 호출 전/후 부가 작업을 할 수 있는 기능을 제공합니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

어떤 용도로 사용할 지에 따라 선택이 달라져야 합니다.

[https://mangkyu.tistory.com/173](https://mangkyu.tistory.com/173)

### :arrow\_forward: JWT 토큰을 검증 할 때 인터셉터와 필터 중 어디서 할까?

어느 쪽을 선택해도 상관 없다 생각합니다. 다만, 보다 빠르게 jwt 토큰을 검증해서 트래픽 비용을 절감하기 위해 필터처리하는 것이 어떨까 생각합니다.

## :leaves: JPA

### :arrow\_forward: JPA 는 뭘까?

JPA 는 자바 ORM 입니다. method 를 통해 db를 조작할 수 있고, 객체 지향적 코드를 작성할 수 있습니다. 또한 하위 디비가 바뀌는 것에 상관없이 작동 할 수 있습니다. (이는 구체에 의존하지 않고 추상화에 의존하기 때문입니다)

[https://docs.oracle.com/javaee%2F7%2Fapi%2F%2F/javax/persistence/EntityManager.html](https://docs.oracle.com/javaee%2F7%2Fapi%2F%2F/javax/persistence/EntityManager.html)

### :arrow\_forward: 영속성 컨택스트는 뭘까?

영속성 컨텍스트는 엔티티를 영구 저장하는 환경을 의미합니다. JPA에서는 엔티티가 영속성 컨텍스트에 포함되면, 해당 엔티티를 관리(생성, 조회, 업데이트, 삭제)하는 것을 맡습니다.

### :arrow\_forward: N+1 을 해결하는 방법

N+1 문제는 1개의 쿼리로 가능한 작업을 N개의 쿼리로 수행하는 것을 말합니다. 이를 해결하기 위한 방법으로는 주로 '조인 쿼리'를 사용하거나, 'Fetch Join' 또는 'Batch Size' 설정 등을 사용합니다. 혹은 연관 관계를 설정하지 않는 방법도 있습니다.&#x20;



Batch Size의 경우 100을 기준으로 조정을 합니다. (doc)

### :arrow\_forward: save 와 saveAndFlush

save 메서드는 데이터를 DB에 저장하는 역할을 합니다. save를 호출하면, 실제로 DB에 즉시 반영되는 것이 아니라, 트랜잭션이 끝나는 시점에 DB에 반영됩니다.

반면 saveAndFlush 메서드는 save와 달리, 호출 즉시 DB에 반영됩니다. 이 방식은 테스트 코드에서 DB 결과를 즉시 확인해야 하는 경우 등에 유용하게 사용됩니다.

