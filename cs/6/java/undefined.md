# 고청천

## SOLID

### SOLID 원칙에 대해 설명 해주세요

**SRP 단일 책임의 원칙**은 작성된 클래스는 하나의 기능만 가져야 하며 이로인해 변경 이유는 오직 하나여야 함을 의미합니다. 이를 통해 특정 영역의 수정이 필요할 때 변경 지점을 최소화 할 수있습니다&#x20;

**OCP 개방 폐쇄의 원칙**은 소프트웨어 구성 요소(컴포넌트, 클래스, 모듈, 함수)는 확장에 열려있고 변경에는 닫혀야 한다 즉 요구사항 변경이나 추가 사항이 발생하더라도 기존 구성 요소는 수정이 일어나지 말고 기존 구성요소를 쉽게 확장해 재사용 가능해야한다는 것이다. 이를 가능하게 하는 건 추상화와 다형성이다&#x20;

**LSP 리스코프 치환의 원칙**은 서브 타입은 언제나 기반 타입으로 교체할 수 있어야 한다는 것이다 즉 서브 타입은 기반 타입이 약속한 규약을 지켜야한다 대표적으로 컬렉션 프레임워크가 이런 식으로 구현되있다&#x20;

**ISP 인터페이스 분리의 원칙** 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야한다는 원리다&#x20;

**DIP 의존 역전의 원칙** 하위 레벨 모듈의 변경이 상위 레벨 모듈의 변경을 요구하는 위계 관계를 최대한 느슨하게 만드는 원칙

### SOLID 원칙 중, 사용 해본 원칙

**ISP 원칙**에 따라 범용적인 인터페이스를 만드는 것을 지양하고 있습니다.&#x20;

**SRP** 와 유사하게 클래스 내부에 있는 메서드들은 각자 하나의 기능만 지니게 하여서, 변경 지점을 최소화 하였습니다

### OCP 란?

기존 코드는 변경하지 않으면서 새로운 기능을 추가 할 수 있다는 것을 의미합니다.

추상화를 통해 가능합니다

```java
abstract class Animal {
    void bite();
    void eat();
    void sleep();
}

class Dog extends Animal {
    eat();
    sleep();
}

class Cat extends Animal {
    eat();
    sleep();
}

class Squirrel extends Animal {
    eat();
    sleep();
}
```

객체를 추상화 하여서 확장에 열려있고 변경에 닫혀있는 구조를 만들면&#x20;

클래스 추가 시 기존  코드 수정 필요 없이 적절하게 상속 관계에 맞춰 추가하면 유연하게 확장 가능합니다

### 추상화란?

중요 부분을 강조하기 위해서 여러 정보들에서 핵심적인 개념이나 기능을 모으는 것입니다.

이를 통해서 복잡한 대상을 이해하기 쉽게 단순화 할 수 있습니다. -객체지향의 사실과 오해



개발자는 추상화를 통해 개발자가 정말 핵심적인 부분에만 집중 할 수 있습니다

예를 들어 변수를 들 수 있습니다. int a 라고 선언하고 값을 할당 했을 때,&#x20;

개발자에게는 a를 어떻게 사용할 지가 중요하지 a 가 실제 컴퓨터 메모리 상 구체적으로 어디에 저장 되었는 지는  크게 중요하지 않습니다.&#x20;

이렇듯 복잡한 대상을 단순화하여서 보다 개발에 집중하기 편하게 만들어줍니다

## 상속

### 정사각형이 직사각형에 속한다를 상속으로 표현

사각형 클래스를 만들고 이를 상속받는 정사각형과 직사각형 클래스를 만들거 같습니다&#x20;

정사각형의 정의 상 직사각형에 속할 수 있지만, 정사각형이 직사각형 클래스를 상속 받게 되면

리스코프 치환 문제가 발생할 수 있기 때문입니다

