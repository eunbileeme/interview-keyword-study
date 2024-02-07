# 최지율

## 💽 데이터 액세스

### 암달의 법칙이란?

암달의 법칙(~~암달의 저주~~)이란 컴퓨터 시스템의 일부를 개선할 때 **전체적으로 얼마만큼의 최대 성능 향상**이 있는지 계산하는 데 사용됩니다.

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption><p>암달의 법칙 공식 (P는 작업 시간, S는 작업 효율 증가 배수)</p></figcaption></figure>

가령, 전체 작업 시간 중에서 CPU가 처리하는 시간이 10%, SDD가 처리하는 시간이 90%를 차지하고 있게 된다면,

* 10%를 차지하는 CPU의 작업의 속도를 2배 증가 시키더라도 전체 작업 속도는 약 1.05배 즉, 전체 성능에서 **5%** 가량 증가하게 되고
* 90%를 차지하는 SDD의작업의 속도를 2배 증가 시켰다면 약 1.81배 즉,  전체 성능에서 **81%**의 성능 개선 효과를 가져올 수 있습니다.

따라서 아무리 빠른 CPU를 가져와서 사용을 하더라도 **디스크 I/O가 빠르게 처리가 되지 않으면 전체 시스템 성능 향상에 큰 영향을 줄 수 없다는 걸** 강조하기 위해서 가져오게 되었는데요.

<figure><img src="../../../.gitbook/assets/image (45).png" alt="" width="563"><figcaption><p><a href="https://hudi.blog/storage-and-random-sequantial-io/">https://hudi.blog/storage-and-random-sequantial-io/</a></p></figcaption></figure>

DB는 대부분 이러한 디스크의 I/O를(읽기, 쓰기, 삭제 등) 많이 처리하고 있어 한번의 잘못 작성된  쿼리는 전체 성능의 저하를 가져올 수 있습니다.

### 랜덤 I/O와 순차I/O 란?

#### 🔀 랜덤I/O

* 랜덤 I/O는 읽어야 하는 데이터가 **물리적으로 불연속적**으로 있기 때문에 디스크 헤더를 이동시킨 다음 데이터를 읽는 것을 의미합니다.
* HDD 일 때에는 이러한 랜덤 I/O를 읽어내기 위해 디스크 헤더를 움직여 읽으므로, 헤더를 움직이는데 걸리는 시간이 오래 걸렸지만.
* SSD는 물리적인 한계였던 디스크 판을 없애고 플래시 메모리를 사용하여 HDD에 비해서 월등하게 높은 성능 향상을 기대할 수 있습니다.

#### ➡️ 순차I/O

* 순차 I/O는 읽어야 하는 데이터가 **물리적으로 연속적**으로 있어 디스크 헤더의 이동 없이 순차적으로 읽는 것을 의미합니다.
* 순차 I/O의 경우 HDD와 SDD의 성능 차이는 비교적 많이 발생하지는 않는다고 합니다.
* SSD는 디스크판이 없지만 마찬가지로랜덤 I/O와 순차 I/O의 성능 차이는 많이 발생한다고 합니다.
  * 그 이유는 SSD는 NAND 플래시 메모리에 저장된 데이터를 직접 읽는 것이 아니라 사실 논리주소를 물리주소로 매핑하는 매핑테이블을 사용하고 있다고 합니다.
  * 따라서 매핑 테이블을 통해 NAND 플래시 메모리의 물리적인 위치를 찾아가야 하므로 SSD도추가적인 성능 이슈가 발생하게 됩니다.

따라서 일반적으로 **쿼리를 튜닝하는 것**은 순차 I/O를 사용할 지 랜덤 I/O를 선택할지 고민하는 것도 좋은 방법일 수 있으나, 실제로 랜덤 I/O가 순차 I/O로 변하는 일은 많이 않기에 가장 중요한 것은 꼭 필요한 정보만을 읽어 **I/O 작업 자체를 줄여주는 것이 주 목적**이라고 생각합니다.

## 🔍 INDEX

### Index 란?

데이터 베이스에서 검색 속도를 향상 시키기 위해 사용되는 특정 열에 대한 정렬된 데이터 구조를 말합니다.

