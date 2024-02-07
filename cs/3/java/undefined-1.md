# 김의진

## 스레드

### synchronized를 사용하면 성능저하가 발생하는 이유

네 synchronized에는 성능저하가 있고, 성능저하가 발생하는 이유는 크게 2가지입니다.

1. synchronized 블록 또는 메소드가 이미 특정 스레드에 의해 락이 걸려 있으면, 해당 락에 접근하는 스레드가 blocked 상태가 되어 다른 작업을 하지 못하기에 CPU 이용률이 떨어질 수 있습니다.
2. 컨텍스트 스위칭 비용이 발생할 수 있습니다. 스레드가 blocked 상태가 되어 CPU가 다른 작업으로 전환할 때, blocked 상태의 스레드가 고유 락을 얻고 ready 혹은 running 상태로 변경할 때 CPU가 해당 작업으로 전환할 때 컨텍스트 스위칭이 발생합니다. 이는 오버헤드를 발생시켜 시스템 성능을 저하시킵니다.

따라서 Atomic 클래스와 같은 lock-free 스레드 안전 단일 변수를 사용하고, synchronized 블록 범위를 최소화한 동시성 컬렉션(Concurrent Collection)을 이용해야 합니다. 또한 불변 객체로 만들 수 있다면 불변객체로 만들어서 스레드 안전을 보장하는 것도 하나의 방법이라고 생각합니다.

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/package-summary.html https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html 이펙티브 자바 - item 78, 81

## Java

### Java를 선택한 이유

* 플랫폼에 독립적 : 자바 애플리케이션은 JVM 위에서 실행되어, 특정 운영체제에 종속되지 않고 다양한 플랫폼에서 실행할 수 있습니다.
* 객체 지향 언어 : 자바는 객체 지향 프로그래밍 언어로, 추상화, 캡슐화, 상속, 다형성과 같은 객체 지향의 핵심 개념을 지원합니다. 이로 인해 적절한 디자인 패턴과 설계를 통해 코드의 재사용성을 높이고 확장성을 고려하여 코드를 작성할 수 있습니다.
* 커뮤니티 지원 : 자바는 광범위한 개발자 커뮤니티와 학습자료가 있어 지속적으로 학습할 수 있습니다.

## 객체지향

### 객체 지향이란?

객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해서 시스템을 분할하는 방법입니다. 자유적인 객체란 이러한 객체지향의 특성으로는 캡슐화, 추상화, 상속, 다형성이 있습니다.

* 캡슐화 : 내부 구현을 숨겨 구현과 API를 분리하는 것입니다.
* 추상화 : 불필요한 세부사항은 생략하고 필요한 기능만 사용자에게 제공하는 것입니다.
* 상속 : 상속은 상위클래스의 특성을 하위 클래스에서 상속(특성 상속)하고 필요한 특성을 확장해서 사용할 수 있습니다.
* 다형성 : 같은 인터페이스나 부모 클래스를 가진 객체들이 서로 다른 방식으로 동작할 수 있게끔 합니다.

```
- 객체지향의 사실과 오해
객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해서 시스템을 분할하는 방법이다.
자율적인 객체란 상태,행위를 함께 지니며 스스로 자기 자신을 책임지는 객체를 의미한다.
객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력한다. 각 객체는 협력 내에서 정해진 역할을 수행하며 역할은 관련된 책임의 집합이다.
객체는 다른 객체와 협력하기 위해서 메시지를 전송하고, 메세지를 수신한 객체는 메시지를 처리하는데 접합한 메시지를 자율적으로 선택한다.
```

### 의존성이란?

의존성은 한 클래스가 다른 클래스의 메소드나 객체를 사용할 때, 이들 사이에 의존성이 존재합니다. 의존성은 일반적으로 한 클래스가 다른 클래스에 일시적으로 의존하여 작업을 수행하는 경우를 나타냅니다. 의존성은 주로 메소드 내부에서 다른 클래스의 객체를 생성하거나, 메소드의 인자로 다른 클래스의 객체를 받을 때 나타납니다. 의존성은 변경에 민감하며, 하나의 클래스가 변경될 경우 의존하는 클래스에도 영향을 미칠 수 있습니다.

### 의존성과 연관관계의 차이점

연관관계는 두 클래스가 서로 연결되어 있음을 나타내며, 이러한 연결은 한 클래스의 객체가 다른 클래스의 객체를 참조하는 것을 포함합니다. 연관관계는 일반적으로 두 클래스 사이에 더 긴 기간 동안 지속되는 관계를 의미합니다.

