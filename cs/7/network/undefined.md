# 최지율

## 🍪 쿠키와 세션

### 쿠키와 세션을 사용하는이유

* HTTP는 비연결성으로 서버에 요청을 전송할 때에는 항상서버와의 새 연결을 진행해야 합니다.
* 따라서 이전 요청에 대해서는 새로운 연결 요청이 이루어지게 되므로 기억하고 있지 않습니다.
* 그걸 위해 쿠키나 세션을 사용하게 되면 이전 요청에서의 정보를 기억하여 사용자는 로그인 등을 다시 진행하지 않고 해당 정보 등을 함께 전송하여 매 요청마다 번거로운 작업을 피하고 더 유용하게 사이트를 활용 할 수 있게 됩니다.

### 세션의 저장 방식

* **서버 메모리 기반 세션 저장:**
  * 세션 데이터를 서버의 메모리에 저장합니다.
  * 빠른 응답 시간을 제공하지만 서버 재시작 시 세션 데이터가 손실됩니다.
  * 단일 서버 환경에서 사용 할 수 있는 방법입니다.
* **데이터베이스 기반 세션 저장:**
  * 세션 데이터를 데이터베이스에 저장합니다.
  * 서버 재시작에도 세션 데이터를 보존할 수 있습니다.
  * 더 안정적이지만 데이터베이스에 대한 I/O 비용이 추가될 수 있습니다.
  * 다중 서버 간 세션 공유가 가능합니다.
* **캐시 기반 세션 저장:**
  * 세션 데이터를 캐시에 저장합니다.
  * Redis 또는 Memcached와 같은 분산 캐싱 시스템을 활용할 수 있습니다.
  * 빠른 응답과 영속성을 동시에 제공합니다.
  * 다중 서버 간 세션 공유가 가능합니다.

### 세션이 날아가면 어떻게 해야할까?

* 우선 세션 정보가 없다면 공통적으로 재 로그인을 통해 세션을 다시 발급을 받는 로직을 작성을 해야할 것 같습니다.
* 이 때 사용자에게 "일시 장애를 통한 오류로 재 로그인을 부탁드린다"는 메시지 등을 잘 전달하여 사용자 경험을 향상 시키고
* 해당 예외발생 로직에 로깅을 하고 로그를 통해 발생한 경위를 최대한 추적하여 반복하여 발생하지 않도록 막는 것이 중요할 것 같습니다.

### 쿠키의 단점에 대해 설명해주세요

* **보안상의 위험**
  * 쿠키의 대표적인 단점으로는 저장위치가 클라이언트(사용자 로컬)에 위치하여 보안상의 위험이 존재한다는 것입니다. 이는 쿠키에개인적인 정보 등 보안에 위험한 데이터를 포함한다면 악성 사용자가 정보를 수정 및 탈취하여 위협을 할 수 있습니다.
* **쿠키 크기 제한**
  * 브라우저마다 다르지만 보통 최대 4KB로 많은 양의 데이터를 쿠키에 저장 할 수 없습니다.
* **성능 저하**
  * 또한 쿠키의 양이 많고 매 요청마다 모든 쿠키가 전송된다면 성능저하 문제도 발생 할 수 있습니다.

### 쿠키와 세션 차이점

#### 쿠키

* 쿠키는 클라이언트의 로컬에 저장이 되어 참조되며 키와 값으로 저장되는 작은 데이터 파일입니다.
* 사용자 인증이 유효한 시간을 명시할 수 있고 유효 시간이 경과되면 삭제됩니다.
* 유효시간이 경과 되지 않았으면 브라우저가 종료 되더라도 유지가 됩니다.
* 주로 사용자 설정 및 추적 정보(행동, 검색 기록 등) 등을 저장하는데 사용됩니다.

#### 세션

* 세션은 서버측에 저장이 되고 클라이언트에는 세션을 식별할 수 있는 ID만 가지고 있습니다.
* 서버에 저장되므로 쿠키에 비해선 보안에 덜 취약하지만 세션ID를 안전하게 관리하지 못하면 세션 역시 보안 문제가 발생 할 수 있습니다.
* 쿠키에 비해 세션은 서버 메모리에 저장하므로 더 많은 데이터를 저장할 수는 있습니다.
* 일반적으로 브라우저를 닫거나 일정 시간 동안 활동이 없을 경우 소멸됩니다.
* 주로 세션을 사용하는 동안 유지되어야 하는 사용자 로그인 정보 등을 저장합니다.

### 쿠키와 세션을 조작할 수 있는데 어떻게 해야할까

* 주로 쿠키와 세션을 조작하는 것은 악의적인 사용자가 보안을 위협하는 행위로 간주됩니다.
* 그러므로 쿠키에는 중요한 데이터 정보를 담지 않아야 합니다.
* 세션키도 탈취 당할 가능성이 있으므로 서버에서 세션에 민감한 정보를 저장하지 않아야 하며,
* 세션의 유효시간을 적절하게 설정해주어야 하고
* 쿠키와 세션의 사용에 대한 최소한의 필요성만 고려하여 설계해야 합니다.

## 🪙 JWT

### JWT를 사용하는 이유

* 서버의 상태를 저장하지 않고도 여러 서비스 간에 사용자 인증을 활용할 수 있어 효율적인 분산 시스템에서 유용합니다.
* 모바일 애플리케이션 및 웹 애플리케이션 등 다양한 도메인에서 JWT를 통해 안전하게 정보를 교환 할 수 있습니다.
* 간결하고 경량적인 형식으로 표현이 가능해서 HTTP 헤더에 포함하여 쉽게 전송 가능합니다.
* 클레임(claim)이라는 정보를 포함 할 수 있어서 사용자의 식별 정보, 권한 등을 토큰에 포함 시켜 전달 할 수 있습니다.

### JWT의 단점

* 토큰이 한번 발급되면 변경이 불가능 하므로 내용을 변경하거나 취소하려면 서버에서 토큰을 검증하고 갱신하는 방법이 필요합니다.
* 토큰은 클레임, 서명 등 많은 정보를 포함하므로 상당한 크기가 될수 있어 매 요청에 포함될 경우 네트워크 부하를 유발 할 수 도 있습니다.
* 토큰이 탈취 당하거나 XSS 등의 스크립트 공격에 취약할 수 있습니다.