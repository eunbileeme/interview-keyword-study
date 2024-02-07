# 최지율

## 🍕 기타

### MySQL을 사용하신 이유는 무엇인가요?

* 저는 MySQL을 전공에서 배운 것이 다였고, 전 회사에서는 SQL-Server나 PostgreSql을 사용하였지만 토이프로젝트에서라도 한번 대중적으로 유명한 DB를 사용해보는 것도 좋은경험이라고 생각하여 사용하게 되었습니다.

### &#x20;버전을 어떤 걸 사용하셨을까요?

* 저는 UTF8MB4 캐릭터 셋이 기본 문자셋으로 설정되어 있는 버전인 **MySQL 8.0**을 사용하였습니다.
  * 이모지 등을 저장하기 위해서. (물론 다른 버전도 UTF8MB4을 지정하면 사용가능)

### 가장 큰 버전 별 특징은 어떤 것이 있을까요?

#### **MySQL 5.7 (2015년):**

* JSON 데이터 형식이 지원되고 Generated Columns과 같은 기능 추가 되었습니다.
* Information Schema를 사용하여 메타데이터를 관리하는 마지막 버전입니다.

#### **MySQL 8.0 (2018년):**

* Information Schema를 대체하는 InnoDB의 내부 데이터 딕셔너리를 활용하여 성능 및 확장성 향상
* SQL 쿼리에서 윈도우 함수의 지원 추가되었습니다. (**Window Functions)**
  * 예: ROW\_NUMBER(), RANK(), DENSE\_RANK(), OVER() 등
* WITH 절을 사용한 재귀 쿼리를 지원합니다.
* UTF8MB4 캐릭터 셋이 기본 문자셋으로 변경되었습니다.
  * MySQL 5.5.3이상부터 utf8mb4 문자 셋을 사용하면 이모지와 같은 4-byte 문자 지원을 하였지만, 이제 utf8mb4를 기본으로 변경하였습니다.
* 인덱스의내림차순을 지원합니다.
  * MySQL 5.7 버전에서는 내림차순 인덱스를 만들기 위해 먼저 오름차순 인덱스를 생성하고 역순으로 검색해야 합니다.
* 외래키의 길이가 최대 64자로 제한되었습니다.

MySQL이 대중화된 이유는 무엇일까요?

* 오래전부터 존재하던 오픈 소스로 많은 기업 및 개발자들에게 비용을 절감하면서도 안정적이고 강력한 기회를 제공하였습니다.
* 그리고 다양한 운영체제와 프로그래밍 언어에서의 호환성이 높아 개발자들이 쉽게 채택할 수 있었습니다.
* 물론PostgreSql도 오래전부터 존재하였지만 그 때 당시에는 개발자들에게 MySQL가 더 강세였고 그로인해오라클에 인수당하기 전까지 많은 개발자들에 의해 커뮤니티 기여와 오픈소스 생태계를 통해 빠르게 발전할 수 있었습니다.&#x20;
* 따라서 많은 사용자로부터 검증된 안정성과 지속적인 발전으로 인해 현재까지도 많이 사용되고 있습니다.

## 🧱 정규화

### 정규화를 시키는 이유가 무엇일까요?

* 데이터의 중복을 최소화 하여 DB에 저장 할 때 저장되는 용량을 줄일 수 있고,
* 데이터 변경 시 한 곳에서만 수정하여 데이터의 일관성을 지키는 장점을 얻습니다.
* 때문에 정규화를 하면 유지보수와 쿼리성능을 최적화 할 수 있습니다.

### **정규화 1, 2, 3형에 대해 설명해주세요.**

#### <mark style="color:blue;">**제 1정규화**</mark> 는 모든 컬럼의 값은 원자값을 가지게 해야합니다.

* 여기서 원자값이란 컬럼에는 하나의 단일 값만 존재해야 합니다.
* 예를 들어

| 이름 | 과목          |
| -- | ----------- |
| 철수 | 생명과학, 지구과학  |

* 위와 같이 하나의 컬럼에는 여러 값을  가지고 있을 경우 아래와 같이 바꾸어야 합니다.

| 이름 | 과목   |
| -- | ---- |
| 철수 | 생명과학 |
| 철수 | 지구과학 |

#### <mark style="color:blue;">**제 2정규화**</mark> 는 1정규화를 충족하면서, 완전 함수 종속이 만족되어야 합니다.

* 여기서 완전 함수 종속이란 어떠한 컬럼이 기본키의 일부에만 종속이 되면 안됩니다.
* 즉, 복합키에서 주로 발생하며 단일 키의 경우 자동으로 조건을 만족합니다.
* 예를 들어, 복합키(이름, 과목)를 사용하는 테이블에서

| 이름 | 과목 | 교실    | 성적  |
| -- | -- | ----- | --- |
| 짱구 | 국어 | 해바라기반 | 100 |
| 치타 | 영어 | 장미반   | 70  |

* 일 때 복합키(이름,과목)로 성적을 결정하지만,
* **교실은** **기본키 중 과목에만 종속** 되므로 충족 되지 않습니다.
* 따라서 다음과 같이 나누어야합니다.

