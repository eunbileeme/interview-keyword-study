# 최지율

## 🫛 쓰레드와 프로세스

### 🍇 프로세스 간의 통신 방법 (IPC : Inter Process Communication)

#### 📨 메세지를 전달하는 방법 (Message Passing, PIPE)

* 커널을 통해 메세지를 전달합니다.
* 프로세스 간에 메세지를 주고받을 때 메세지 큐, 파이프, 소켓 등을 사용할 수 있습니다.
* Direct Communication: 통신하려는 프로세스의 이름을 명시적으로 표시 하여 직접 통신하는 방법입니다. (커널을 거치긴 함.)
* Indirect Communication: 커널에 있는 mailbox를 통해 메세지를 간접적으로 받는 경우입니다.

#### 🥜 주소 공간을 공유하는 방법 (Shared Memory)

* 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 합니다.
* 그만큼 신뢰할 수 있는 프로세스 들끼리만 해야합니다.
* 이 공유 메모리 영역을 통해 데이터를 공유하고 효율적으로 전달할 수 있습니다.
* 프로세스들끼리 쉐어드 메모리를 할 수 있도록 최초에는 동일하게 커널의 도움을 받아야 할 수 있지만
* 한번 할당 받고 난 이후로는 커널의 도움 없이 프로세스 간의 통신을 진행 할 수 있습니다.

이외에도 **Message Queue, Memory Map, Socket** 등이 있습니다.

### 🍈 크롬 브라우저의 탭은 프로세스일까 쓰레드 일까?

아마 이미 쓰레드에 대한 학습을 해보셨던분들이라면 하나의 프로세스를 여러 탭에서 쓰레드로 효율적으로 재사용 할 것 같지만,

<figure><img src="../../../.gitbook/assets/image (81).png" alt=""><figcaption><p><a href="https://developer.chrome.com/blog/inside-browser-part1?hl=ko#the_benefit_of_multi-process_architecture_in_chrome">https://developer.chrome.com/blog/inside-browser-part1?hl=ko#the_benefit_of_multi-process_architecture_in_chrome</a></p></figcaption></figure>

정답을 말씀드리자면 아래의 이유들을 통해 크롬의 브라우저 탭은 각각의 **프로세스**로 이루어져 있습니다.&#x20;

* 만약 모든 탭이 하나의 프로세스에 여러 쓰레드로 사용하는 환경이였다면,
* 하나의 페이지가 오류가 발생하여 응답이 되지 않을 경우 위의 사진과 같이 다른 탭의 페이지에도 응답을 기다리기 위해 동일하게 응답하지 않을 것 입니다.
* 따라서 특정 탭이 응답하지 않을 경우, 해당 탭의 프로세스를 재시작하여 문제를 해결하는 방식을 채택하고 있습니다.
* 단, 유추할 수 있듯이 멀티 쓰레드 방식에 비해서 멀티 프로세스는 높은 성능을 요구하게 됩니다.
* 그럼에도 크롬은 이러한 방식을 선택한 이유로는 웹 사이트를 별도의 심사 없이 배포하기 때문에 악성웹 사이트에게 모든 공유 자원을 주어 더 큰 문제의 발생을 막기 위함으로 볼 수 있습니다. (~~왠지 메모리를 맨날 많이 잡아먹더라니..~~)

#### 🍀참고로! 하나의 탭에도 하나의 프로세스가 아닌 멀티 프로세스를 사용하고 있다고 합니다.

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

* 하나의 탭을 구성할 때에도 브라우저 프로세스, 렌더러 프로세스, 플러그인  프로세스, GPU 프로세스 등 여러 프로세스 들이 모여 하나의 브라우저를 그리게 되는데,
* 이외에도 그림에는 포함되지 않은 확장 프로세스, 유틸리티 프로세스 등과 같은 더 많은 프로세스들이 존재하고 있다고 합니다.

#### 🍀 또한 최근에는 페이지 내부에 각각의 **iframe마다 사이트 격리**를 목적으로 사이트와는 별도의 렌더기 프로세스를 사용하여 구현하고 있는데,

<figure><img src="../../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

* 이는 동일한 렌더기 프로세스를 사용하여 iframe을 실행하게 된다면 한 사이트에서 동의 없이 다른 사이트의 데이터를 액세스 할 수 있고,
* 이렇게 우회하는 방식으로 **보안 공격의 목표**가 될 수 있기 때문이라고 합니다.

#### 📚 참고자료