* 데이터베이스 엔진은 이를 참조하여 효율적으로 데이터에 접근 할 수 있습니다.
* 인덱스는 주로 SELECT 쿼리에서 WHERE나 JOIN 절에서 사용되어 검색 조건을 만족하는 행을 찾는데 도움이 됩니다.

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption><p><a href="https://mangkyu.tistory.com/96">https://mangkyu.tistory.com/96</a></p></figcaption></figure>

### Index는 많은게 좋을까 적은게 좋을까?

저는 인덱스는 기본적으로 적합한 열에만 **적을 수록** 좋다고 생각합니다.

인덱스를 사용하지 않는 조회를 해야 하는 상황이라면 테이블전체를 탐색하는 Full Scan을 해야하므로, 인덱스를 설정해주는 것은 꼭 필요한 작업이라고 할 수 있습니다.

#### 하지만 이렇게 좋은 일을 하는 인덱스가 왜 적을 수록 좋다고 하였을까요?&#x20;

* 그 이유는 인덱스도 **생성** **및** **유지 비용**이 많이 들기 때문인데요.
  * **인덱스 생성 비용**
    * 인덱스는 원하는 값을 빠르게 탐색하기 위해 항상 최신의 정렬된 상태를 유지해야 합니다.
    * 인덱스가 적용된 컬럼에 데이터 변경 시 각각INSERT(인덱스 추가), DELETE(인덱스  사용하지 않음), UPDATE(인덱스 사용하지 않음 및 추가)의 작업을 진행하여야 합니다.
  * **인덱스 유지 비용**
    * 인덱스를 저장하기 위해선 테이블의 약 10% 정도의 디스크를 추가로 사용하여야 합니다.

### Index Scan 방식 종류

#### Index Range Scan

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="341"><figcaption></figcaption></figure>

* 인덱스 루트에서 리프 블록까지 수직적 탐색을 하고, 필요한 범위만큼 수평적 탐색하는 스캔 방식입니다.
* 선두 컬럼을 가공하지 않은 상태로 조건 절에 사용해야 합니다.
* 해당 스캔 방법이 가장 흔히 사용하는 인덱스 조건입니다.

#### Index Unique Scan

<figure><img src="../../../.gitbook/assets/image (51).png" alt="" width="328"><figcaption></figcaption></figure>

* 수직적 탐색으로만 데이터를 찾는 방식입니다.
* 값이 하나만 존재하는 것을 보장하기 때문에 1번만 조회합니다.
* 따라서 등치(=) 조건으로 탐색하는 경우에 작동합니다.
* 예를 들면 PK로 조회할 때 사용됩니다.

#### Index Skip Scan

<figure><img src="../../../.gitbook/assets/image (52).png" alt="" width="328"><figcaption></figcaption></figure>

