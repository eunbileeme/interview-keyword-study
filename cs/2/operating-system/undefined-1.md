# 이은비

## :desktop: 운영체제란 무엇인가요?

### 운영체제의 역할이 무엇이 있을까요?

운영체제의 역할은 다음과 같이 구분할 수 있습니다.

1. 프로세스 관리 : 운영체제에서 작동하는 응용 프로그램을 관리

> CPU를 점유할 프로세스를 정한 후, 프로세스 간 공유 자원 접근과 통신 등을 관리

2. 저장장치 관리

> 1차 저장 장치에 해당하는 메인 메모리와 2차 저장 장치에 해당하는 하드 디스크 등을 관리

3. 네트워킹

> TCP/IP 기반의 인터넷에 연결 및 네트워크 프로토콜을 지원 (이때, DNS도 지원)

4. 사용자 관리

> 각 계정을 관리 및 파일 등에 접근 권한을 지정

5. 디바이스 드라이버

> 입출력 장치와 같은 하드웨어 장치와 상호 작용하기 위해 만들어진 시스템인 디바이스 드라이버를 관리



### 커널이란?

<figure><img src="../../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

커널이란

* 자원에 접근하고 조작하는 기능
* 프로그램이 올바르고 안전하게 실행되게 하는 기능

위와같은 운영체제의 핵심 서비스를 담당하고 있는 개념입니다.

또한, 다음과 같은 종류로 나뉘어집니다.

1. 일체형 커널

> 운영체제의 모든 서비스가 커널 내에 포함된 커널

2. 마이크로 커널

> 운영체제 요소의 대부분을 커널 외부로 분리하여 최소한의 요소(메모리 관리, 프로세스 간 통신 등)만 남긴 커널



## 컴퓨터 구조

### CPU의 구조

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

1. ALU(Arithmetic Logic Unit)

> 명령어를 실행하기 위한 마이크로 연산(산술,  논리  등)을 수행하는 장치

2. CU(Control Unit)

> 제어 장치로서 컴퓨터 시스템의 작동을 통제하고 지시하는 장치
>
> ex. 기억 장치로부터 프로그램 명령을 순차적으로 꺼내 해독하고, 해석에 따라서 명령어 실행에 필요한 제어 신호를 기억 장치 / 연산 장치 / 입출력 장치 등으로 전송

3. Register

> CPU 내에 있는 소규모의 고속 기억 장치



### 인터럽트란?

> 프로그램 실행 도중 예기치 않은 상황이 발생했을 경우, 현재 실행 중인 작업을 즉시 중단하여 CPU에게 알리는 것

인터럽트는 다음과 같이 나눌 수 있습니다.

1. 외부 인터럽트

> 입출력 장치, 전원 등 외부적인 요인으로 발생하는 인터럽트

2. 내부 인터럽트

> Overflow, Exception 등과 같은 현상이 발생하는 인터럽트

3. 소프트웨어 인터럽트

> 프로그램 처리 중 명령 요청에 의해 발생하는 인터럽트



### Cache의 지역성

Cache의 지역성은 다음과 같이 2가지로 나눌 수 있습니다.

1. 시간적 지역성

> 최근에 참조된 주소의 내용은 다음에 다시 참조된다는 특성

2. 공간적 지역성

> 기억 장치 내에 서로 인접하여 저장되어 있는 데이터들이 연속적으로 액세스 될 가능성이 높아진다는 특성



#### CPU가 Cache를 삭제하는 알고리즘

* LRU(Least Recently Used)

> Cache에서 작업을 제거할 때, 가장 오랫동안 사용하지 않은 데이터를 제거

> Doubled Linked List를 통해 구현할 수 있으며 Head에 가까울수록 최근에 사용된 데이터, Tail에 가까울수록 오랫동안 사용되지 않은 데이터로 간주



## 커널 모드

### 유저 모드와 커널 모드

#### 커널 모드의 보안상 이점

운영체제 내부에서는 컴퓨터가 CPU를 사용하는 방법에 따라 유저 모드와 커널 모드로 컴퓨터를 제어하게 됩니다.

<figure><img src="../../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

과정은 다음과 같습니다.

1. 운영체제가 유저 모드로 컴퓨터를 제어
2. 사용자 입력 시, 시스템 콜이 호출되며 커널 모드로 전환 (trap mode bit = 0)
3. 요청을 통해 알맞은 작업 수행
4. 유저 모드로 전환 (return mode bit = 1)

그리고 유저 모드와 커널 모드는 다음과 같습니다.

1. 유저 모드

> 사용자와 데이터를 주고 받는 작업 수행

2. 커널 모드

> 운영체제 내부에서 실제로 하드웨어를 제어하는 작업 수행

> 운영체제가 CPU의 제어권을 가지고 모든 종류의 명령을 다 수행하는 모드

따라서 시스템에 중요한 영향을 미치는 연산을 커널 모드에서만 실행하도록 함으로써 하드웨어의 보안을 유지할 수 있다는 이점이 있습니다.



### 시스템 콜

커널모드는 운영체제 내부에서 실제로 하드웨어를 작업하는 작업을 수행하기 때문에, 기본적으로 커널 모드에서 수행하게 됩니다.

또한, 다음과 같은 상황에 대비해 시스템 콜은 커널 모드에서 처리하게 됩니다.

1. 프로그램 다운로드
2. 이때, 비정상적인 프로그램이라고 가정
3. 프로그램이 사용자의 컴퓨터에 접근할 때, 다른 프로그램이 사용하는 주소 공간을 침범하는 등의 좋지 않은 결과 초래
4. 이러한 결과를 막기 위해 유저 모드에서 요청 후, 커널 모드에서 처리



### DMA(Direct Memory Access)

<figure><img src="../../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

> CPU의 개입 없이 직접적으로 I/O 장치와 기억 장치 사이의 데이터를 전송하는 접근 방식

기존의 인터럽트 방식은 CPU에 인터럽트 신호를 전송하기 때문에, CPU의 가용 시간을 소요하게 됩니다.

따라서 CPU가 하는 일을 직접적으로 DMA Controller가 수행하게 함으로써 입출력 장치 간 데이터를 주고 받는 과정이 끝난 후에만, 인터럽트가 발생하므로 CPU의 부하를 줄일 수 있다는 이점이 있습니다.



## 컴파일러와 인터프리터

컴파일러와 인터프리터는 다음과 같이 분류할 수 있습니다.

1. 컴파일러 (Compiler)

> 프로그램 전체를 스캔하여 모두 기계어로 번역하는 개념

> 초기 스캔 시간은 오래 걸리지만 전체 실행 시간은 인터프리터보다 적은 시간을 소요

* C, C++, Java

2. 인터프리터 (Interpreter)

> 프로그램 실행 시, 한 번에 한 문장씩 기계어로 번역하는 개념

> 컴파일러보다 전체 실행 시간이 오래 걸리는 편

* Python, Javascript, Java