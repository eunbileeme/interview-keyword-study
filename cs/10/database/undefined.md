# 이은비

## :floppy\_disk: 이상현상이란?

> 이상현상은 테이블을 설계할 때, 잘못 설계하여 데이터를 삽입 / 삭제 / 수정할 때 논리적으로 생기는 오류를 말합니다.

1. 삽입 이상(Insertion Anomaly)

> 자료(= row)를 삽입할 때, 의도치 않은 자료까지 삽입해야만 자료를 테이블에 추가할 수 있는 현상

* ex. 특정 column에 대하여 데이터(= row)를 삽입할 경우, 다른 column에 대한 row의 값에는 null을 추가함으로써 삽입이 가능한 상황

2. 갱신 이상(Modification Anomaly)

> 중복된 데이터의 일부만 수정되어 데이터 모순이 일어나는 현상

3. 삭제 이상(Deleation Anomaly)

> 어떤 데이터를 삭제할 경우, 의도치 않은 데이터까지 삭제되는 현상

> [https://mangkyu.tistory.com/299](https://mangkyu.tistory.com/299)

## 갱신 손실 문제란?

> 두 개 이상의 트랜잭션이 하나의 값에 접근할 때, 모두 write연산을 수행하여 무제어 병행 수행을 하는 경우에 발생하는 문제점 중 하나입니다. 즉, 갱신 손실 문제란 **하나의 트랜잭션이 갱신한 내용을 다른 트랜잭션이 덮어씀으로써 갱신이 무효화가 되는 것을 의미**합니다.

<figure><img src="../../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

* 또한 두 개의 트랜잭션이 한 개의 데이터를 동시에 갱신할 때 발생합니다.

<figure><img src="../../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

1. x = 1000
2. 트랜잭션 T\_01이 +300을 수행 (x = 1300)
3. 이때, 동시성 제어를 해주지 않았다면 트랜잭션 T\_02는 기존 x의 값을 읽어와서 업데이트 (x = 500)
4. 최종적으로 트랜잭션 T\_01의 값과 트랜잭션 T\_02의 값이 저장되었다면, 트랜잭션 T\_01의 값은 덮어씌워짐으로써 손실이 발생

> [https://mangkyu.tistory.com/30](https://mangkyu.tistory.com/30)

## 트랜잭션 격리 수준

> **여러 트랜잭션이 동시에 처리**될 때, **특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 여부를 결정**하는 것입니다.

<figure><img src="../../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

1. Serializable

> 가장 최고 수준의 격리 수준으로, 트랜잭션을 순차적으로 진행시키는 방법으로써 **여러 트랜잭션은 동일한 레코드, 즉 데이터에 동시 접근할 수 없는** 개념입니다.

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

* 어떠한 데이터 부정합 문제도 발생하지 않습니다.
  * 하지만, 트랜잭션이 순차적으로 처리되어야 하므로 동시 처리 성능이 떨어집니다.
* MySQL에서는 순수한 SELECT 작업은 아무런 레코드 잠금 없이 실행되지만, Serializable에서는 위 경우에도 대상 레코드에 Shared Lock을 걸음으로써 한 트젝션에서 해당 레코드를 다른 트랜잭션에서 절대로 추가 / 수정 / 삭제를 진행할 수 없습니다.
  * 단, 한 트랜잭션에서 읽은 레코드를 다른 트랜잭션에서도 조회할 수는 있습니다.

2. Repeatable Read

> 기본적으로 한 트랜잭션에서 다른 트랜잭션이 변경한 작업 내역이 보이지 않습니다.

<figure><img src="../../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

* Phantom Read

> MySQL에서 Phantom Read를 발생시키려면 사용자 B가 첫 조회에는 잠금이 없지만, 두 번째 조회에는 잠금을 획득해야 합니다. (MVCC를 사용하면 일반적으로는 Phantom Read가 발생하지 않지만, 비관적 락을 사용할 경우에는 Phantom Read를 발생시킬 수 있다, 강제로 발생시키려는 조건인)

<figure><img src="../../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

3. Read Committed

<figure><img src="../../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

4. Read Uncommited

> 커밋하지 않은 데이터까지 접근할 수 있는 격리 수준입니다. 즉, 다른 트랜잭션의 작업이 커밋 또는 롤백되지 않아도 즉시 보일 수 있습니다.

* Dirty Read : 다른 트랜잭션의 작업이 커밋 또는 롤백되지 않았기에 노출되는 데이터를 조회하는 문제입니다.

<figure><img src="../../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

> [https://app.gitbook.com/o/tN1MSPHlxoiOgPZsGKiX/s/E4DEN4C9RzZlPzeJn3xf/\~/changes/26/db](broken-reference)

> [https://mangkyu.tistory.com/300](https://mangkyu.tistory.com/300)

### MySQL은 왜 Repeatable Read가 기본일까?

1. Repeatable Read는 Serializable 격리 수준에 비해 더 낮은 잠금 오버헤드를 가집니다. 따라서 필요한 경우에만 shared lock을 사용하여 성능을 유지할 수 있기 때문입니다.
2. Repeatable Read에서는 한 트랜잭션 내에서 일관된 읽기가 보장됩니다.&#x20;

* 즉, 트랜잭션이 시작한 시점의 데이터 스냅샷을 기반으로 조회가 이루어지며 트랜잭션 도중, 다른 트랜잭션에 의한 변경 사항은 보이지 않습니다.
  * 이는 데이터 무결성을 우지하는데 더 도움이 됩니다.

3. 데드락과 충돌 감소

* Repeatable Read는 더 낮은 격리 수준들에 비해 데드락과 충돌 가능성을 줄여줍니다.
  * 이는 복잡한 트랜잭션 시나리오에서 안정성을 향상시킵니다.

### MySQL은 Phantom Read가 왜 발생하지 않을까?

> MySQL의 InnoDB 스토리지 엔진이 사용하는 멀티버전 동시성 제어(MVCC, Multi-Version Cocurrency Control) 메커니즘 때문입니다.

1. 멀티버전 동시성 제어(MVCC)&#x20;

* InnoDB 스토리지 엔진은 MVCC를 사용하여 데이터의 여러 버전을 관리합니다.
* MVCC는 각 트랜잭션이 시작될 때, 데이터의 스냅샷을 생성합니다.
  * 이 스냅샷은 트랜잭션이 데이터를 읽을 때마다 일관된 데이터 뷰를 제공합니다.

2. Next-Key-Locks

* InnoDB는 Next-Key Locks라는 방식을 사용하여 키 범위 잠금을 구현합니다.
* 이는 인덱스 레코드와 해당 레코드와 인접한 레코드와 인접한 gap에 대한 잠금을 포함합니다.
  * 이러한 잠금은 새로운 행이 해당 범위에 삽입되는 것을 방지하여 Phantom Read를 방지합니다.

3. 트랜잭션의 격리

* Repeatable Read 격리 수준에서 MySQL은 트랜잭션이 시작될 때의 데이터 스냅샷을 사용합니다.
  * 따라서 트랜잭션 동안 다른 트랜잭션에 의해 삽입되거나 삭제된 행이 트랜잭션에 영향을 미치지 않습니다.
    * 즉, 트랜잭션 도중 다른 트랜잭션에 의해 생성된 유령 레코드는 보이지 않습니다.

### 은행과 같은 중요한 업무에서는 어떠한 격리수준을 사용하면 좋을까?

> 은행과 같은 중요한 금융 업무에서는 데이터의 정확성과 일관성이 매우 중요합니다. 따라서 트랜잭션 간의 간섭을 최소호화하면서도 동시성을 적절히 관리해야 합니다.

#### Serializable

* 데이터베이스의 가장 엄격한 격리 수준입니다.
  * 트랜잭션이 다른 트랜잭션으로부터 완전히 격리되며, Phantom Read, 논리적 일관성 문제, 업데이트 문제 등을 방지할 수 있습니다.
    * 하지만, 성능 상의 오버헤드가 크고 동시성이 상대적으로 낮다는 단점이 있습니다.
* 그럼에도 불구하고, 데이터의 정확성이 중요한 은행과 같은 업무에서는 Serializable 격리 수준이 종종 선호됩니다. (청천님께 자료 보내드리기)

## 옵티마이저

### 옵티마이저란?

> 데이터베이스 성능 개선을 위한 기법 중 하나로서, SQL을 수행할 최적의 처리 경로를 생성해주는 DBMS의 핵심 엔진으로서 가장 효율적인 방법입니다.

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

1. 규칙 기반 옵티마이저

> 실행 속도가 빠른 순으로 규칙을 먼저 세워두고, 우선순위가 앞서는 방법을 채택합니다.

2. 비용 기반 옵티마이저

> 옵티마이저에서 실행 계획을 세운 후, 비용이 최소한으로 나온 실행 계획을 수행하는 방식입니다.

* 통계 정보란?
  * [https://dev.mysql.com/blog-archive/histogram-statistics-in-mysql/](https://dev.mysql.com/blog-archive/histogram-statistics-in-mysql/)
  * [https://hoing.io/archives/1051](https://hoing.io/archives/1051)

> [https://velog.io/@kwontae1313/%EC%98%B5%ED%8B%B0%EB%A7%88%EC%9D%B4%EC%A0%80](https://velog.io/@kwontae1313/%EC%98%B5%ED%8B%B0%EB%A7%88%EC%9D%B4%EC%A0%80)
