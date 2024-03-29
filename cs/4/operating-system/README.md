# Operating system

## 운영체제란 무엇인가요?

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

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

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

CPU의 구조는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

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



### DMA(Direct Memory Access)

> CPU의 개입 없이 직접적으로 I/O 장치와 기억 장치 사이의 데이터를 전송하는 접근 방식

<figure><img src="../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

기존의 인터럽트 방식은 CPU에 인터럽트 신호를 전송하기 때문에, CPU의 가용 시간을 소요하게 됩니다.

따라서 CPU가 하는 일을 직접적으로 DMA Controller가 수행하게 함으로써 입출력 장치 간 데이터를 주고 받는 과정이 끝난 후에만, 인터럽트가 발생하므로 CPU의 부하를 줄일 수 있다는 이점이 있습니다.



## fork()

### fork()를 수행하는 이유?

> 다른 프로그램을 실행하는 새 프로세스를 만들기 위해 사용합니다.

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

* fork()는 system call을 호출한 프로세스를 복사합니다.
  * 기존 프로세스에서 복사된 내용(Stack, Heap, Data, PCB)을 새로운 프로세스 공간에 복사합니다.
* 따라서 기존 프로세스는 부모 프로세스가 되고 새로운 프로세스는 자식 프로세스가 됩니다.
  * 이때, 텍스트(code)는 공유합니다.