의존성은 클래스 간의 일시적이고 간접적인 관계를 나타내며, 주로 메소드 수준에서 발생합니다. 반면에 연관관계는 두 클래스 간의 보다 직접적이고 장기적인 관계를 나타내며, 필드 수준에서 정의됩니다.

### 세터를 지양해야 하는 이유는?

* 불변성 유지 불가 : 세터 메소드는 객체의 상태를 변경할 수 있게 해서 객체의 불변성을 해칩니다. 불변 객체일 때 변경 가능성이 없으므로 더 안정적이고 예측가능하며 스레드 안전한데, 이러한 이점을 누릴 수 없게 됩니다.
* 캡슐화 위반 : 세터를 사용하면 객체의 내부 구현이 외부로 노출되어 캡슐화 원칙이 깨지게 됩니다. 따라서 객체 스스로 상태와 행동을 관리하기 어렵습니다.
* 사이드 이펙트 증가 : 세터를 통해 객체의 상태를 변경하면, 객체의 상태를 예측하기 어려워 예상치 못한 사이드 이펙트가 발생할 수 있습니다.

## 함수형 인터페이스와 익명클래스

### 함수형 인터페이스란?

함수형 인터페이스는 추상메소드를 하나만 갖고 있는 인터페이스입니다. 람다 표현식과 메소드 참조에 사용되고 있습니다. 함수형 인터페이스에 두 개 이상의 메소드 선언을 방지하기 위해 @FuntionalInterface라는 어노테이션을 사용하고 있습니다.

### 익명 클래스와 함수형 인터페이스의 차이

* 익명 클래스는 이름이 없는 클래스로 한 번만 사용됩니다. 익명 클래스는 인터페이스 구현하거나 추상 클래스를 확장해서 정의합니다.
* 함수형 인터페이스는 단 하나의 추상 메소드를 가진 인터페이스입니다. 람다표현식을 사용하여 인스턴스를 간결하게 작성할 수 있습니다.
* 함수형 인터페이스는 익명클래스보다 람다 표현식을 사용해서 간결한 문법을 가지고, 함수형 프로그래밍 스타일을 쉽게 채택할 수 있게 해줍니다.

## 인터페이스

### 인터페이스 사용 시 왜 유지 보수성이 높아지나요

추상화는 기능의 내부 구현을 숨기고 사용자에게 필요한 기능만을 보여줍니다. 인터페이스를 통해서 추상화를 할 수 있습니다. 내부 구현이 변경되더라도 인터페이스를 사용하는 코드는 영향을 받지 않으므로 OCP를 지킬 수 있습니다.

인터페이스를 통해 객체 간의 느슨한 결합을 유지할 수 있습니다. 구현체의 변경이 다른 부분에 미치는 영향을 최소화할 수 있습니다. 인터페이스를 사용하면 테스트 시 목 객체를 쉽게 사용할 수 있어, 단위 테스트를 간편하게 수행할 수 있습니다.

### 추상 클래스 대신 인터페이스를 사용할 때의 이점과 사용하는 경우

추상 클래스와 비교해서 다음과 같은 장점이 있기에 인터페이스를 사용합니다.

* 추상 클래스를 상속 받은 클래스는 구체적인 상태와 행동을 인터페이스는 상태와 행동에 관한 규약만 명시되어 있기 때문입니다.
* 인터페이스는 특정 기능을 구현하는 것을 강제합니다. 따라서 일관된 설계를 할 수 있습니다.
* 다중 상속 : 자바에서는 클래스는 여러 인터페이스를 구현할 수 있지만, 단 하나의 추상클래스만 상속받을 수 있습니다. 이러한 다중 상속의 특성은 클래스가 여러 인터페이스를 조합함으로써 유연성을 높입니다.

> * 인터페이스와 추상 클래스의 차이\
>   추상 클래스는 인스턴스화할 수 없는 클래스로, 추상 메소드와 구체적인 구현이 포함된 메소드를 포함할 수 있다. 반면 인터페이스는 클래스가 구현해야할 메소드들을 명시한다. 클래스는 하나의 추상클래스로부터만 상속받을 수 있지만, 여러 인터페이스를 구현할 수 있다. 추상클래스는 public, protected, private와 같은 접근 제어자를 가질 수 있지만 인터페이스는 public 접근만 가능하다. 추상 클래스는 멤버 변수를 가질 수 있지만 인터페이스는 가질 수 없다. 추상 클래스는 구체적인 하위 클래스가 상속받을 수 있는 기본 클래스를 제공하는 데 사용되며, 인터페이스는 클래스가 구현해야 하는 일련의 메소드를 정의하는 데 사용된다.