# 이은비

## :desktop: fork()

### fork()를 수행하는 이유?

> 다른 프로그램을 실행하는 새 프로세스를 만들기 위해 사용합니다.

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

* fork()는 system call을 호출한 프로세스를 복사합니다.
  * 기존 프로세스에서 복사된 내용(Stack, Heap, Data, PCB)을 새로운 프로세스 공간에 복사합니다.
* 따라서 기존 프로세스는 부모 프로세스가 되고 새로운 프로세스는 자식 프로세스가 됩니다.
  * 이때, 텍스트(code)는 공유합니다.
* 결과적으로 복사한 자식 프로세스는 동일한 데이터들을 **참조** 만 하고 있는 상태이기 때문에, 실제 메모리 상에서는 같은 걸 복사하고 있진 않습니다.
  * 별개의 가상 메모리이지만, 물리 메모리에서는 같은 frame(영역)을 참조하고 있습니다.
  * [https://velog.io/@jungbumwoo/fork-%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90](https://velog.io/@jungbumwoo/fork-%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

### fork() - 자식 프로세스의 변환 예시?

### fork() - 자식 프로세스의 변환 예시?

```
#include <stdio.h>
#include <unistd.h>
#include <errno.h>

int global_count = 0; // Data영역에 할당

int main(int argc, const char * argv[]) {
	int local_count = 0; // Stack영역에 할당
	int pid = fork();

	if (pid == 0) {
		// 자식 프로세스에서 실행되는 영역
		global_count++;
		local_count++;
		printf("[Child] global count: %d(%p)\n", global_count, &global_count);
		printf("[Child] local count: %d(%p)\n", local_count, &local_count);
	} else {
		// 부모 프로세스에서 실행되는 영역
		printf("[Parent] global count: %d(%p)\n", global_count, &global_count);
		printf("[Parent] local count: %d(%p)\n", local_count, &local_count);
	}

	return 0;
}
// 실행결과
// [Child] global count: 1(0x10ddc5028)
// [Child] local count: 1(0x7ffee1e4283c)
// [Parent] global count: 1(0x10ddc5028)
// [Parent] local count: 0(0x7ffee1e4283c)
```

> 이처럼 fork()를 통해 새 프로세스(= 자식 프로세스)를 생성할 경우, 변수의 값은 다르지만 가리키는 메모리 주소는 동일합니다.

과정은 다음과 같습니다.

1. fork() 성공 시, 자식 프로세스 생성
2. 부모 프로세스에게는 자식 프로세스의 PID 반환
3. 자식 프로세스에게는 0 반환
4. 각각의 프로세스에서 main() 실행

* 부모의 PCB도 fork()를 통해 자식 프로세스에게 복사됩니다.
  * PPID(Parent Process ID)
  * PID(Process IDentification)만 부모 프로세스와 자식 프로세스에 다르게 존재합니다.
* PCB에는 CPU에서 수행되던 레지스터의 값 + Program counter의 정보가 존재합니다.
  * 따라서 자식 프로세스에서도 fork() 이후부터 코드가 실행됩니다.
    * [https://wslog.dev/fork-exec](https://wslog.dev/fork-exec)

<figure><img src="../../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

### Copy On write란?

> COW(Copy On Write)란 평소엔 자원을 공유하다가, 자원을 수정할 경우가 발생한다면 이전 자원의 데이터를 실제로 복사하여 사용하는 개념입니다.

<figure><img src="../../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

운영체제에서 COW는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

### fork()란?

> fork()란 프로세스를 복사하여 자식 프로세스를 생성하는 시스템 콜로서, 같은 프로그램에서 새로운 제어 흐름을 만드는 용도 또는 다른 프로그램을 실행하는 새 프로세스를 만드는 용도로서 사용합니다.

### fork() 명령어 장점

비용 측면에서 장점이 있습니다.

* fork()를 통해 페이지 테이블 자체를 복사했으므로 두 프로세스에서 변수가 갖는 페이지 테이블 주소(상대 주소)  자체는 동일하게 유지됩니다.
  * 초기에는 두 상대 주소가 갖는 물리 메모리 주소도 동일
  * 이후, 한 프로세스의 변수에서 변경이 일어나면 해당 프로세스의 페이지 테이블은 새로운 물리 메모리 주소를 가리킵니다. (COW)
    * 변경된 변수에 대해서 두 페이지 테이블이 실제로 가리키는 물리 메모리 주소는 달라졌지만, 페이지 테이블의 주소는 동일한 상태

결과적으로 fork()의 장점 = 즉, fork()를 사용하는 이유는 다음과 같습니다.

1. 프로세스를 새로생성하는 것보다 복제라는 비교적 간단한 작업
2. COW로 인해 복제의 비용이 거의 들지 않습니다.

## IPC

### IPC란?

> IPC(Inter Process Communication)란 프로세스 간 통신으로서 프로세스들끼리 서로 데이터를 주고 받는 행위 또는 방법을 일컫는 개념입니다.

* 프로세스는 완전히 독립된 실행 객체입니다.
*   커널 영역에서 IPC라는 내부 프로세스 간 통신을 제공하여 프로세스 간 통신을 진행합니다.



종류는 다음과 같이 2가지가 존재합니다.

<figure><img src="../../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

### IPC 중 공유 메모리에서 한 번 공유된 이후에 커널의 도움이 필요한지?

> 공유 메모리는 여러 프로세스가 동시에 접근할 수 있는 메모리 영역으로서, 공유 메모리의 접근에는 커널이 필요하지 않습니다.

* [https://dokhakdubini.tistory.com/490](https://dokhakdubini.tistory.com/490)

## PCB

### PCB란?

> PCB(Process Control Block)이란 운영체제가 프로세스를 제어하기 위해 정보를 저장하는 공간으로서, 프로세스의 상태 정보를 저장하는 구조체입니다.

* 프로세스의 상태 관리 및 Context switching (문맥 교환)을 위해 필요합니다.
* 프로세스 생성 시 만들어지며, 주 기억장치에 저장됩니다.
  * [https://junsangkwon.tistory.com/45](https://junsangkwon.tistory.com/45)

<figure><img src="../../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

* [https://hooun.tistory.com/193](https://hooun.tistory.com/193)