* 조건 절에 첫 번째 인덱스가 없어도 두 번째 인덱스 만으로 인덱스를 검색 할 수 있게 해주는 기능입니다.
* MySQL 8.0 버전에 인덱스 스킵 스캔 최적화 기능이 도입되면서 옵티마이저가 A 컬럼을 건너뛰어서 B컬럼만으로도 인덱스 검색이 가능하게 되었습니다.
* 단점으로는 첫 번째 인덱스의 유니크한 값의 개수가 적어야 하고,
* 쿼리가 인덱스에 존재하는 컬럼만으로 처리가 가능해야합니다.
* [참고하기 좋은 자료](https://github.com/woowacourse-study/2022-Real-MySQL/issues/18)

#### Index Full Scan

<figure><img src="../../../.gitbook/assets/image (53).png" alt="" width="375"><figcaption></figcaption></figure>

* 수직 탐색 없이 리프 블록 처음부터 끝까지 수평적으로 탐색하는 방식입니다.
* 보통 데이터 검색을 위한 최적의 인덱스가 없을 때 차선으로 선택됩니다.
* 인덱스 풀 스캔 후 필터 된 데이터를 대상으로 테이블에 엑세스 하는 것이 효율적입니다.

#### Index Fast Full Scan

<figure><img src="../../../.gitbook/assets/image (54).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

* 인덱스 테이블의 특정 부분만을 블록 단위로 스캔을 합니다.
* Index Full Scan과의 가장 큰 차이점은 정렬을 보장하지 않지만 병렬 읽기가 가능하다는 것입니다.
* 따라서 상대적으로 속도가 빠릅니다.
* 예를 들어, 2024년에 생성된 데이터만을 찾고 싶을 경우 인덱스 풀 스캔 보다는 Index Fast Full Scan으로 일정 부분의 블럭만을 스캔하는 것이 빠를 수 있습니다.

#### Index Merge Scan

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

* 여러 개의 인덱스를 병합하여 검색하는 방법입니다.
* 병합 방법으로는 합집합(union), 교집합(intersection) 방식이 있습니다.
* 단점으로는 인덱스의 크기가 작거나 데이터의 분포가 특정한 경우에 유용합니다.
* 데이터 테이블의 Access 비용을 줄일 수 있습니다.
  * 예를 들자면, A 인덱스 테이블에서 결과 값이 1, 2, 3 이 나오고 B 인덱스 테이블에서 2, 3, 4가 나오면,
    * union에선 1,2,3,4의 4번의 테이블 Access 비용만이 필요하게 되는데,
    * 만약 각각의 인덱스를 별개로 태운다면 1, 2, 3 / 2, 3, 4 총 6번의 Access 비용이 필요할 것 입니다.
* 단, 각 병합에는 인덱스의 양이 많을 경우 병합에 대한 오버헤드가 더 클 수 있어 다음 방식을 시도해 볼 수 있습니다.
  * SQL 문 자체를 독립된 하나의 인덱스만 사용하는 방법을 선택합니다.
  * 만약 위의 방법이 안된다면 별개로 생성된 인덱스들을 하나의 인덱스로 통합합니다.

#### Index Loose Scan

<figure><img src="../../../.gitbook/assets/image (57).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption><p><a href="https://12bme.tistory.com/138">https://12bme.tistory.com/138</a></p></figcaption></figure>

* 인덱스의 필요한 부분들만 골라서 스캔하는 방식입니다.
* 즉, 넓은 범위에 전부 접근하지 않고 WHERE 조건문 기준으로 불필요한 데이터의 인덱스 키는 무시합니다.
* GROUP BY, MAX(), MIN() 함수 포함 시 동작합니다.
* Index Loose Scan 불 충족 예제
  * 다른 집계 함수가 들어가 있는 있는 경우.
    * ```sql
      SELECT c1, SUM(c2) FROM t1 GROUP BY c1;
      ```
  * Group by 요소 중 인덱스의 첫 번째 요소가 들어가 있지 않은 경우.
    * ```sql
      SELECT c1, c2 FROM t1 GROUP BY c2, c3;
      ```
  * Group By 부분에 필요한 컬럼 들이 충분하지 않을 경우.
    * ```sql
      SELECT c1, c3 FROM t1 GROUP BY c1, c2; // WHERE에 c3 추가하면 가능
      ```

### Hash Table

* 컬럼의 값으로 해시 값을 계산해서 인덱싱하는 알고리즘으로 매우 빠른 검색을 지원합니다.
* 하지만 값을 해싱하여 저장하기 때문에 한글자라도 바뀌면 전혀 다른 값이 되므로, 특정 문자로 시작하는 값 등을 검색할 경우 해시 인덱스를 사용할 수 없습니다.
* 위와 마찬가지의 이유로 = 동등 연산에는 특화되었지만 <,> 등의 부등호 연산에도 해시 인덱스를 사용할 수 없습니다.

### B-Tree

DB와 파일 시스템에서 널리 사용되는 Self-balanced Tree입니다.

* 노드에는 2개 이상의 데이터가 들어갈 수 있으며 항상 정렬된 상태로 저장됩니다.

<figure><img src="../../../.gitbook/assets/image (59).png" alt=""><figcaption><p><a href="https://code-lab1.tistory.com/217">https://code-lab1.tistory.com/217</a></p></figcaption></figure>

* 최대 M개의 자식을 가질 수 있는 B 트리를 M차 B트리라고 하는데, 내부노드는 최소 ceil(M/2)개 부터 최대 M개의 자식을 가질 수 있습니다. (ceil() 은 소수점 무조건올림)
*

    <figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption><p><a href="https://code-lab1.tistory.com/217">https://code-lab1.tistory.com/217</a></p></figcaption></figure>
* 특정 노드의 데이터가 K개 라면 자식 노드의 개수는 K+1개 여야 합니다.

<figure><img src="../../../.gitbook/assets/image (61).png" alt=""><figcaption><p><a href="https://code-lab1.tistory.com/217">https://code-lab1.tistory.com/217</a></p></figcaption></figure>

* 왼쪽 서브트리보다 오른쪽에 위치한 서브트리는 항상 더 큰 값들로 구성됩니다.
* 내부노드 내 데이터는 수는 항상 최소 ceil(M/2) - 1개부터 최대 M - 1개 이여야 합니다.

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption><p><a href="https://code-lab1.tistory.com/217">https://code-lab1.tistory.com/217</a></p></figcaption></figure>

* 균형다진트리로 모든 리프 노드들은 같은 레벨에 존재해야 합니다. (깨지면 다시 균형을 유지)
*

    <figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption><p><a href="https://code-lab1.tistory.com/217">https://code-lab1.tistory.com/217</a></p></figcaption></figure>
* 이렇듯 구조를 유지하기 위해서는 추가적인 연산이 수행되거나 새로운 노드가 생성이 되는 단점이 존재합니다.
* **B-tree의 삽입 분할 삭제 과정**
  * [https://velog.io/@chanyoung1998/B%ED%8A%B8%EB%A6%AC](https://velog.io/@chanyoung1998/B%ED%8A%B8%EB%A6%AC)

### B+Tree

<figure><img src="../../../.gitbook/assets/image (64).png" alt=""><figcaption><p><a href="https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/">https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/</a></p></figcaption></figure>

* leaf node에 모든 자료들이 존재하고, 그 자료들이 연결 리스트로 연결되어 있어 탐색에 매우 유리합니다.
* 따라서, 데이터 검색을 위해서는 반드시 leaf node까지 내려가야 한다는 특징을 가지고 있습니다.
* leaf node가 아닌 자료는 **인덱스 노드**라고 부르며, Value  값에 다음 노드를 가르키는 포인터 주소가 존재합니다.
* leaf node 는 **데이터 노드**라고 부르며, Value값에 데이터가 존재합니다.
* 데이터베이스에서 가장 중요한 것은 **검색 속도** 이므로 대부분의 현대 데이터베이스 시스템은 B+Tree구조를 채택하고 있습니다.

### B\*Tree

* &#x20;B-Tree에서 자식 노드 수 최소 제약 조건이 2M/3개로 늘어났습니다.
* 노드가 가득하면 분열 대신 이웃한 형제 노드로 재배치 합니다.
* 더 이상의 재배치가 불가한 경우에 분열합니다.

### Clustered Index 란?

데이터가 테이블에 클러스터형 인덱스의 키 순서로 물리적으로 정렬됩니다.

<figure><img src="../../../.gitbook/assets/image (65).png" alt=""><figcaption><p>Clustered 인덱스의 구조</p></figcaption></figure>

* Leaf level 과 DataPage가 동일한 구조를 갖습니다.
* 테이블 데이터는 오직 한 가지의 방법으로 정렬되기 때문에 테이블 당 하나의 클러스터 형 인덱스만 존재 할 수 있습니다.
* Clustred는 정렬을 위해 중간에 새로운 데이터가 삽입된다면 이후의 모든 컬럼을 한칸씩 이동시켜줘야 합니다.
* 1\~10000까지의 id가 있을경우 중간에 2를 새로 삽입한다면 기존의2\~10000까지의 컬럼을 순차적으로 1씩 증가시켜 이동 시켜야 합니다. (물론 상황에 따라 전체를 전부 밀지는 않을 수도 있습니다. B트리라면 분열 등)
*

    <figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption><p><a href="https://gwang920.github.io/database/clusterednonclustered/">https://gwang920.github.io/database/clusterednonclustered/</a></p></figcaption></figure>
* 검색속도는 향상시키는 장점이 있지만, 이처럼 새로운 데이터를 삽입할 때 많은 비용이 소모되는 단점이 존재합니다.
* 항상 정렬 된 방식으로 데이터를 반환해야 하는 경우에 사용하면 좋고, (Order By 대신)
* 자주 업데이트 되지 않는 테이블에서 사용하는 것이 좋아주로 읽기 작업이 많은 경우에 사용하면 좋습니다.

### Clustered Index를 명시적으로 설정하지 않으면 어떻게 될까요?

* 보통PK를 자동 생성하여 사용한다면 데이터베이스에서는 PK가 클러스터 된 인덱스를 자동으로 생성하기 때문에 PK가 클러스터 인덱스가 됩니다.
* InnoDB 스토리지 엔진을 사용하는 경우, 기본 키는 클러스터 인덱스로 설정됩니다.
* 테이블에 PK를 명시적으로 정의하지 않고 테이블을 생성하는 경우, 테이블은 클러스터 인덱스 없이 생성될 수 있습니다.
* Clusterd Index없는 테이블은 힙 구조로 데이터가 저장됩니다.

### Nonclusterd Index 란?

<figure><img src="../../../.gitbook/assets/image (67).png" alt=""><figcaption><p>논 클러스터 인덱스 구조</p></figcaption></figure>

* Clustered 구조와는 다르게 Leaf level과 DataPage가 따로 구분이 되어 있으며,  Data Page의 데이터는 정렬 되어있지 않습니다.
* 순서와 상관이 없으며 한 테이블에 여러개를 생성할 수 있습니다.
* Index를 저장할 테이블의 약 10%정도의 추가적인 공간이 필요합니다. (인덱스가DataPage에 포함되지 않음)
*

    <figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption><p>논 클러스터형 예시</p></figcaption></figure>

### Clusterd와 Nonclusterd를 같이 사용할 경우

DB를 사용하다 보면 클러스터 인덱스와 논 클러스터 인덱스를 같이 사용하는 경우가 많은데요. (PK + 기타 인덱스) 이런 경우에 어떻게 검색이 효과적으로 이루어지는지 간단히 설명을 드리겠습니다.

<figure><img src="../../../.gitbook/assets/image (69).png" alt=""><figcaption><p>논 클러스터 조회 -> PK 얻음-> 클러스터 인덱스 조회</p></figcaption></figure>

먼저  논 클러스터 인덱스로만 조회한 쿼리를 실행 시키게 되면,

* 해당 쿼리에서 사용된 논 클러스터 인덱스로 우선 빠르게 데이터를 찾아냅니다.
* 하지만논 클러스터 인덱스 테이블은 리프노드에 데이터를 가지고 있지 않는데,&#x20;
* 이 리프노드들은 실제 데이터 위치가 아닌 클러스터 인덱스(기본키)의 포인터를 저장하고 있게 됩니다.
* 이로써찾아낸 클러스터 인덱스로 빠르게 실제 데이터 페이지를 조회 할 수 있게 됩니다.
* 이는 클러스터 인덱스에서만 물리적으로 정렬하여 데이터를 저장할 수 있게 되고,
* 나머지 논클러스터 인덱스 테이블들에서는 데이터가 변경 시 각 테이블마다 데이터를 정렬하지 않아도 되는 장점을 가질 수 있습니다.

#### 📚 참고자료

* [https://hudi.blog/db-clustered-and-non-clustered-index/](https://hudi.blog/db-clustered-and-non-clustered-index/)
* [https://so-what-93.tistory.com/81](https://so-what-93.tistory.com/81)

### 인덱스 실행 계획 확인하는 방법

#### EXPLAIN 방법

ex) `EXPLAIN SELECT * FROM your_table WHERE your_condition;`

* EXPLAIN은 쿼리 실행 계획을 분석하고 최적화 하기 위한 명령어입니다.
* 해당 명령어를 아래와 같이 쿼리 문 앞에 붙여주면 MySQL이 쿼리를 실행하는 방식과 사용하는 인덱스, 테이블의 읽기 순서 등에 대한 정보를 얻을 수 있습니다.
  * **possible\_keys**는 MySQL의 옵티마이저가 쿼리에서 사용할 수 있는 인덱스의 모음을 확인할 수 있고,
  * **key**는 해당 쿼리에서 실제로 사용한 인덱스를 확인 할 수 있습니다.



## 📜 로그

### Binary log란? (binlog)

* 이진 로그 라고도 불리며 데이터베이스 시스템에서 수행된 변경 사항을 기록하는 파일입니다.
* 기본적으로 Transaction Commit 시에 기록되어지며 데이터 변경 순서를 보장한다는 특징이 있습니다.
* 에러코드, 바이너리 로그 자체에 대한 메타데이터 등 다양한 데이터가 포함됩니다.
* 원래는 Master DB서버를 Slave DB서버와 Replication하기 위한 것이 주목적 이였다고 합니다.

### Binary log는 언제 사용할까?

1. **복구 (Recovery)**
   * 데이터베이스에서 문제가 발생했을 때 이진 로그를 사용하여 이전 상태로 복구할 수 있습니다.
   * 데이터베이스의 일관성을 유지하고 손상된 데이터를 복구 할 수 있습니다.
   * 복구 방법 예시 참고([https://hudi.blog/mysql-pit-recover/](https://hudi.blog/mysql-pit-recover/))
2. **복제 (Replication)**
   * 데이터베이스의 변경 사항을 다른 서버로 전송하여 레플리케이션 등을 구현하는데 사용됩니다.
   * 이를 통해 여러 서버 간에 데이터를 동기화 하고 가용성도 향상 시킬 수 있습니다.

### Transaction log 란?

* 트랜잭션이 커밋될 때 마다 기록이 되며 보통사용자가 읽을 수 있는 형태로 기록됩니다.
* 트랜잭션에 대한 처리, 롤백, Crash Recovery, Point In Time Recovery 등을 위한 용도로 사용됩니다.
* 트랜잭션 로그는 데이터 변경의 일관성을 유지하고 데이터베이스의 장애 발생 시 복구하는 데 중점을 두고 있습니다.

## 📦 트랜잭션

* 트랜잭션은 데이터의 일관성과 무결성을 보장하며 다중 클라이언트와 같이 병렬적으로 발생하는 작업에서 발생할 수 있는 데이터 부정합을 방지하고자 할 때 사용됩니다.
* 일련의 수행을 하나의 논리적인 단위의 묶음으로 처리하며 정상적으로 성공하였을 경우에는 모든 변경사항을 저장하고,
* 어떠한 이유들로 인하여 중간에 비정상적인 종료가 되었을 경우 모든 변경사항을 처음과 동일한 상황으로 돌아갑니다.

### :arrow\_forward: 트랜잭션 4가지 특성(ACID)

#### 원자성(Automicity)

* 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행되던지 모두 전혀 실행되지 않은 상태로 남아 있어야 합니다. (전부 성공 또는 전부 롤백)

#### 일관성(Consistency)

* 트랜잭션 시작 전과 완료 후에도 데이터 베이스가 일관된 상태로 유지되어야 합니다.

#### 격리성(Isolation)

* 모든 트랜잭션은 다른 트랙잭션으로부터 독립되어 있어야 합니다.
* 즉, 여러개의 트랜잭션이 실행될 때 서로에게 영향을 끼치면 안되고, 연속으로 실행 된 것과 동일한 결과를 나타내야 합니다.

#### 지속성(Durability)

* 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장된다.
* 트랜잭션이 성공적으로 완료 되었을 경우, 시스템 장애 또는 기타 문제가 발생하더라도 트랜잭션의결과는 보존되어야 합니다.

### TCL이란?

* Transaction Control Language의 약자로 DBMS에서 트랜잭션을 제어하기 위한 언어입니다.
* 주로 트랜잭션의 관리, 커밋, 롤백 등의 관련된 작업을 수행합니다.
* 명령어로는 COMMIT, ROLLBACK, SAVEPOINT 가 있습니다.
  * COMMIT: 트랜잭션의 변경사항을 영구적으로 데이트베이스에 적용하고 종료합니다.
  * ROLLBACK: 트랜잭션의 모든 변경 사항을 취소하고 이전(기존) 상태로 되돌립니다.
  * SAVEPOINT: 트랜잭션을 잘게 분할하여 트랜잭션 내에서 지정한 곳까지 롤백 할 수 있습니다.