리스코프 치환의 원칙은 하위 클래스는 상위 클래스로 대체할 수 있어야 하는 것을 의미합니다. 일반적으로 생각할 때 정사각형은 정의에 따라 직사각형이기도 합니다. 하지만, 객체지향 관점에서 이를 클래스 상속 관계로 모델링 하면 문제가 발생합니다. 직사각형 클래스가 가로 세로 길이를 각각 변경할 수 잇는 메소드를 가지고 잇다고 가정합니다. 정사각형이 직사각형 클래스를 상속 받는 다면, 직사각형 클래스의 메서드를 사용해 한 변을 변경한다면 정사각형 정의에 어긋나게 됩니다. 이는 리스코프 치환 원칙을 위반하는 것으로 상위 클래스 인스턴스를 하위 클래스 인스턴스로 대체할 수 없게 한다. 따라서 객체를 설계할 때 IS A 관계가 확실히 성립하는 지 확인하는 것이 중요합니다

### 상속을 사용했을 시, 고려할 점

정사각형 직사각형 문제에서 보이는 거처럼 실제 세계에서는 is a 관계가 성립한다해서 프로그래밍 세계에서도 그러리란 보장은 없습니다 그렇기 때문에 프로그래밍 세계에서 is a 관계가 성립하는 지 논리적으로 따져 보아야 합니다

### 상속의 단점과 대안

상속의 단점은 하나만 상속을 받을 수 있다는 것입니다.&#x20;

또한 상속의 경우 캡슐화를 깨트릴 수 있다는 단점이 존재합니다 상위 클래스 구현이 하위 클래스에 노출 되기 때문입니다 하위 클래스가 상위 클래스에 강하게 의존하기 때문에 상위 클래스의 변경 사항에 의해 모든 하위 클래스를 수정 해줘야합니다

조합을 사용하면 메서드를 호출하는 방식으로 작동해서 캡슐화를 깨트리지 않습니다. 또한 메소드 호출 방식이라 기존 클래스 변화에도 영향이 적어집니다

## 자료 구조

### HashMap과 Concurrent HashMap

HashMap은 쓰레드 세이프 하지 않는 map 입니다 동기화 처리를 하지 않아 데이터 탐색 속도는 빠르다는 장점을 지닙니다 또한 key value 에 null을 허용합니다

concurrentHashMap 은 쓰레드 세이프한 map 입니다. 동기화 처리 시, 특정 Entry 조작하는 경우 해당 Entry에만 락이 걸립니다. 그래서 HashTable에 비해 성능적으로 우수합니다 key value 에 null을 허용하지 않습니다

```java
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) {
            throw new NullPointerException();
        }
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) { // 특정 Entry 조작하는 경우 해당 Entry에만 락이 걸립니다
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        
```

## 객체 지향

### 불변 객체의 단점

생성 후 그 상태를 바꿀 수 없는 객체를 말합니다 즉, 힙 영역을 가르키는 객체의 데이터 변화가 불가능 하다는 것입니다. 불변 객체를 사용했을 때 장점은 외부에서 값을 변경 할 수 없기 때문에 쓰레드 세이프래서 동기화작업이 필요없다는 것입니다



하지만 불변 객체는 수정을 할 때 새로운 객체를 만드는 방식으로 이루어 지는 데, 매 순간 객체를 생성하다보니 객체 생성 비용에 따른 단점이 생길 수 있습니다

## 기타

### 에러와 예외

[https://www.geeksforgeeks.org/errors-v-s-exceptions-in-java/\
\
https://www.nextree.co.kr/p3239/](https://www.geeksforgeeks.org/errors-v-s-exceptions-in-java/https://www.nextree.co.kr/p3239/)

에러는 개발자가 미리 예측해 처리할 수 없는 시스템 레벨에서 발생하는 오류입니다 대표적으로 OOM, 스택오버플로우 가 있습니다 예외는 개발자가 구현한 로직에서 발생하며, 미리 예측해 대비할 수 있습니다 예외는 크게 Checked Exception 과 UnChecked Exception(RuntimeException 하위) 이 있습니다 Checked Exception은 컴파일 단계에 확인이 되며 그렇기에 반드시 예외 처리해야합니다 (ex IOException) UnChecked Exception은 실행 단계에 확인이 될 수 있으며 그렇기에 명시적 처리를 강제하진 않습니다.

예외 발생시 트랜잭션 처리 기본 전략은 Checked Exception은 롤백 하지 않으며 UnChecked Exception은 롤백을 합니다. 물론 이는 옵션을 통해 변경 할 수 있습니다

왜 기본 전략이 이런 지는 다음을 참고하시면h됩니다

https://jangjjolkit.tistory.com/3
