# 김의진

## 대칭키/비대칭키

### 대칭키/비대칭키란?

* 대칭키 : 암호화와 복호화에 사용하는 키가 동일하다.
* 비대칭키(공개키) : 암호화와 복호화에 사용하는 키가 서로 다르다.
  * 비대칭키 암호화에서는 송수신자가 모두 한상의 키(개인키, 공개키)를 갖고 있게 된다.

### 성능 상 이점을 누리기 위해 사용하는 방법

대칭키는 연산이 덜 복잡하여 빠르게 처리될 수 있고, 비대칭키는 키 교환을 안전하게 할 수 있으며 신원을 확인하는데 유용하다.

대칭키와 비대칭키의 성능 상 이점을 누리기 위해 함께 사용하는 방식이 있고 다음과 같이 진행된다.

1. 키 교환 : 비대칭키 암호화를 이용하여 안전하게 대칭키를 교환한다.
2. 데이터 암호화 및 전송 : 교화된 대칭키를 사용하여 전송할 데이터를 암호화한다.
3. 데이터 수신 및 복호화 : 데이터 수신자는 교환된 대칭키를 사용하여 암호화된 데이터를 복호화한다.

SSL/TLS와 같이 네트워크 상에서 안전하게 통신하기 위해 사용되는 프로토콜의 경우, 이와 같은 암호화 방식이 사용된다.

## IP

### IPv6가 나오게 된 배경

* IP주소에 대한 수요가 증가하면서 주소길이가 32bit인 IPv4의 주소체계로는 처리가 어렵게 되었다. IPv6는 주소길이를 128bit로 확장했다.

## 공인 IP/사설 IP

### 공인 IP와 사설 IP란?

* 공인 IP
  * 외부에 공개되어 있는 IP주소이다.
  * 인터넷 사용자의 로컬 네트워크를 식별하기 위해 ISP(인터넷 서비스 공급자)가 제공하는 IP 주소이다.
  * 공인 IP는 전세계에서 유일한 IP 주소를 갖는다.
* 사설 IP
  * 일반 가정이나 회사 내 등에 할당된 네트워크 IP 주소

|       | 공인 IP (Public IP) | 사설 IP (Private IP) |
| ----- | ----------------- | ------------------ |
| 할당 주체 | ISP(인터넷 서비스 공급자)  | 라우터(공유기)           |
| 할당 대상 | 개인 또는 회사의 서버(라우터) | 개인 또는 회사의 기기       |
| 고유성   | 인터넷 상에서 유일한 주소    | 하나의 네트워크 안에서 유일    |
| 공개 여부 | 내/외부 접근 가능        | 외부 접근 불가능          |

### 사설 IP를 사용하는 이유

IPv4 체계에서 공인 IP 주소만으로는 부족하다. 사설 IP 주소 사용을 통해 IP주소를 절약하기 위해 사용하고 있다.

### 서브넷마스크를 사용하는 이유

서브넷 마스크는 IP주소에서 네트워크 주소와 호스트 주소를 구분하기 위해 사용된다. 네트워크의 규모가 더 클수록 네트워크를 관리하고 유지하기 어려워진다. 하나의 큰 네트워크를 여러 개의 작은 서브넷으로 나눌 수 있어 네트워크를 조직화하고 관리할 수 있다. 또한 네트워크 성능을 개선하고 자원을 효율적으로 분배할 수 있다.

## 종단 암호화(엔드투엔드 암호화)

* 모든 사용자로부터 메시지를 비공개로 유지하는 메시징 유형
* 중간에서 메시지를 해독하는 것이 불가능하다.
* 비대칭키(공개키) 암호화를 사용한다.
  * 두 당사자가 안전하지 않은 채널을 통해 비밀 키를 전송하지 않고도 통신할 수 있다.

\[참고자료]

* [cloudfare 엔드투엔드 암호화](https://www.cloudflare.com/ko-kr/learning/privacy/what-is-end-to-end-encryption/)

## Docker

컨테이너 기술을 사용하여 애플리케이션을 격리된 환경에서 실행할 수 있게 하는 도구

* 컨테이너화된 소프트웨어는 리눅스와 윈도우 기반 애플리케이션에서 모두 사용가능하며 인프라에 관계 없이 동일하게 실행된다.
* 컨테이너는 소프트웨어를 소프트웨어가 속한 환경으로부터 격리시켜 개발 환경과 스테이징 환경과 같이 서로 다른 환경에서도 일관되게 작동하도록 보장한다.

### Docker 동작과정

* 컨테이너 :
  * 코드와 의존성을 모두 포함한다.
  * 하나의 컴퓨팅 환경에서 다른 환경으로 응용프로그램이 빠르고 안정적으로 실행될 수 있도록 하는 표준 소프트웨어 단위
  * 동일한 컨테이너는 어디서 혹은 누구에 의해 실행되느냐에 관계 없이 항상 동일한 애플리케이션과 실행 동작을 제공한다.
* 이미지 :
  * Docker 컨테이너 이미지는 애플리케이션을 실행시키는데 필요한 모든 것을 포함하는 독립적이고, 실행가능한 경량 소프트웨어 패키지이다.
  * 코드, 런타임, 시스템 도구, 시스템 관련 라이브러리와 설정이 포함된다.
  * Docker 컨테이너의 경우 컨테이너 이미지는 런타임에 컨테이너가 된다.
    * Docker 엔진을 실행시킬 때 이미지가 컨테이너가 된다.
* 컨테이너 간 통신 :
  * 네트워크를 통해 컨테이너가 서로 통신하거나 외부 네트워크에 접근할 수 있게 해준다.
* 볼륨 관리 :
  * 데이터를 영구적으로 저장
  * 컨테이너가 데이터를 저장하고 다른 컨테이너와 공유할 수 있게 해준다.

## CDN

* CDN은 정적 콘텐츠를 전송하는데 쓰이는 지리적으로 분산된 서버의 네트워크이다.
* 이미지, 비디오, CSS, JavaScript 파일 등을 캐시할 수 있다.

### CDN의 동작

* 사용자가 웹사이트를 방문하면, 그 사용자에게 가장 가까운 CDN 서버가 정적 콘텐츠를 전달하게 된다.
* 사용자가 CDN 서버로부터 멀면 멀수록 웹사이트는 천천히 로드된다.
* DNS는 해상 사이트의 주소를 입력하면 서버의 IP가 아닌 CDN 주소로 연결해준다. 그렇게 대신 요청을 받은 CDN은 이제 세계 각지의 서버들(Edge)들 중에서 클라이언트에게 가장 빠르게 서비스를 제공할 수 있는 엣지를 선택해서 그 서버와 사용자를 주선해주게된다. **반드시 물리적으로 가까운 엣지는 아니다.** 지역에 따라 **트래픽**이 몰리는 곳도 있기 때문에 이를 고려하기도 한다. 이처럼 좋은 CDN은 각 엣지들이 잘 동작하는지를 꾸준히 헬스체크해서 안좋은 엣지를 걸러내기도 한다.