* 결과적으로 복사한 자식 프로세스는 동일한 데이터들을 **참조** 만 하고 있는 상태이기 때문에, 실제 메모리 상에서는 같은 걸 복사하고 있진 않습니다.
  * 별개의 가상 메모리이지만, 물리 메모리에서는 같은 frame(영역)을 참조하고 있습니다.
    * [https://velog.io/@jungbumwoo/fork-%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90](https://velog.io/@jungbumwoo/fork-%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

<figure><img src="broken-reference" alt=""><figcaption><p>COW - 프로세스1에서 fork()가 호출된 후</p></figcaption></figure>

<figure><img src="broken-reference" alt=""><figcaption><p>COW - 프로세스1의 특정 페이지에서 수정이 발생한 경우</p></figcaption></figure>

### fork()란?

> fork()란 프로세스를 복사하여 자식 프로세스를 생성하는 시스템 콜로서, 같은 프로그램에서 새로운 제어 흐름을 만드는 용도 또는 다른 프로그램을 실행하는 새 프로세스를 만드는 용도로서 사용합니다.

### fork() 명령어 장점

비용 측면에서 장점이 있습니다.

* fork()를 통해 페이지 테이블 자체를 복사했으므로 두 프로세스에서 변수가 갖는 페이지 테이블 주소(상대 주소)  자체는 동일하게 유지됩니다.
  * 초기에는 두 상대 주소가 갖는 물리 메모리 주소도 동일
  * 이후, 한 프로세스의 변수에서 변경이 일어나면 해당 프로세스의 페이지 테이블은 새로운 물리 메모리 주소를 가리킵니다. (COW)
    * 변경된 변수에 대해서 두 페이지 테이블이 실제로 가리키는 물리 메모리 주소는 달라졌지만, 페이지 테이블의 주소는 동일한 상태
* fork()의 비용 = 부모 프로세스의 페이지 테이블을 복사하는 비용 + 자식 프로세스를 기술하기 위한 PCB를 할당받는 비용



결과적으로 fork()의 장점 = 즉, fork()를 사용하는 이유는 다음과 같습니다.

1. 프로세스를 새로생성하는 것보다 복제라는 비교적 간단한 작업
2. COW로 인해 복제의 비용이 거의 들지 않음



## IPC

### IPC란?

> IPC(Inter Process Communication)란 프로세스 간 통신으로서 프로세스들끼리 서로 데이터를 주고 받는 행위 또는 방법을 일컫는 개념입니다.

* 프로세스는 완전히 독립된 실행 객체입니다.
*   커널 영역에서 IPC라는 내부 프로세스 간 통신을 제공하여 프로세스 간 통신을 진행합니다.



종류는 다음과 같이 2가지가 존재합니다.

<figure><img src="broken-reference" alt=""><figcaption><p>(A) Meaage Passing, (B) Shared Memory</p></figcaption></figure>

### IPC 중 공유 메모리에서 한 번 공유된 이후에 커널의 도움이 필요한지?

> 공유 메모리는 여러 프로세스가 동시에 접근할 수 있는 메모리 영역으로서, 공유 메모리의 접근에는 커널이 필요하지 않습니다.

* [https://dokhakdubini.tistory.com/490](https://dokhakdubini.tistory.com/490)



## PCB

### PCB란?

> PCB(Process Control Block)이란 운영체제가 프로세스를 제어하기 위해 정보를 저장하는 공간으로서, 프로세스의 상태 정보를 저장하는 구조체입니다.

* 프로세스의 상태 관리 및 Context switching (문맥 교환)을 위해 필요합니다.
* 프로세스 생성 시 만들어지며, 주 기억장치에 저장됩니다.
  * [https://junsangkwon.tistory.com/45](https://junsangkwon.tistory.com/45)

<figure><img src="broken-reference" alt=""><figcaption><p>PCB (Process Control Block)</p></figcaption></figure>

정리하자면 다음과 같습니다.

* **PCB**란운영체제라는 프로그램이 프로세스를 만들어서 실행을 하기 위해, 프로세스와 관련된 데이터 구조 - 즉 필요한 데이터들을 저장하고 있는 저장소입니다.
  * [https://dev-mystory.tistory.com/119](https://dev-mystory.tistory.com/119)

### PCB는 어디에 저장되고 있을까요?

> PCB는 프로세스의 중요한 정보를 포함하고 있기 때문에, 일반 사용자가 접근하지 못하도록 보호된 메모리 영역에 위치해있습니다.

* 일부 운영체제에서는 커널 스택의 처음 부분에 위치합니다.
  * [https://vicente-blog.com/blog/68/](https://vicente-blog.com/blog/68/)

## 프로세스 동기화

### 뮤텍스(Mutex)

> 임계 구역(Critical Section)을 가진 스레드들의 실행 시간이 서로 겹치지 않고, 각각 단독으로 실행(상호 배제, Mutual Exclution)되도록 하는 기술입니다.

IPC 통신에서 프로세스 간 데이터를 동기화하고 보호하기 위해 사용하는 알고리즘입니다.

* &#x20;Mutual Exclution의 약자이기도 합니다.

<figure><img src="broken-reference" alt=""><figcaption><p>Mutex</p></figcaption></figure>

* 다중 프로세스들의 공유 리소스에 대한 접근을 조율하기 위해 동기화(Synchronization) 또는 락(Lock)을 사용합니다.
  * 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없습니다.

사용하는 lock과 unlock은 다음과 같습니다.

1. lock : 현재 임계 구역에 들어갈 권한을 얻습니다.

* 다른 프로세스 또는 스레드가 임계 구역을 수행 중이라면 종료할 때까지 대기합니다.

2. unlock : 현재 임계 구역을 모두 사용했음을 알립니다.

* 대기 중인 다른 프로세스 또는 스레드가 임계 구역에 진입할 수 있습니다.
  * [https://chelseashin.tistory.com/40](https://chelseashin.tistory.com/40)



## 메모리

### 가상 메모리 왜 사용할까

> 운영체제가 프로그램에게 가상 메모리를 할당함으로써 프로그램이 종료되어도, 운영체제가 해당 프로그램의 주소를 알기 때문에 자원을 회수할 수 있기에 메모리 낭비가 없다는 장점이 있습니다.

* [https://dding9code.tistory.com/100](https://dding9code.tistory.com/100)

### 내부 단편화와 외부 단편화의 차이점에 대해 설명해주세요

> 내부 단편화는 필요한 양보다 더 큰 메모리가 할당되어서 프로세스 내 메모리 공간이 낭비되는 반면, 외부 단편화는 사용하지 않는 메모리가 곳곳에 존재하여 총 메모리 공간을 충분하지만 실제로 할당할 수 없는 상황을 일컫습니다.

* [https://junghyun100.github.io/%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%8B%A8%ED%8E%B8%ED%99%94/](https://junghyun100.github.io/%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%8B%A8%ED%8E%B8%ED%99%94/)

### 스택과 힙 차이점

> 스택(Stack)은 메모리 할당과 할당 해제가 자동으로 수행되는 반면 힙(Heap)은 프로그래머가 수동으로 메모리를 할당하고 해제해야 합니다.

<figure><img src="broken-reference" alt=""><figcaption><p>스택과 힙 메모리의 차이</p></figcaption></figure>

* [https://hooun.tistory.com/193](https://hooun.tistory.com/193)