[https://developer.chrome.com/blog/inside-browser-part1?hl=ko#browser-architecture](https://developer.chrome.com/blog/inside-browser-part1?hl=ko#browser-architecture)

&#x20;

### 🍉 프로세스와 쓰레드의 메모리 영역

#### 프로세스

<figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption><p><a href="https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html">https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html</a></p></figcaption></figure>

* **코드 영역**: 실행 할 프로그램의 명령어가 저장되는 곳 입니다.
* **데이터 영역**: 전역 변수 및 정적 변수가 저장되는 곳 입니다.
* **스택 영역**: 지역 변수 및 함수 호출마다 스택 프레임이 생성됩니다.
* **힙 영역**: 동적으로 할당된 메모리가 저장되는 공간입니다.

#### 쓰레드

<figure><img src="../../../.gitbook/assets/image (76).png" alt=""><figcaption><p><a href="https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html">https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html</a></p></figcaption></figure>

* 모든쓰레드의 코드, 데이터, 힙 영역은 프로세스와 동일합니다. (공유)
* 때문에 동기화 관련된 문제에 대한 주의가 필요합니다.
* 스택 영역만을 독립적인 스택으로 가지고 있습니다.
* 또한 쓰레드 간에 스택 정보는 공유 되지않습니다.
* 따라서 쓰레드는 프로세스 내에서 실행 되는 여러 실행 흐름으로 볼 수 있습니다.

### 🍊 멀티 쓰레드의 장/단점

<figure><img src="../../../.gitbook/assets/image (75).png" alt=""><figcaption><p><a href="https://velog.io/@eunjin/OS-%EC%8B%B1%EA%B8%80%EC%8A%A4%EB%A0%88%EB%93%9C-%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%9D%98%EB%AF%B8">그림 참조</a></p></figcaption></figure>

#### 장점

* 프로세스의 코드, 데이터, 힙 영역을 공유하고 있기 때문에 자원을 효율적으로 사용할 수 있고,
* 쓰레드 간에 컨텍스트 스위칭 비용을 상당히 아낄 수 있습니다. (전혀 없는 것은 아님)
* 또한 프로세스 간의 통신(IPC)은 대게 불편하거나 시스템 콜을 발생 시키는 반면에 쓰레드 간에 데이터를 주고 받는 것은 간단해져 공유에 대한 자원 소모를 줄일 수 있습니다.
* 작업을 쉽게 여러 쓰레드 단위로 나누어 빠른병렬 처리를 기대 할 수 있습니다.

#### 단점

* 하나의 프로세스에서 여러 쓰레드가 자원을 공유하므로 동기화 관련 문제가 발생합니다.
* 하나의 스레드에 문제가 발생하면 전체 프로세스가 영향을 받습니다.

### 🍋 Context Switching에 대해 설명해주세요.

<figure><img src="../../../.gitbook/assets/image (74).png" alt=""><figcaption><p><a href="https://byjus.com/gate/context-switching-in-os-notes/">https://byjus.com/gate/context-switching-in-os-notes/</a></p></figcaption></figure>

* Context switching이란 CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정입니다.
* 현재 실행 중인 프로세스가 다른 I/O 등의 리소스를 필요로 할 경우 CPU는 대기를 해야 하는데,&#x20;
* 이 때, 대기하는 시간동안 CPU가 사용되지 못하므로 CPU의 최대한의 효율을 내기 어렵습니다.
* 그로인해 빠른 CPU를 더 효율적으로 사용하기 위해 컨텍스트 스위칭을 사용하게 됩니다.

### 🍌 Context Switching 비용이 왜 발생 할까요.

* Context Switching이 발생하게 되면 실행 중이던 프로세스의 정보를(CPU에 있던 레지스터와 PC 등) 운영체제 커널 영역 내에 존재하는 PCB에 저장 하게 됩니다.
* 또한 다음에 실행하게 되는 Context(Process)에 대한 정보를 해당 PCB를 통해 가져오게 되는데,
* 이 때 마다 캐시 정보를 flush 해야 하므로 상당한 오버헤드가 발생합니다.

#### 🤔 그렇다면 Context Switching이 오히려더 발생하지 않을까 고민을 해볼 수 있을 텐데요.

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption><p><a href="https://inst.eecs.berkeley.edu/~cs162/sp18/static/lectures/10.pdf">캘리포니아대학교 버클리캠퍼스 강의자료 중</a></p></figcaption></figure>

* 오늘날의 일반적인 타임 슬라이스는 10ms - 100ms 사이라고 하고,
* 컨텍스트 스위치 오버헤드 스위치는 대략 0.1ms – 1ms가 걸린다고 합니다.

때문에 오늘날의 약 1% 정도의 문맥 교환 비용은 CPU의 유휴 시간을 가지는 것보다 감당할 수 있는 수준이기 때문에 오늘날에 Context Switching가 사용된다고 합니다.

## 🎡 스케줄링

### 🍍 CPU 스케줄링이란?

<figure><img src="../../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

* 운영체제의 CPU 스케줄러는 Ready 상태의 프로세스 중에서 어떤 프로세스에게 CPU를 할당할 지 결정합니다.
* 여러 프로세스가 동시에 실행될 수 있는 시스템에서 CPU 스케줄링은 시스템의 성능, 응답 시간, 처리량 등을 관리하고 최적화하기 위해 중요한 역할을 합니다.
* 그로인해 다양한 CPU 스케줄링 알고리즘이 있으며, 각각의 알고리즘은 특정 상황이나 요구사항에 맞게 선택되어야 합니다.

### 🥭 PCB 란? (Process Control Block)

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption><p><a href="https://byjus.com/gate/process-control-block-notes/">https://byjus.com/gate/process-control-block-notes/</a></p></figcaption></figure>

* PCB란 특정 프로세스에 대한 정보를 추적하는 데이터 구조로 운영체제가 프로세스를 표현 한 것이라고 할 수 있습니다.
* 운영체제의 커널영역에 저장되어 있는 자료구조로 각 프로세스를 관리하기 위해 프로세스 당 유지하는 정보입니다.
* PCB 구조 정
  * OS가 관리상 사용하는 정보
    * 프로세스 ID, 프로세스 상태(Ready, Running, Blocked)
    * scheduling information(스케줄링 정보), priority(우선순위)
  * CPU 수행 관련 하드웨어 값
    * PC(Program counter), registers
  * 메모리 관련
    * Code, Data, Stack 들의 위치 정보.
  * 파일관련
    * Open file descriptors
    * 오픈하고 있는 파일의 정보, 리소스와 관련 된 정보들

### 🍎 Multilevel Queue 스케줄링이란?

<figure><img src="../../../.gitbook/assets/image (71).png" alt=""><figcaption><p><a href="https://www.javatpoint.com/multilevel-queue-scheduling-in-operating-system">https://www.javatpoint.com/multilevel-queue-scheduling-in-operating-system</a></p></figcaption></figure>

* Multilevel Queue란 우선순위에 따라 Ready Queue를 여러개의 foreground 큐와 background 큐로 분할 하는 방법입니다.
* 이는 시스템에서 다양한 작업들 간에 우선순위를 부여하여 효율적으로 관리할 수 있게 해줍니다.
* 또한, 각 큐에는 특정 우선순위의 작업들만을 관리하므로 처리 시에 우선순위에 따라 적절한 작업을 선택할 수 있습니다.&#x20;
* 이는 시스템의 성능과 응답 시간을 최적화하는 데 도움이 됩니다.
* 하지만 높은 우선순위를 가지는 foreground 큐에 계속하여 프로세스가 많으면 background 큐에는 Starvation(기아현상)이 발생할 수 있으므로,
* 낮은 우선순위 background 큐에게도 CPU time을 적절하게 배분해주어야 합니다. (예를 들어 7:3 배분)

### 🍏 CPU가 여러 개 일 때 왜 멀티레벨 큐가 왜 적합할까

* 우선 멀티레벨 큐를 사용하는 목적은 우선순위에 따라 효율적으로 스케줄링을 하기 위함에 있는데요.
* 그 중에서 CPU가 여러 개 일 경우에 장점으로는 꼽는다면 병렬 처리를 효과적으로 할 수 있습니다.
* 만약 하나의 CPU만을 사용한다면 우선 순위가 높은 작업만 계속 실행 될 수 있고 이는 기아 현상이 발생 할 수 있습니다.
* 하지만 여러개의 CPU를 사용한다면 우선순위가 더 높은 큐에는 CPU를 더 많이 배분해주는 대신 다른 큐의 있는 작업들도 병렬적으로 실행하며 전체 시스템 처리율을 높일 수 있습니다.

### 🍐 비선점 스케줄링 VS 선점 스케줄링

#### 선점형(preemptive)

CPU를 강제로 중단시키고 빼앗는 경우에 해당합니다.

* Timer Interrupt 발생에 의해 할당 시간 만료로 CPU를 반납할 때. (Running -> Ready)
* System call 등이 발생한 경우. (Running -> Blocked)
* I/O 작업 수행 완료 시 해당 작업이 우선순위가 높은 경우. (Blocked -> Ready)

#### 비선점형(nonpreemptive)

CPU를 자진 해서 반납할 때까지 계속 실행 되는 것을 말합니다.

* 오래걸리는 I/O를 요청하는 시스템 콜 발생으로 인해 자진해서 CPU를 반납 할 때. (Running -> Blocked)
* 프로세스 종료에 의해 CPU 반환 할 때. (Terminate)
* 이렇듯 위와 같은 상황이 아니라면 계속하여 실행되기 때문에 Context Switching 비용은 거의 없지만 대기중인 프로세스들이 지연되어 전체 시스템의 처리율이 떨어질 수 있습니다.

### 🍑 선점 스케줄링의 예시

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption><p>Round-Robin 스케줄링</p></figcaption></figure>

선점 스케줄링에는 SJN, SJF, 멀티레벨 큐 등 여러가지가 있지만 현대의 CPU 스케줄링의 기본이 되고 있는 **Round Robin 스케줄링**을 예시로 들어보겠습니다.

* Round Robin은 모두에게 공평한 자원 기회를 주기 위해 각 프로세스 별로 일정 시간을 실행 시키고 컨텍스트 스위칭을 통해 번갈아 가면서 실행을 합니다.&#x20;
* 이 때 할당 시간이 만료 되면 OS의 CPU 스케줄러는 강제로 CPU를 뺏고 다른 프로세스에게 넘겨주게 됩니다.