| 이름 | 과목 | 성적  |
| -- | -- | --- |
| 짱구 | 국어 | 100 |
| 치타 | 영어 | 70  |

| 과목 | 교실    |
| -- | ----- |
| 국어 | 해바라기반 |
| 영어 | 장미반   |

#### <mark style="color:blue;">**제 3정규화**</mark> 는 2정규화를 충족하면서, 이행적 종속이 되면 안됩니다.

* 여기서 이행적 종속은 한 테이블에서 A ->B이고 B->C 이므로 A->C가 되는 것은 안된다는 것 입니다.
* 쉽게 말해서 기본키가 아닌 컬럼에 종속이 되면 안됩니다.
* 예를 들어

| 유치원생 ID | 이름 | 과목 | 선생님 | 성적  |
| ------- | -- | -- | --- | --- |
| 1       | 짱구 | 국어 | 채성아 | 90  |
| 2       | 맹구 | 과학 | 채성아 | 80  |
| 3       | 철수 | 영어 | 나미리 | 100 |

* 위테이블에서 모든 ID는 PK에 종속적이지만
* **강사는 과목에도 종속**적이므로 만족하지 않습니다.
* 따라서 다음과 같이 두 테이블로 정규화가 되어야 합니다.

| 유치원생 ID | 이름 | 과목 | 성적  |
| ------- | -- | -- | --- |
| 1       | 짱구 | 국어 | 90  |
| 2       | 맹구 | 과학 | 80  |
| 3       | 철수 | 영어 | 100 |

| 과목 | 선생님 |
| -- | --- |
| 국어 | 채성아 |
| 과학 | 채성아 |
| 영어 | 나미리 |



### 복합키의 장단점에 대해서 설명해주세요.

#### 장점

* 특정 데이터를 식별하는게 큰 의미가 없을 경우에 사용할 경우 도움이 될 수 있습니다.
  * 예를들어 통계성 데이터들을 저장할 경우에 사용해도 좋습니다.
  * 복합키를 통한 조회로 인덱스를 잘 활용하여 일부 쿼리에서는 성능향상을 가져올 수 있습니다.

#### 단점

* JPA는 복합키 생성 시 컬럼명의 알파벳 순으로 생성합니다.
  * Entity class에 정의된 순서대로 생성되는 것이 아니기 때문에
  * 기대했던 PK Index가 타지 않을 가능성이 높습니다.
* 쿼리 작성이나 인덱스 관리 등에 더 많은 주의가 필요합니다.
* FK를 맺을 때 불필요하게 많은 키가 맺어져야합니다.
*

참고하면 좋을 자료 : [https://prohannah.tistory.com/99](https://prohannah.tistory.com/99)

##

## 🪶NOSQL

### NOSQL이란 무엇일까요?

* Not Only SQL이란 뜻으로 관계형 데이터베이스 모델을 기반으로 하지 않는다는 뜻으로 테이블 간에 강력한 관계를 가지지 않습니다.
* 또한 고정된 스키마를 가지지 않고 샤딩(Sharding)이라는 기술로여러 노드로 분산하여 데이터베이스를 관리하므로&#x20;
* 수평적으로 확장이 가능하도록 설계가 되어있습니다.

### NOSQL의 장점이 무엇일까요?

* 데이터에 따라 다양한 모델의 형태로 저장하고 검색할 수 있습니다.
  * Key-Value, Document-Oreinted(Json), Column-Family, Graph 등등
* 대량의 읽기나 쓰기 연상이 필요한 경우에 RDB에 비하여 더 유리합니다.
* 필요에 따라 서버를 추가하여 수평적으로 확장하기에 편리합니다.
* RDB에 비해 기존의 필드를 삭제하거나 변경하는 것이 쉽게 가능하여 유연합니다.

### DML에 대해 설명해주시겠어요?

* **데이터 조작 언어**(Data Manipulation)로 **데이터의 검색, 삽입, 갱신, 삭제하는 작업**을 수행합니다.
* 테이블 내의 레코드를 조작하고 데이터를 변경하는 데 사용됩니다.
* 주요 명령어로는 SELECT, INSERT, DELETE 등이 있습니다.

### DDL에 대해 설명해주시겠어요?

* **데이터 정의 언어**(Data Definition Language)로 **데이터베이스의 구조를 정의하거나 변경**합니다.
* 테이블, 인덱스, 뷰 등 데이터베이스 구조를 정의하고 조작하는데 사용됩니다.
* 주요명령어로는 CREATE, ALTER, DROP 등이 있습니다.

### DCL에 대해 설명해주시겠어요?

* **데이터 제어 언어**(Data Control Language)로 **권한을 관리**합니다.
* 사용자에게 어떤 종류의 권한을 부여하거나 회수하는데 사용됩니다.
  * 이를 통해 불법 사용자들을 차단하여 데이터를 보호합니다.
* 주요 명령어로는 GRANT(권한부여), REVOKE(권한취소) 등이 있습니다.