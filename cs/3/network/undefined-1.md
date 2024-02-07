# 이은비

## :minidisc: 웹

### 웹이란 무엇일까요?

> 브라우저를 통해서 서버에서 데이터를 받아온 후, 받아온 데이터를 렌더링해서 보여주는 것

<figure><img src="../../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

## URI, URL, URN의 차이에 대해서 설명해주세요.

### URI(Uniform Resource Identifier) : 통합 자원 식별자(= resource에 대한 id)

> scheme:\[//\[user\[:password]@]host\[:port]]\[/path]\[?query]\[#fragment]

인터넷에 있는 자원을 나타내는 유일한 주소입니다.

각 리소스는 리소스 식별을 위해 HTTP 전체에서 사용되는 URI로 식별됩니다.

* resource : 문서, 사진 등으로 요청하는 어떤 것이든 대상이 될 수 있다.

### URL(Unifrom Resource Locater) : 파일 식별자(= resource의 위치)

> scheme:\[//\[user\[:password]@]host\[:port]]\[/path]\[?query]

(네트워크 상에서) 자원이 어디 있는지를 식별하는 URI입니다.

### URN(Uniform Resource Name) : 통합 자원 이름

> "urn:" ":"

> ex. urn:isbn:9780141036144

이름으로 자원을 식별하는 URI입니다.

## Stateless에 대해 설명해주세요.

> 서버가 클라이언트의 상태를 보존하지 않는 것

<figure><img src="../../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

> ex. 로그인이 필요 없는 단순한 서비스 소개 화면

따라서 서버 확장성이 높다는 장점(= scale out)이 있으나 클라이언트가 추가 데이터를 전송해야 한다는 단점이 있습니다.

## Stateful에 대해서 설명해주세요.

> 서버가 클라이언트의 상태를 보존하는 것입니다.

<figure><img src="../../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

따라서 같은 기능을 수행하는 서버가 여러 대가 존재하며, 서버가 장애가 날 경우 상태 유지에 실패하게 됩니다.

## 사용자 요청에서 어떻게 사용자를 구분할 수 있을까요?

> [https://kscodebase.tistory.com/312](https://kscodebase.tistory.com/312)

> HTTP 헤더, IP 주소, 사용자 로그인 인증, 쿠키 등을 통해 사용자를 식별합니다.

1. HTTP 헤더

* From : 사용자의 이메일 주소를 포함
* User-Agent : 사용자가 쓰고 있는 브라우저의 이름과 버전 정보 등을 포함
* Referer : 사용자가 현재 페이지로 유입하게 된 웹 페이지의 URL

2. IP 주소

* 웹 서버는 HTTP 요청을 보내는 반대쪽 TCP 커넥션의 IP 주소를 알아낼 수 있다.

3. 사용자 로그인

* Authenticate(인증), Authorization(권한 부여)헤더

4. 뚱뚱한 URL

* 사용자의 상태 정보를 URL 경로의 처음이나 끝에 추가하여 식별

5. 쿠키

## PUT과 PATCH의 차이에 대해서 설명해주세요.

### PUT

> 리소스가 있다면 대체, 리소스가 없다면 생성하는 HTTP 메서드

<figure><img src="../../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

서버에 이미 리소스가 있는 경우, 값을 대체해버리는 HTTP 메서드입니다.

* 서버에 리소스가 없는 경우, 주어진 값을 기반으로 리소스 생성

<figure><img src="../../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

또한 클라이언트가 리소스 위치를 알고 URI 지정을 수행합니다.

* POST와의 차이점

<figure><img src="../../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

### PATCH

> 리소스를 부분적으로 변경하는 HTTP 메서드

<figure><img src="../../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

* PATCH는 리소스가 없는 경우에도 생성하지 않으며 PK로 찾아와서 리소스를 수정하기 위해 리소스가 없다면 예외를 던집니다.

> [https://www.inflearn.com/questions/290847/patch-%EC%A7%88%EB%AC%B8-%EB%A6%AC%EC%86%8C%EC%8A%A4%EA%B0%80-%EC%97%86%EB%8B%A4%EB%A9%B4](https://www.inflearn.com/questions/290847/patch-%EC%A7%88%EB%AC%B8-%EB%A6%AC%EC%86%8C%EC%8A%A4%EA%B0%80-%EC%97%86%EB%8B%A4%EB%A9%B4)

* 수정을 하기 위해서는 고유 식별자(PK, EMAIL)등의 아이디가 필요하며, 이를 통해 찾아오는 과정에서 자원이 없다면 404 NOT\_FOUND가 발생\
  그런데 그 과정에서 생성 로직이 있는건 로직을 이상하게 짠 것

## DNS

> [https://codybuilder.com/35](https://codybuilder.com/35)

### DNS Round Robin에 대해 설명해주세요.

<figure><img src="../../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

> DNS의 구성 방식 중 하나로서, Domain에 대한 IP 요청 쿼리 시 RR(Round-Robin) 방식으로 IP를 반환

선점형 스케줄링의 하나로서, 프로세스들 사이에 우선순위를 두지 않고 시간 단위(Time Quantum / Slice)의 순서대로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘입니다.

1. 각 프로세스에 일정 시간을 할당
2. 할당된 시간이 지나면 그 프로세스는 잠시 보류한 뒤, 다른 프로세스에게 기회를 줌
3. 다음 프로세스에게 반복..

자동적으로 시간에 따라서 스케줄링이 변환되기 때문에 로드 밸런서가 필요 없다는 장점이 있지만, 서버의 수 만큼 공인 IP가 필요하다는 점 등의 단점이 존재합니다.

### DNS Resolver에 대해 설명해주세요.

> DNS Resolver는 사용자의 컴퓨터나 네트워크에 위치한 DNS Client

<figure><img src="../../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

사용자가 도메인 이름(Domain Name)을 입력하면, 해당 도메인 이름을 IP 주소로 변환하기 위해 DNS 서버에 요청하여 질문하는 역할을 합니다.

### DNS 재귀적 질의와 반복적 질의에 대해 설명해주세요.

> 실제로는 DNS Local 서버 대신 Resolver가 수행

1. Iterative Query (반복적 질의)

> IP 주소를 얻을 때까지 요청과 응답을 반환하는 과정을 반복하는 DNS 질의 방법

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

* (1) 사용자가 Local DNS 서버에 쿼리를 보냄
* (2) Local DNS 서버가 Root Name 서버에 쿼리를 보내 TLD 서버의 주소를 반환받음
* (3) 이후, 다시 TLD 서버에 쿼리를 보내며 최종 IP 주소를 받을 때까지 요청과 응답을 반환

2. 재귀적 질의 (Recursive Query)

> 실제 도메인 정보를 가지고 있는 서버까지 쿼리가 이동하여 IP 주소를 얻는 방법

<figure><img src="../../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

* (1) 사용자가 Local DNS 서버에 쿼리를 보냄
* (2) Local DNS 서버가 Root Name 서버에 쿼리를 보냄
* (3) Root Name 서버는 자신의 서버에 주소가 없을 경우, 해당 TLD 서버에 요청

### DNS에서 사용되는 프로토콜은 어떤 걸까요?

> [https://steady-coding.tistory.com/523](https://steady-coding.tistory.com/523)

1. UDP

> DNS 서버에 질의를 보낼 때 사용

* 512 Byte 이하의 데이터를 전송할 경우 사용
  * UDP 메시지는 512 Byte 까지만 지원하기에 작은 데이터 전송에 적합

2. TCP

> Zone Transfer (영역 전송)과 512 Byte를 초과하는 패킷을 전송할 경우 사용

* 영역 데이터 및 데이터를 요청한 다른 DNS 서버로 전체 영역을 전송하여 영역 데이터를 보낼 경우 사용
  * DNS 영역 데이터베이스는 일관성이 있어야 함
* 512 Byte를 넘는 데이터를 전송할 경우 사용
  * 큰 데이터를 전송할 경우에 적합
* 클라이언트가 DNS에서 응답을 받지 못하는 경우, TCP를 통해 데이터를 다시 전송하기 위해 사용

## 멱등성에 대해 설명해주세요.

> 한 번 호출하든 두 번 호출하든 몇 번을 호출하든 결과가 똑같은 것을 의미합니다.

<figure><img src="../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

멱등성의 개념을 기반으로 HTTP 메서드를 표현하자면 다음과 같습니다.

1. GET : 결과를 조회한다. 몇 번을 조회하든 같은 결과가 조회
2. POST : 결과를 생성한다. 두 번 호출하면 같은 결제가 중복해서 발생할 수 있으므로 멱등이 아님
3. PUT : 결과를 대체한다.  따라서 같은 요청을 여러 번 해도 최종 결과는 동일
4. DELET : 결과를 삭제한다. 같은 요청을 여러 번 해도 삭제된 결과는 동일

