# 고청천

## HTTP/HTTPS

### 멱등성이란?

멱등성은 연산을 여러 번 해도 결과가 달라지지 않는 성질을 말합니다

즉, 동일한 요청을 한 번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때 해당 HTTP 메서드가 멱등성을 가졌다고 이야기 합니다!

멱등성을 가지는 메서드로는 GET, HEAD, DELETE 가 있습니다

### TCP, HTTP 차이

[https://hwan-shell.tistory.com/271](https://hwan-shell.tistory.com/271)

TCP 는 전송 계층 프로토콜이고 HTTP 는 응용 계층 프로토콜입니다 TCP의 경우 두 앤드 포인트의 논리적 연결을 담당하고 HTTP는 실제 전송할 데이터들을 다룹니다 물론 HTTP 3 이전 까지 HTTP 는 TCP 기반 프로토콜 위에 동작합니다

### HTTPS 사용 시 공인 인증서 발급 과정

[https://nuritech.tistory.com/25](https://nuritech.tistory.com/25)

서버에 공개키와 비밀키를 생성하고 인증서를 발급받기위해 CA 에 공개키와 서버 정보를 전달합니다CA는 서버로 부터 받은 정보를 담아 SSL 인증서를 발급합니다 그리고 만든 인증서는 암호화하키위해 CA 역시 공개키 비밀키를 만들고 비밀키로 인증서를 암호화 합니다. 이렇게 암호화된 SSL 인증서를 서버에 전달합니다

### CA 란?

[http://www.ktword.co.kr/test/view/view.php?m\_temp1=2123](http://www.ktword.co.kr/test/view/view.php?m\_temp1=2123)

SSL 보안 인증서를 발급하는 기관들을 말합니다&#x20;

```
CA ( Authority, 인증기관)

  ㅇ 을 이용한  등을 함에 있어서,
     - 누구나가 객관적으로 신뢰할 수 있는 제3자(Trusted Third Party)를 의미함

  ㅇ 즉,  및 를 위한 를 발급,관리하는, 서비스 제공 기관/를 말함
  - 출처 : [정보통신기술용어해설]
```

이러한 인증 기관은 계층적 구조를 지닙니다

### 패킷 순서가 섞이면 어떻게 처리되나요

패킷의 순서가 섞이게 되면 수신 측에서 재조립됩니다 패킷이 분할 될 때 IP 헤더의 Identification, Fragments offset 값이 기록되는 데 이 정보를 바탕으로 수신측이 재조립할 수 있습니다&#x20;

\


<figure><img src="https://blog.kakaocdn.net/dn/bbga3G/btsd4c0NbyK/yxJ81tgToptrQHEU8D8OdK/img.png" alt=""><figcaption></figcaption></figure>

* Identification : 같은 패킷에서 분할되었다는 것을 알기위한 식별자
* Don't Fragment : 분할된 패킷에는 0, 분할되지 않은 패킷에는 1 부여
* More Fragment : 분할된 패킷들 중에 마지막이 아닌 패킷들은 1이 부여되고, 마지막 패킷(조립할게 더 없을 때) 이거나 분할되지 않은 패킷일 경우에 0이 부여된다.
* Fragemnt offset : 분할된 패킷이 분할되기 전에 패킷에서 상대위치를 나타낸다. 조립 과정에서 이 값을 보고 조립의 순서를 결정한다.

### 3WayHandShake, 4WayHandShake 차이

먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

<figure><img src="../../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

3-way Handshake의 과정을 살펴보자.

1. 클라이언트는 서버에 접속을 요청하는 SYN 패킷을 보낸다. 이 때 클라이언트는 SYN을 보내고 SYN/ACK 응답을 기다리는 SYN\_SENT 상태가 된다.
2. 서버는 SYN 요청을 받고 클라이언트에게 요청을 수락한다는 ACK와 SYN Flag가 설정된 패킷을 발송하고 클라이언트가 다시 ACK로 응답하기를 기다린다. 이 때 서버는 SYN\_RECEIVED 상태가 된다.
3. 클라이언트는 서버에게 ACK를 보낸 후부터는 연결이 이루어지고 데이터가 오가게 되는 것이다. 이 때 서버 상태는 ESTABLISHED가 된다.

<figure><img src="../../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>



연결을 해제하기 위한 과정을 의미합니다

4-way Handshake 과정을 살펴보자.

1. 클라이언트가 연결을 종료하겠다는 FIN Flag를 전송한다.
2. 서버에서는 FIN 패킷을 정상적으로 받았다는 ACK를 클라이언트에 전송해준다. 그 후 서버는 CLOSE-WAIT 상태로 빠져든다.
3. 연결을 종료한 후 서버는 클라이언트에게 FIN Flag를 전송해준다.
4. 서버로부터 전송된 FIN Flag를 받은 클라이언트는 확인을 알리는 ACK를 서버로 전송한 후, 일정 시간동안 TIME-WAIT 상태에 빠지게 된다.
5. 클라이언트로 부터 ACK를 받은 서버는 소켓을 Close하고 두 TCP간의 세션이 종료된다.
6. TIME-WAIT에 빠진 클라이언트는 서버로 부터 FIN을 수신하더라도, 일정시간동안 세션을 유지하며 도착하지 않은 패킷을 기다린다.

### HTTP와 HTTPS 차이

둘다 서버 클라이언트가 정보를 주고 받기 위한 프로토콜입니다

HTTP 는TCP/IP 위에서 작동한다. HTTP는 상태를 가지고 있지 않는 Stateless 프로토콜이며 Method, Path, Version, Headers, Body 등으로 구성됩니다&#x20;



HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이라서, HTTP로 비밀번호나 주민등록번호 등을 주고 받으면 제3자가 정보를 조회할 수 있었다. 그리고 이러한 문제를 해결하기 위해 HTTPS 가 등장하게 되었습니다



HTTPS는 대칭키 암호화와 비대칭키 암호화를 모두 사용하여 빠른 연산 속도와 안정성 모두 취합니다

HTTPS 연결 과정(Hand-Shaking)에서는 먼저 서버와 클라이언트 간에 세션키를 교환하는 데 이때 교환하는 키가  데이터를      암호화 사용되는 대칭키입니다

### REST API란?

### Keep-Alive 와 Non-Persistent 차이
