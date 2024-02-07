# 이은비

## List vs Set

> 가장 큰 차이점은 리스트는 중복되는 원소를 허용하고 순서를 유지하는 반면, 집합은 중복 원소를 허용하지 않고 순서 또한 보장하지 않습니다.

> [https://www.scaler.com/topics/java-set-vs-list/](https://www.scaler.com/topics/java-set-vs-list/)

### ArrayList의 가장 큰 특징은 무엇이라고 생각하시나요?

> 동적 배열이 가장 큰 특징이라고 생각합니다. 이는 기본 Object 배열과 ArrayList가 유사하지만, 크기 제한이 없다는 점에서 차이를 가지게 되는 점이기도 합니다. 즉, 언제든지 원소를 추가하거나 제거할 수 있기에 유연한 작업을 수행할 수 있습니다.

> [https://www.javatpoint.com/java-arraylist](https://www.javatpoint.com/java-arraylist)

### Set은 순서를 보장할까요?

> Java에서의 집합은 순서가 지정되지 않은 객체 컬렉션입니다. 따라서 순서는 보장하지 않으며, 위치 인덱싱 또한 허용되지 않습니다.

## 라이브러리 vs 프레임워크

> 라이브러리란 개발자가 작업을 쉽게 하고 개발을 가속화하는 데 사용할 수 있는 사전 정의된 메서드 및 클래스 모음입니다. 또한, 표준 라이브러리가 포함되어 있지만 자신만의 맞춤형 라이브러리를 만들 수도 있습니다.

> 반면 프레임워크는 기본 구조를 제공하는 동시에 소프트웨어 애플리케이션의 작업 흐름을 정의하며 개발자에게 필요한 것을 알리고, 필요할 때 코드를 호출합니다.&#x20;
>
> 따라서 프레임워크의 코드에 완성된 함수가 포함되어 있지 않다는 것이 큰 차이점입니다.

> [https://www.sencha.com/blog/difference-between-framework-vs-library-snc/](https://www.sencha.com/blog/difference-between-framework-vs-library-snc/)

## String vs StringBuffer vs StringBuilder

> String은 imumutable 하다는 특징을 가지고 있습니다. 또한 연산 시 새로 객체를 만드는 오버헤드가 발생하며 불변하다는 특징과 함께 멀티 스레드에서 동기화를 고려하지 않아도 됩니다. 즉, 연산이 적고 조회가 많은 멀티 스레드 환경에서 유리합니다.

> StringBuffer와 StringBuilder는 mutable한 특징 덕에 연산 시 새로 객체를 만들지 않고, 추가하는 형식으로 크기를 변경시킵니다. 또한 두 클래스의 메서드가 동일하다는 특징이 있습니다.&#x20;
>
> 그러나 StringBuilder는 Thread-safe하지 않기 때문에 문자열 연산이 많은 싱글 스레드 또는 스레드를 고려하지 않아도 되는 환경에 유리한 반면, StringBuffer는 문자열 연산이 많은 멀티 스레드 환경에 적합합니다.

## 서버에 세션을 저장할 때, HashMap과 Concurrent HashMap 중 어떤 것이 나을까요?

> 서버의 동시성 요구 사항에 따라 달라집니다, 즉 주요 고려 사항은 동시성 제어와 성능입니다.

HashMap은 동시성을 고려하지 않는 구현으로, 멀티 스레드 환경에서 안전하지 않습니다.

* 즉, 여러 스레드가 동시에 HashMap을 수정하려고 할 때, 데이터 무결성을 보장할 수 없습니다.
  * 이는 데이터 손실이나 예기치 않은 동작을 초래할 수 있습니다.
* 따라서 동기화 처리를 개발자가 직접 구현해야 하며, 이는 `Collections.synchronizedMap()` 를 사용하여 HashMap을 동기화된 맵으로 감싸는 방식으로 할 수 있습니다.
  * 하지만, 이 방식은 Map에 대한 모든 접근을 동기화하기 때문에, 대규모 멀티 스레드 환경에서는 성능 저하를 초래할 수 있습니다.

ConcurrentHashMap은 동시성을 고려하여 설계된 자료 구조로, 멀티 스레드 환경에서 데이터의 동시 수정이 일어나더라도 데이터의 무결성을 유지할 수 있게 해줍니다.

* 세그먼트 락킹(Segment locking)이라는 기술을 사용하여, 전체 Map을 락킹하는 대신 일부 세그먼트에 대해서만 락을 적용합니다.
  * 이로 인해 여러 스레드가 ConcurrentHashMap의 다른 부분을 동시에 읽고 쓸 수 있습니다.
    * 이는 훨씬 높은 동시성과 성능을 제공합니다.

따라서 서버에 세션을 저장하는 경우, 멀티 스레드 환경을 고려한다면 `ConcurrentHashMap` 이 더 나은 선택이라고 생각합니다.

## final

> final 키워드는 variable, method, class에 사용될 수 있습니다. 때문에, 어떤 곳에 사용되냐에 따라 다른 의미를 가지는 반면 "무언가를 제한한다."는 공통적인 특징은 가지고 있습니다.

1. variable

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

* 해당 변수는 수정할 수 없습니다.
  * 수정할 수 없기 때문에, 초기화는 필수적으로 진행되어야 합니다.
  * 단, 수정할 수 없는 범위는 해당 변수의 값에 한정됩니다.
    * 즉, primitive로 선언되었을 경우라면 값을 변경할 수 없는 반면 Reference로 선언되었을 경우에는 가리키는 객체를 변경하지 못합니다.
      * 이때, 후자의 경우 가리키고 있는 객체 내부의 값은 final의 범위 밖에 있기 때문에 변경이 가능합니다.

2. method

* 오버라이딩을 제한합니다.
  * 즉, 상속 받은 클래스에서 해당 메서드를 수정해서 사용하지 못하도록 할 수 있는 것이 메서드에 final을 붙이는 것입니다.

3. class

* 상속이 불가능한 클래스가 됩니다.
  * 즉, 다른 클래스에서 상속하여 재정의하는 것이 불가능해집니다.
    * ex. Integer, Wrapper

> [https://sabarada.tistory.com/148](https://sabarada.tistory.com/148)

## Stream

### Stream을 사용하는 이유는 무엇일까요?

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

1. 간결하고 읽기 쉬운 코드

* 명시적으로 컬렉션을 조작하는 방법을 통해 코드를 더욱 간결하고 읽기 쉽게 만듭니다.

2. 병렬 처리

* 스트림을 쉽게 병렬화(.parrallel())할 수 있어 대규모 데이터 세트를 효율적으로 처리할 수 있습니다.

3. 성능 향상

* (컴파일러로 스트림을 최적화 -> **Lazy evaluation)**하여 코드 실행 속도를 높일 수 있습니다.

그 밖에도 함수형 프로그래밍, 지연 평가 등이 가능하다는 장점이 있습니다.

> [https://medium.com/@harshgajjar7110/java-streams-the-good-the-bad-and-the-ugly-6f6b54526619](https://medium.com/@harshgajjar7110/java-streams-the-good-the-bad-and-the-ugly-6f6b54526619)

> [https://bugoverdose.github.io/development/stream-lazy-evaluation/](https://bugoverdose.github.io/development/stream-lazy-evaluation/)

## Generic이란?

> 메서드나 클래스 및 인터페이스에 대한 매개변수가 되도록 허용하는 개념입니다. 즉, Generic을 사용하면 다양한 데이터 유형으로 작동하는 클래스를 생성할 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

> [https://www.geeksforgeeks.org/generics-in-java/](https://www.geeksforgeeks.org/generics-in-java/)

### Generic의 와일드카드를 어떻게 사용해야할까요?

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### AOP란?

> 개발자가 애플리케이션에서 로깅 및 보안과 같은 크로스 커팅 문제를 모듈화할 수 있도록 하는 프로그래밍 패러다임입니다. 특히, Spring AOP는 Java 애플리케이션에서 AOP를 구현하기 위한 강력한 프레임워크이며 널리 사용되는 Spring Framework와 통합되어 있습니다.

### Compile

### war와 jar의 차이점은 무엇일까요?

> JAR(Java ARchive) 파일은 Java 클래스 파일, 관련 메타데이터 및 리소스를 배포하는 데 사용되는 패키지 파일 형식입니다. 일반적으로 독립형 Java 애플리케이션이나 라이브러리를 배포하는 데 사용됩니다.
>
> 반면 WAR(Web ARchive) 파일은 웹 애플리케이션을 패키지하고 배포하는 데 사용되는 특정 유형의 JAR 파일입니다. 여기에는 서블릿, JSP 및 HTML 파일과 같은 웹 애플리케이션의 리소스뿐만 아니라 web.xml 배포 설명자와 같은 추가 메타데이터가 포함된 WEB-INF 디렉터리가 포함되어 있습니다.

> [https://dev.to/martygo/what-is-the-difference-between-a-jar-and-a-war-file-402a](https://dev.to/martygo/what-is-the-difference-between-a-jar-and-a-war-file-402a)

### Java의 compile 과정

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

1. 개발자가 **자바 소스 코드(.java)** = 자바 클래스 파일(.java)를 작성합니다.
2. 자바 컴파일러(Java Compiler)가 자바 소스 파일(.java)을 **컴파일**
3. 컴파일된 바이트 코드(.class)를 JVM의 클래스 로더(Class Loader)에게 전달
4. 클래스 로더(Class Loader)는 동적 로딩(Dynamic Loading)을 통해 **필요한 클래스들을 로딩 및 링크**하여 런타임 데이터 영역(Runtime Data area) = **JVM Memory**에 올림

* **(1) 로드** : 클래스 파일(.class)을 가져와서 **JVM의 메모리에 로드**
* **(2) 검증** : 자바 언어 명세(Java Language Specification) 및 **JVM 명세**에 명시된 대로 구성되어 있는지 **검사**
* **(3) 준비** : **클래스가 필요로 하는 메모리**를 **할당**(**필드, 메서드, 인터페이스** 등등)
* **(4) 분석** : **클래스의 constant pool 내 모든 심볼릭 레퍼런스** → 다이렉트 레퍼런스로 변경
  * 메모리에 바로 접근할 수 있도록, 직접 참조로 변경
    * 실제 메모리로 바로 연결, 단 대상은 constant pool 내 모든 심볼릭 레퍼런스뿐
      * 상수들은 동적이 아닌 정적으로 할당이 되어 있기 때문입니다.

> [https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-5.html](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-5.html)

> [https://velog.io/@ariul-dev/%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-Java-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%8B%A4%ED%96%89-%EA%B3%BC%EC%A0%95#%E2%9D%B6-](https://velog.io/@ariul-dev/%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-Java-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%8B%A4%ED%96%89-%EA%B3%BC%EC%A0%95#%E2%9D%B6-)

5. **초기화** : **클래스 변수**들을 **적절한 값으로 초기화**(static 필드)
6. 실행 엔진(Execution Engine)은 VM 메모리에 올라온 바이트 코드(.class)들을 **명령어 단위로 하나씩 가져와서 실행** (이때, **실행 엔진**은 **두 가지 방식**으로 변경)
7. **인터프리터** : **바이트 코드 명령어를 하나씩 읽어서 해석하고 실행**

* 하나 하나의 실행은 빠르나, 전체적인 실행 속도가 느림

5. **JIT 컴파일러(Just-In-Time Compiler)** : 인터프리터의 단점을 보완하기 위해 도입된 방식

* (1) **바이트 코드(.class) 전체를 컴파일**하여 **바이너리 코드로 변경**
* (2) 그 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, **바이너리 코드로 직접 실행**
